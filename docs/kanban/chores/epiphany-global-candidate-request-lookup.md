---
slug: epiphany-global-candidate-request-lookup
uuid: bdd5f047-76f6-4e7b-9196-e24366f30e31
title: "Reject candidate request reuse across registered resources"
status: incoming
type: chore
category: chores
priority: P2
phase: 1
points: 2
labels: candidates, idempotency, parity
dependency: ["3e423919-a420-4195-b808-3ecb931dd16c"]
---

## Context

Native [P2 finding3997461699](https://github.com/octave-commons/epiphany/pull/18#discussion_r3997461699) remains unresolved at audited source `de47366e82f3f4b6405020776f3c3428aaef0596`. Candidate request lookup filters to the incoming resource, while candidate request-ID uniqueness is collection-wide. Reuse across resources is therefore misreported as storage unavailability by local/Mongo, while Clio reports conflict.

## Outcome

A candidate request UUID identifies its admitted intent globally across resources. Reuse with changed resource/content is an idempotency conflict; an identical replay returns the accepted durable record.

## Scope

Express the global candidate request lookup as an observations port contract and implement that contract at affected provider boundaries. Use the global lookup before and after admission, retaining resource-id as part of intent equality. Keep resource-scoped listing for its existing query callers.

## Non-goals

No whole-store export scan as a substitute for lookup authority, similarity/identity merge, record rewrite, arbitrary collision resolution, service/DB execution, or runtime/warning migration.

## Acceptance criteria

- Before-fix cross-resource UUID reuse fails for the expected wrong-outcome reason; after-fix it reports a consistent explicit idempotency conflict for local, Mongo-contract and isolated Clio cases.
- Exact same-resource/same-content retry returns the accepted record; changed relation/source/target/confidence/generator/tier also conflicts.
- Missing/corrupt/unreadable results and uncertain writes remain unavailable/integrity outcomes rather than fabricated empty success.
- Concurrent same-ID admission is verified against global lookup/readback semantics, with no duplicate candidate or false acknowledgement.
- Existing resource-scoped queries remain correct and native finding disposition binds the new personal head without transferring approval.

## Verification

Red/green application/provider contract fixtures, global lookup valid/invalid/absent cases, local/isolated-Clio parity, lint/format/layer/interop and fresh unit/AOT checks. Do not claim Mongo integration without an authorized real isolated fixture; no ambient Mongo access.

## Risks

A new lookup port affects every provider and fake port fixture; enumerate those consumers before changes. Global uniqueness and request namespaces must stay explicit. This incoming card depends on reviewed migration/readiness; required native warning evidence remains an independent blocker.
