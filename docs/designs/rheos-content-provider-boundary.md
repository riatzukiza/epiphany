---
slug: rheos-content-provider-boundary
uuid: ac6c1d68-89b6-46e8-9f07-01cbf254d94a
title: "Rheos content and review across document providers"
kind: design
status: open
description: "Proposes an Epiphany evidence boundary for a Rheos content/review workspace over compatible document maps, with Markdown first and provider-specific retention and writes."
labels: [rheos, documents, review, providers, provenance]
created: "2026-10-03"
requires-decisions: ["ADR-000"]
source-revision: "643be698ea0d841dd19385506b272872306e456e"
license: GPL-3.0-or-later
---

# Purpose and status

Rheos is being framed as a content management and review workspace: open a
document, discuss the observed content, propose a change, and review the result.
Markdown remains the primary representation and first implementation slice.
Compatible document maps may also come from API queries, live or volatile data,
or raw input assembled into a readable representation. Those inputs need not
become stored Markdown files.

This is an open design proposal, not an accepted architecture decision or a
claim that these providers exist. It connects Epiphany's document/evidence
distinctions to the wider Rheos direction without changing Phase 1 ingestion,
existing process-policy status, board behavior, or production code.

The draft and its first review fixes preceded local board intake. The fresh
[planning/review card](../kanban/chores/plan-rheos-provider-boundary-review-7648b7b5.md)
is `incoming`; it records prospective grounding and review, not retroactive
readiness, completion or acceptance of the prepared draft. Review and readiness
remain outstanding under the existing Epiphany process.

## Scope and non-goals

The proposed boundary covers source context, assembly into compatible document
maps, evidence supplied to discussion/review, retention, and provider
capabilities. It does not specify a new universal schema or implement a provider
registry, source connector, conversation service, filesystem writer, board
engine, or autonomous acceptance policy. Markdown-first delivery remains intact.

# Existing evidence

These paths were inspected at `643be698ea0d841dd19385506b272872306e456e`.
Their design and implementation claims have different scopes:

