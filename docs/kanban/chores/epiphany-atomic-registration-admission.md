---
slug: epiphany-atomic-registration-admission
uuid: e98d2f9c-7266-47fc-9f3b-fee895306ec6
title: "Acknowledge only the durably admitted registration intent"
status: incoming
type: chore
category: chores
priority: P1
phase: 1
points: 3
labels: registration, idempotency, concurrency
dependency: ["3e423919-a420-4195-b808-3ecb931dd16c"]
---

## Context

Native [P1 finding3997461698](https://github.com/octave-commons/epiphany/pull/18#discussion_r3997461698) remains unresolved at audited source `de47366e82f3f4b6405020776f3c3428aaef0596`. Two requests with the same UUID and different paths can both miss the initial lookup. Mongo returns an idempotency-conflict map for the losing insert, but registration ignores it and acknowledges the losing path after writing its Git-local metadata.

## Outcome

A registration success identifies the one durably admitted request intent. Conflicting concurrent requests cannot acknowledge their own unaccepted path/resource or mutate losing repository metadata.

## Scope

Define the admission result contract before changing observations adapters/application orchestration. Preserve accepted request/path/resource/common-Git-dir identity and failure semantics across local, Mongo and Clio providers. Separate admission, durable metadata ownership and acknowledgement so every result is accounted for.

## Non-goals

No service/DB execution in this lane, new identity authority, broad adapter migration, path normalization, accepted record rewrite, runtime/warning workaround, or alteration of unrelated source findings.

## Acceptance criteria

- A deterministic two-request barrier fixture reproduces the lookup/admission race before the fix; exactly one distinct intent is accepted and the loser receives an explicit idempotency conflict.
- No losing-path metadata write occurs. An acknowledged registration matches the durable accepted record and correct common Git directory.
- Identical request replay preserves accepted identity and durability; changed intent, corrupt/unreadable admission, metadata-write failure and uncertain write outcomes remain explicit and cannot become success.
- Provider contracts and pure decision tests cover validation before ports, exact path preservation and the same observable conflict semantics across each affected adapter.
- Actual native finding is verified against the new personal head, then settled through canonical pr-flow; source-thread state/history is not imported as approval.

## Verification

Red/green application race and failure tests using controlled ports, contract tests for valid/invalid admission results, isolated local/Clio parity tests, lint/format/layer/interop checks and fresh unit/AOT gates. Mongo adapter integration remains separately unavailable until a separately authorized isolated service fixture exists; no whole-adapter parity claim from a fake port.

## Risks

Metadata is a second durable boundary, so ordering alone may not guarantee recovery or same-repository concurrent identity stability. Surface those anomalies, revise law/shape first and split if needed. This card is incoming; reviewed migration/readiness and native warning qualification remain blockers.
