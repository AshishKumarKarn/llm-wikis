# Primer on Consistent Hashing

*Seed source — written to demonstrate the ingest pipeline. Replace with your own sources
(use Obsidian Web Clipper, paste articles, drop PDFs, etc.).*

## The problem

Suppose you run a distributed cache across N servers. The naive way to decide which server
holds a key is `server = hash(key) % N`. This works until N changes. Add or remove one
server and almost every key now maps to a different server — `% 4` and `% 5` agree on
almost nothing. The result is a cache stampede: nearly the entire cache misses at once and
the backing database is hammered. The same problem appears in sharded databases and any
partitioned store.

## Consistent hashing

Map both servers and keys onto the same circular keyspace (a "hash ring", e.g. integers
0 to 2^32−1). To find the server for a key, hash the key to a point on the ring and walk
clockwise to the first server you hit.

When a server is added, it takes over only the keys between it and the previous server on
the ring — roughly 1/N of keys move, not all of them. When a server is removed, only its
keys move, to the next server clockwise. On average a membership change relocates about
**K/N keys** (K = total keys), versus nearly all keys with modulo hashing.

## Virtual nodes

Naive consistent hashing has a load-balance problem: with few servers, ring positions are
uneven and one server can own a much larger arc than another. The fix is **virtual nodes**:
each physical server is placed at many positions on the ring (e.g. 100–200 tokens per
server). This smooths the distribution and also lets heterogeneous servers carry weight
proportional to capacity (a bigger box gets more virtual nodes).

## Where it's used

Consistent hashing is the partitioning scheme behind Amazon Dynamo, Apache Cassandra,
Riak, and memcached client libraries (ketama). It is a foundational technique for
horizontally scaling stateful systems.

## Key trade-offs

- Reduces key remapping on resize from ~100% to ~1/N — the whole point.
- Virtual nodes are required in practice for even load.
- It solves *placement*, not *replication* or *consistency* — those are separate concerns
  layered on top (e.g. replicate to the next R nodes clockwise).
