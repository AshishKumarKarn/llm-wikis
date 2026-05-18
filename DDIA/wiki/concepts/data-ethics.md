---
title: Data Ethics (Doing the Right Thing)
type: concept
chapters: [12]
tags: [ethics, privacy, surveillance, predictive-analytics]
status: solid
updated: 2026-05-16
---

# Data Ethics (Doing the Right Thing)

Every system is built for a purpose with intended *and unintended* consequences;
engineers bear ethical responsibility. Much data is about **people** — treat it with
humanity and respect. *(DDIA Ch 12)*

## Predictive analytics

Predicting weather is one thing; predicting recidivism, loan default, or
insurance/employment risk **directly affects lives**. Systematic "no" decisions →
"algorithmic prison" (excluded from jobs/credit/housing with no proof of guilt, little
appeal — vs. presumption of innocence).

- **Bias & discrimination**: learned patterns are opaque; systematic input bias is
  learned and **amplified**. Proxies (postal code/IP ≈ race) defeat
  anti-discrimination law — "ML is like money laundering for bias". Predictive
  systems extrapolate the past; if the past is discriminatory they codify it. Data &
  models should be tools, not masters.
- **Responsibility & accountability**: who is accountable when an algorithm errs
  (self-driving crash, discriminatory credit scoring)? Can you explain a decision to
  a judge? Credit scores = "how *you* behaved"; predictive analytics = "how people
  *like you* behaved" → stereotyping; statistical correctness ≠ correct in individual
  cases; erroneous-data recourse near-impossible.
- **Feedback loops**: self-reinforcing (credit score → joblessness → worse score).
  Predict via **systems thinking** (include the humans). Does the system amplify
  inequality or combat injustice? Beware unintended consequences.

## Privacy & surveillance

Data collected as a *side effect* (not user-requested) → the service takes on its own
interests; if ad-funded, **users' data is the core asset**, users are the product.
Thought experiment: replace "data" with "**surveillance**" — we've built the greatest
mass-surveillance infrastructure ever, voluntarily.

- **Consent is largely meaningless**: users can't understand derived datasets;
  one-sided non-reciprocal relationship; opting out of a de-facto-mandatory service
  isn't free (network effects, privilege).
- **Privacy** = the *decision right* to choose what to reveal to whom — not secrecy.
  Surveillance **transfers** that right from individual to corporation, which keeps
  it secret (creepy) for profit.
- **Data as a toxic asset/hazardous material**: wanted by companies, governments
  (deals/coercion/theft), sold in bankruptcy, leaked in breaches. Consider *all
  future* governments — "poor civic hygiene to install technologies that could
  facilitate a police state". Knowledge is power; scrutinizing others while avoiding
  scrutiny is power.

## Industrial-revolution analogy & regulation

"Data is the pollution problem of the information age; protecting privacy is the
environmental challenge" (Schneier). Like factory regulation, safeguards will be
costly but worth it. Existing data-protection law (collect for specified purposes,
not excessive) runs counter to Big Data's explore-everything philosophy; updated
regulation is emerging. Needed: a **culture shift** — users are humans deserving
dignity/agency, self-regulate, educate users, don't retain data forever (purge when
done), enforce access control cryptographically not just by policy. Ubiquitous
surveillance is not inevitable.

## Related concepts

- [[state-streams-immutability]] (limits of immutability — purging for privacy) ·
  [[systems-of-record-and-derived-data]] · [[reliability]] · [[maintainability]]

## Sources

DDIA Ch 12 ("Doing the Right Thing"). Refs: Schneier; O'Neil *Weapons of Math
Destruction*-adjacent themes; ACM Software Engineering Code of Ethics.
