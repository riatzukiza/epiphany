---
slug: plan-rheos-provider-boundary-review
id: "7648b7b5-116a-42e8-9cad-52f9f32e3ba6"
uuid: "7648b7b5-116a-42e8-9cad-52f9f32e3ba6"
title: "PLAN-RHEOS-001: Ground and review the prepared provider-boundary proposal"
kind: story
type: planning-record
category: chores
status: incoming
priority: P1
phase: 1
points: 3
owner: codex
labels: "rheos, design, research, review, planning, provenance"
description: "Prospective local intake for grounding and independently reviewing the already prepared provider-boundary draft; no implementation or prior acceptance is claimed."
created: "2026-10-03"
design: docs/designs/rheos-content-provider-boundary.md
license: GPL-3.0-or-later
---

# Ground and review the prepared provider-boundary proposal

## Intake and current state

The [open design](../../designs/rheos-content-provider-boundary.md) and its first
review corrections were prepared before this local card existed. The published
[draft review](https://github.com/riatzukiza/epiphany/pull/1) exposed that missing
intake. This fresh local identity records prospective grounding and review;
`incoming` does not assert an eligible ready card, prior execution authorization,
completed work, architectural acceptance or a process exception. No historical
transition is reconstructed.

The independent [Foresight coordination story](https://github.com/riatzukiza/foresight/blob/1e0cabfca49788e43bfb4cd93446ad8e8971c457/docs/agile/kanban/design-epiphany-provider-and-review-boundaries-76983190.md)
at `1e0cabfca49788e43bfb4cd93446ad8e8971c457`, published in
[Foresight PR 2](https://github.com/riatzukiza/foresight/pull/2), supplies broader
coordination context. It is not this card's identity, an Epiphany board
dependency, a local ready-state grant or a completion record.

## Outcome

A qualified reviewer can evaluate the prepared provider-boundary proposal for
its bounded purpose, inspect concrete grounding and remaining research gaps,
and record a scoped disposition without confusing the draft with accepted
architecture or a delivered provider runtime.

## Scope and inputs

- Triage this initial card and qualify the prospective review slice under
  [board policy](../../process/kanban.md) and the [board operational guide](../AGENTS.md).
- Review the design against [the Process Charter](../../../PROCESS.md),
  [research practice](../../process/research.md),
  [document governance](../../process/document-governance.md) and existing ADR
  status; policies and proposals retain their declared status.
- Inspect the [bounded research finding](../../research/rheos-source-preservation-qualification-finding.md)
  and its immutable primary inputs, separating observed evidence, derived
  conclusions and provisional architectural candidates.
- Identify follow-on research or decisions where the existing evidence is
  insufficient; record owner, question and affected scope before further work.
- Review and settle findings with immutable artifact/check references, retaining
  the missing-ready-card finding until prospective qualification is recorded.

## Non-goals

- No production code, source provider, generic schema, API integration or writer.
- No changes to ADR authority, current process policy or the board engine.
- No acceptance of the proposed shared law, retention model or decision candidates.
- No retroactive ready/done transition, fabricated review event or waiver.
- No duplication of Foresight's card identity or cross-board dependency edge.

## Dependencies and readiness

No local execution dependency is asserted for initial triage. The design and
finding are draft inputs to review, not completed prerequisites. A qualified
actor must confirm scope, estimate, applicable authority, verification and
delivery order through the active `eta-mu kanban` process before this card may
become `ready`. Use the installed CLI facade documented in the
[operational guide](../AGENTS.md), for example this read-only inspection:

```bash
eta-mu kanban list --tasks-dir docs/kanban
```

Identify this card by UUID `7648b7b5-116a-42e8-9cad-52f9f32e3ba6` in the output.
Rheos remains the sole implementation authority for board semantics; the
active CLI facade is the command surface for those operations. This does not
restore the older workflow replaced by [current policy](../../process/kanban.md#transition-note)
or authorize another board implementation or hand-edited status transition.
Subsequent
architectural or implementation work needs its own bounded, qualified work item
and the appropriate research/decision inputs. Initial intake is not that grant.

## Acceptance criteria

- Prospective readiness and board review are recorded through the active
  `eta-mu kanban` process, without implying they predated the prepared draft.
- Review names the exact design/finding versions, criteria, inspected source
  basis, reviewer/authority, disposition and remaining limits.
- The finding traces source preservation and separate qualification of
  read/write/persistence to concrete evidence; the broader architectural evidence
  gap remains explicit or is addressed by separately scoped research.
- The design separates implemented contracts, proposals, open decisions and
  blocked production integrations; it claims no delivered provider behavior.
- Receipt corrections preserve historical lines and bind checks to immutable
  tested blobs or trees. Review findings are settled according to their actual
  disposition rather than suppressed by a passing document check.
- Authorized acceptance, if later recorded, names only this card's grounding
  and review outcome; acceptance of architecture or implementation is separate.

## Verification approach

Inspect applicable process/ADR/document rules and pinned source records. Check
new document frontmatter, local identity uniqueness, relations, relative links
and anchors, balanced fences, append-only EDN receipts and whitespace. Reproduce
the cited pure baseline probe through Rheos's pinned implementation; do not
create another parser or board implementation. These checks qualify artifact
structure and bounded observations; independent review evaluates the design.
No provider runtime suite or board validation is claimed by this intake.

## Risks, stop conditions and handoff

The prepared draft predates local intake, and no eligible ready card has yet
qualified its review. That process finding remains open. Stop any architectural
or implementation claim if a required decision, source, authority or qualified
scope is missing. This card's 3-point estimate covers bounded grounding and
review; return to planning and split the work if the broader provider/retention
research becomes necessary to its outcome. The next responsible step is local
triage by the qualified project reviewer through the active `eta-mu kanban`
process.