| Existing surface | What it establishes | Limit |
| --- | --- | --- |
| [Document governance λ](../process/document-governance.md) | Document form, bounded content, provenance, relations and status make a reviewable artifact. Its scope includes Markdown and structured records. | Draft process policy; it explicitly permits design data before parsers/checkers exist. |
| [Research artifact set](../process/research.md#artifact-set) | Sources, observations, findings and reports remain addressable across files, controlled sections, structured records and generated views. | Draft policy; a report is a synthesis, not the sole authority for its inputs. |
| [Review/acceptance α](../process/review-and-acceptance.md) | Review names target/version, criteria, evidence, reviewer/authority, disposition and limits. Verification, judgment and acceptance are distinct. | Draft policy; status, a successful command or a merge alone is not acceptance. |
| [Markdown law](../../src/epiphany/law/markdown.clj), [parser](../../src/epiphany/shape/markdown.clj) | Implemented string-to-map parsing with raw optional frontmatter, typed body blocks and exact spans. [Tests](../../test/epiphany/shape/markdown_test.clj) cover documents with and without frontmatter. | Markdown-specific shapes, not a generic decoded metadata map or API provider contract. |
| [Storage ports](../../src/epiphany/law/ports.clj), [profile assembly](../../src/epiphany/infra/profile.clj) | Implemented injection of named-function maps and validation around observation adapters. | Storage/application assembly, not a document-provider registry. |
| [Artifact identity](artifact-identity-model.md), [authority questions](phase-1-decision-status.md) | Phase 1 observes Git repositories; extraction and semantic continuity are distinct from Git facts. | Non-Git authority, including API responses, was deliberately deferred. |
| [ADR-000](../adrs/adr-000-authoritative-data-boundary.md) | Proposed Git source ownership, operation-specific caches and selective preservation. | Proposed and scoped to Git-backed Phase 1; it does not settle live-source retention. |

## Bounded research grounding and remaining gap

The concrete [source-preservation and qualification finding](../research/rheos-source-preservation-qualification-finding.md#bounded-finding)
informs the requirements to preserve unrelated Markdown source and separately
qualify parsing, real writes and durable recording. Its inputs are the pinned
Rheos baseline and probe, plus historical Promethean testing, blocker and source
records. A new direct reproduction confirmed the recorded 186-to-145-character
loss while updating one frontmatter key. The historical source inspection also
distinguishes mocked read/write control flow from real file persistence.

Those observations support bounded preservation and qualification work. They
do not establish the proposed generic provider abstraction, contract ownership,
volatile-source retention policy, discussion binding or write authority. The
finding records that [architectural evidence gap](../research/rheos-source-preservation-qualification-finding.md#architectural-evidence-gap)
explicitly. Those parts remain provisional design candidates requiring further
decision-support research and the resolution artifacts named below; this
proposal must not be treated as a completed or accepted architecture.

The [August Rheos generalization](https://github.com/open-hax/foresight/blob/b03805b0b87e5c7e9a628b0efa1fab61a066f20a/docs/notes/generalizing-rheos-artifact-event-reaction.md)
uses Epiphany's governance as design evidence for Artifact → Event → Reaction,
with boards as a projection. It is a draft integration proposal, not an
implemented general content engine or accepted common law.

# Proposed shape and responsibilities

```text
provider input + source observation
  -> declared assembly + input/output validation
  -> compatible document map
  -> reading + discussion + proposal + review
  -> optional guarded write through the owning provider
  -> observed outcome + explicit review basis
```

Compatibility means satisfying the contracts needed for a declared capability.
It does not mean every source has a path, Git revision, frontmatter, persistent
payload, workflow status, or write operation. Assembly names its input/output
shapes, mappings, transformations and version. Parsing or formatting derives a
representation; it does not establish truth, acceptance or write authority.

| Participant | Proposed responsibility |
| --- | --- |
| Source provider | Source/entity/query identity, observation context, coverage, freshness, retention, reproducibility and supported writes. A source keeps its own authority. |
| Rheos | Content views, observation-bound discussion, proposals, review commands and coordinated provider operations. Board participation is an optional task facet. |
| Epiphany | Historical observations, retrieval, exact spans, lineage and evidence packets within its admitted source scope. It does not become Rheos's editor or silently authorize source changes. |
| Shared contracts | Reuse Alpha/Katamorph and existing Epiphany shapes where compatible. Promote genuinely shared pure laws through explicit ownership/acceptance, rather than duplicating their implementation here. |
| Other adapters | Chat UI can supply conversation primitives, Osmos acquisition/jobs and Clio admitted event semantics. These are proposed integrations, not mandatory new runtime dependencies for every read. |

## Proposed preparatory streams

Outside this repository, this design proposes narrower preparatory streams:
common pure document-observation/admission contracts in Foresight Alpha, and
targeted Markdown frontmatter write preservation in Rheos. These are proposed
coordination context, not existing evidence of a qualified implementation or
merge. This proposal neither depends on an unqualified external candidate nor
claims that either stream supplies a provider runtime or general CMS reader.
Before consuming external work, its source record must pin an immutable
revision and the checks/review that qualify it. Document reading, discussion and
reviewed-edit integration remain follow-on work.

An API query and each returned entity have different identities. Content digest,
observation time, source version and semantic identity also differ. A provider
without reproducible versions exposes that limitation; it never invents a Git
revision. Raw input can be formatted into a derived view while its origin and
transformation remain inspectable.

## Review and retention

Discussion binds to the source observation, assembly version and selected
context it actually used. Refreshing live content creates another observation;
it cannot silently rebind old messages or proposals. A review names its target,
criteria, available basis, disposition, actor/authority and limitations.

Payload retention is separate from review-record retention. Reading, discussion
and provisional review may use an ephemeral payload; bounded capture remains
optional for those activities. A retained review record preserves its available
source locator, observation/query context, digest, transformation version,
disposition and relevant evidence references under an explicit policy.

Acceptance or promotion of a claim that depends on volatile evidence requires
sufficient bounded retained evidence, or a pinned source capable of reproducing
the relevant observation. The durable acceptance event must reference that basis
and state its limits. A locator, query description or digest alone is insufficient
when it cannot recover the evidence supporting the claim. Without that basis,
the claim remains provisional or needs investigation; it cannot be accepted or
promoted. This gate preserves the evidence needed for the claim, not every live
payload. Credentials are excluded. Exact replay of a full payload is reported
unavailable when it was not retained and the source cannot reproduce it, even
when bounded evidence is sufficient for a narrower accepted claim.

A read-only provider permits discussion and judgment without an implied write
protocol. Publishing a report derived from its data creates a separate target.
For writable sources, accepting a judgment, authorizing an application, attempting
the write and observing its outcome are distinct. The owning writer qualifies
its source-version guard, retry behavior and recovery; rendering compatibility
does not establish those guarantees.

For the first Markdown slice, preserve untouched source text and arbitrary
metadata. Compare against actual reviewed bytes, including working-tree edits,
and handle comparison/write races at the owned write boundary. Record partial
outcomes when the write succeeds but durable review/event recording fails.

# Follow-on plan and evidence

1. **Common read boundary — unblocked preparation.** Map one Markdown observation and one read-only
   API-shaped fixture into compatible reviewable inputs. Reject malformed
   assembly output. The fixture has no path, Git revision or frontmatter and
   performs no network operation. Prototype contracts do not settle shared
   ownership or admit a production source.
2. **Markdown preservation — unblocked preparation.** Qualify preservation of
   untouched source text and arbitrary metadata within the existing Markdown
   boundary. This does not add conversation persistence, review authority or a
   new write protocol.
3. **Markdown discussion and reviewed edit — blocked.** Resolve `DISCUSSION-EVIDENCE-BINDING`
   and `WRITE-AUTHORITY-GUARDS` before implementing this integration; resolve
   `DOCUMENT-CONTRACT-OWNERSHIP` before consuming a shared cross-repository law.
   Bind discussion and a diff to the
   observation. Demonstrate rejection with unchanged bytes, source-change
   conflict, targeted source preservation, idempotent acceptance, and recovery
   of discussion/decision context after restart.
4. **Epiphany evidence seam — runtime integration blocked.** Resolve
   `DOCUMENT-CONTRACT-OWNERSHIP` and `DISCUSSION-EVIDENCE-BINDING` before integration.
   Supply one exact revision/span evidence packet
   from the existing Git corpus. Show observed evidence and provisional
   interpretation separately, including unavailable evidence. This is the first
   integration candidate, not a replacement ingestion engine.
5. **Additional production providers — blocked.** Resolve `DOCUMENT-CONTRACT-OWNERSHIP`
   and `NON-GIT-AUTHORITY-RETENTION` before admitting API, live or raw-input
   providers. Discussion and writes additionally depend on their corresponding
   decisions below. Qualify live refresh,
   production raw-input formatting, optional capture and external writes only
   when their adapters are implemented. Each identifies its retention and
   authority decisions and demonstrates its real provider boundary.

An initial read contract is demonstrated by fixtures; a user-visible loop also
needs a real provider/browser walkthrough. Existing board UUID/FSM/comment and
ledger behavior remains under Rheos and is checked as a consumer regression,
not reimplemented in Epiphany. Documentation review verifies source references
and claim scope; it does not report any proposed integration as tested runtime
behavior.

# Alternatives and open decisions

Persisting every input as Markdown would simplify one editing path but change
source authority and retention for live/query data. Keeping every source native
with unrelated review flows would preserve origin but duplicate review
semantics. Compatible maps with explicit capabilities offer a common review
boundary while leaving those source differences visible.

## Decision candidates and implementation blockers

The following names identify open decision candidates in this proposal, not
issued or accepted ADRs. The `requires-decisions` reference to ADR-000 preserves
the existing authority boundary; clearing it does not resolve these candidates.
The affected integration slices above remain blocked until the named resolution
artifacts exist and are accepted through the repository's decision process.

| Open decision candidate | Blocked scope | Required resolution artifact |
| --- | --- | --- |
| `DOCUMENT-CONTRACT-OWNERSHIP` | Production use of a shared cross-repository document boundary and the Epiphany/Rheos evidence integration. | Accepted architectural decision naming contract ownership, source/query/entity identity, assembly/schema versioning, and qualified immutable source revisions before consumption. |
| `NON-GIT-AUTHORITY-RETENTION` | Production API/live/raw-input providers and non-Git evidence retention or promotion. | Accepted ADR-000 follow-on naming canonical source authority, observation coverage/freshness, retention choices, sufficient preserved evidence for promotion, and unavailable/reproducibility behavior. |
| `DISCUSSION-EVIDENCE-BINDING` | Persisted discussion/review integration, including the Markdown loop and Epiphany evidence seam. | Accepted architectural decision defining observation/assembly/target binding, refresh and conflict behavior, durable review records, and restart/recovery contracts. |
| `WRITE-AUTHORITY-GUARDS` | Applying reviewed Markdown edits and every external source write. | Accepted provider-specific architectural decision naming authorization, reviewed-version guards, race handling, idempotency, and recovery when writes and review-record persistence diverge. |

Pure fixture work, inspection of existing Git evidence, and targeted preservation
work within existing Markdown contracts remain unblocked preparation. They do
not authorize the blocked integration stories, decide these architectural
questions, or admit a production non-Git source.

The next bounded step is review of the common read contract and its two fixture
representations. This proposal does not mark that step accepted or implemented.
