---
title: JVM Debugging — Heap Dumps, Thread Dumps, GC Logs
type: concept
tags: [jvm, debugging, heap-dump, thread-dump, gc, memory-leak, concepts]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# JVM Debugging — Heap Dumps, Thread Dumps, GC Logs

Three complementary tools for diagnosing JVM production issues. Always use all three
together (see [[sources/scenario-questions-study-guide]]).

## The three tools

| Tool | What it captures | Used for |
|---|---|---|
| **GC Logs** | Memory behavior over time | Detect patterns: leak, high allocation, GC storms |
| **Heap Dump** | Snapshot of all objects in memory | Find what's consuming memory; OOM root cause |
| **Thread Dump** | Snapshot of all running threads | Deadlocks, high CPU, application hangs |

## Heap Dump

### When to capture
- OutOfMemoryError
- Heap usage trending up after GC (memory leak signal)
- High memory usage with no clear cause

### How to capture
```bash
# Preferred (modern JVMs)
jcmd <pid> GC.heap_dump heap.hprof

# Traditional
jmap -dump:format=b,file=heap.hprof <pid>

# Docker/Kubernetes
docker exec -it <container_id> jcmd 1 GC.heap_dump /tmp/heap.hprof
kubectl exec -it <pod> -- jcmd 1 GC.heap_dump /tmp/heap.hprof
kubectl cp <pod>:/tmp/heap.hprof .

# Automatic on OOM (enable in production always)
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=/path/to/dumps/
```

### Analysis tools
- **Eclipse MAT** — deep analysis; Leak Suspects report; Dominator Tree; handles multi-GB.
- **VisualVM** — quick exploration; object counts; memory usage.
- **HeapHero** — web-based upload; visual reports; good for sharing across teams.

### Key concepts
- **Shallow Heap:** memory occupied by the object itself only (references, not referenced objects).
- **Retained Heap:** total memory freed if the object is GC'd — includes all objects only
  reachable through it. **This is the number that matters for leak detection.**
- **Dominator Tree:** shows which objects hold the most retained heap — start here.

```
class A { B b = new B(); }
class B { int[] arr = new int[1000]; }

Shallow Heap:  A = 16 bytes, B = 24 bytes
Retained Heap: A = 16 + 24 + 4000 = ~4040 bytes
```

## Thread Dump

### When to capture
- Application hangs or freezes
- Deadlock suspected
- High CPU usage (threads spinning)
- Requests timing out

### How to capture
```bash
# Safest — no JVM interruption, output to app logs
kill -3 <pid>

# Controlled output to file
jstack <pid> > threaddump.txt

# Docker/Kubernetes
docker exec -it <container_id> kill -3 1
kubectl exec -it <pod> -- kill -3 1

# Best practice: take 3-5 dumps at 10s intervals to see patterns
kill -3 <pid> && sleep 10 && kill -3 <pid> && sleep 10 && kill -3 <pid>
```

**Golden rule:** `kill -3` first — safest, no extra tools, minimal overhead.

### Analysis
VisualVM can't load external thread dump files — use **fastThread.io** for offline analysis.
Look for: BLOCKED threads, deadlocks, repeating stack traces.

## GC Logs

### Enable in production (always)
```bash
# Java 9+
-Xlog:gc*:file=gc.log:time,uptime,level,tags

# Java 8
-XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:gc.log

# Log rotation (required)
-Xlog:gc*:file=gc.log:time:filecount=5,filesize=10M
```

### Analysis tools
- **GCeasy** — upload; visual report; pause times, memory trends, GC frequency.
- **GCViewer** — offline graphs.
- **VisualVM** — correlate GC with CPU/memory.

### Warning signals
- Long GC pauses (> 1–2s STW)
- Frequent minor GC cycles
- Full GC repeatedly
- Heap not reducing after GC (`Before: 80%` → `After: 75%` → memory leak)

## Combined workflow (production)
```
1. Alert triggers (high GC pause or memory)
2. Check GC logs → identify pattern (time window)
3. Take heap dump at peak memory
4. Analyze with MAT → Dominator Tree, GC roots
5. Correlate: GC logs (WHEN) + heap dump (WHAT)
6. Fix: memory leak / GC tuning
```

**GC logs = "when did it happen?" | Heap dump = "what caused it?"**

## Related
[[scenarios/production-incident-response]] · [[components/elk-stack]] ·
[[concepts/distributed-tracing]]
