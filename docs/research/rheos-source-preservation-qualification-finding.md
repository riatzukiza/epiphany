---
slug: rheos-source-preservation-qualification-finding
uuid: "255a8b66-4ac6-430e-ad68-12ce933a9323"
title: "Rheos source preservation needs a stronger boundary than parsed-field round trips"
kind: finding
status: draft
epistemic-tier: derived
description: "A bounded finding from a pinned Rheos parser reproduction and historical Promethean source inspection; it does not settle the provider or retention architecture."
labels: [rheos, research, source-preservation, qualification, provenance]
created: "2026-10-03"
owner: codex
informs: ["docs/designs/rheos-content-provider-boundary.md"]
license: GPL-3.0-or-later
---

# Question and intended use

Does the inspected Markdown/task boundary preserve unrelated source content
while updating metadata, and what kind of verification is justified for later
content-review features that rely on it?

This is bounded implementation-grounding for source preservation and
qualification scope. It supplies a concrete finding to the
[open provider-boundary design](../designs/rheos-content-provider-boundary.md).
It is a draft research artifact awaiting prospective review under the
[local intake card](../kanban/chores/plan-rheos-provider-boundary-review-7648b7b5.md).
The design and earlier evidence brief predated this local intake; this document
does not invent an earlier brief, ready-state grant or accepted research outcome.

## Scope and method

On October 3, 2026, inspect immutable source/test/report records identified by
the [Foresight evidence brief](https://github.com/riatzukiza/foresight/blob/1e0cabfca49788e43bfb4cd93446ad8e8971c457/docs/notes/rheos-markdown-content-loop.md)
at `1e0cabfca49788e43bfb4cd93446ad8e8971c457`. Select inputs bearing directly on
source preservation and the difference between mock control flow and persistence.
Reproduce its pure preservation probe with `nbb v1.3.204`, loading only the exact
Rheos parser source at the pinned baseline. Inspect the historical Promethean
primary sources to corroborate the brief rather than treating the synthesis
as its own sole authority. Compare each result with the capability it can
actually establish and search the inspected test/report record for stronger
contrary coverage. Stop at a bounded finding or an explicit evidence gap.

This inquiry excludes current candidate branches, browser/transport execution,
production provider evaluation, 2025 runtime reenactment and a general history
of every defect. It does not rerun either repository's complete suite.

## Addressable source and observation records

All sources below were inspected on October 3, 2026. Repository source is primary
evidence for code behavior; a changelog or blocker is a historical project claim
whose limits remain visible.

| Record | Immutable input and method | Observation and limit |
| --- | --- | --- |
| O1 | [Rheos parser](https://github.com/open-hax/rheos/blob/11811264a308d406cb612aefa1dad40818675e5e/src/rheos/backend/shape/content_parser.cljs) at `11811264a308d406cb612aefa1dad40818675e5e`; Git blob `458c9384375215a9766e95642209427c990a05b7`; direct source inspection and pure probe below. | Updating one key reparses and serializes the whole task. The exact fixture loses nested/list/multiline metadata and changes fenced-body spacing. This is the inspected baseline, not evidence about every later head. |
| O2 | [Rheos parser tests](https://github.com/open-hax/rheos/blob/11811264a308d406cb612aefa1dad40818675e5e/test/rheos/backend/shape/content_parser_test.cljs#L136); inspect the round-trip assertions and adjacent update tests. | Flat strings, an inline label vector and selected parsed fields are covered; the round-trip assertion checks UUID/title/status/labels, rather than original bytes or arbitrary nested metadata. No test execution is claimed for this source inspection. |
| O3 | [Promethean October 11 testing record](https://github.com/octave-commons/promethean/blob/06a8b83312ea70dcde6d2e423369b410e6d0d3f2/changelog.d/2025.10.11.18.00.00.md); inspect the Kanban coverage section. | The project reports mocked TaskAIManager analyze/rewrite/breakdown flows alongside broader tests. That report does not establish real writes by that manager. Other file-cache/content-manager coverage is contrary evidence against claiming the entire package had no persistence tests. |
| O4 | [Promethean October 26 blocker](https://github.com/octave-commons/promethean/blob/06a8b83312ea70dcde6d2e423369b410e6d0d3f2/docs/agile/tasks/6859f9a9-fix-taskai-manager-mock-cache.md); inspect the reported defect and required operations. | The historical task reports fake task reads and calls for real file operations and workflow integration. This is a reported production blocker, not a retained execution log proving when a user invocation failed. |
| O5 | [Promethean TaskAIManager source](https://github.com/octave-commons/promethean/blob/302bce3cc28167c2fee9a7fa67c87e60ead7c705/packages/kanban/src/lib/task-content/ai.ts); inspect the constructor's cache function map. | The reader returns a constructed test task; the writer only logs; the cache backup function returns a pathname. This corroborates the narrower missing real persistence boundary without asserting all backup or mutation paths elsewhere behaved that way. |

## Direct reproduction

Load `src/rheos/backend/shape/content_parser.cljs` from Rheos baseline
`11811264a308d406cb612aefa1dad40818675e5e` into an isolated temporary classpath,
preserving source blob `458c9384375215a9766e95642209427c990a05b7`. Invoke that
implementation directly; no alternate parser is introduced. This exact probe
ran with `nbb -cp <temporary-source-root> <probe.cljs>`, exit 0:

```clojure
(require '[rheos.backend.shape.content-parser :as p]
         '[clojure.string :as str])
(let [raw "---\nuuid: doc-1\nstatus: incoming\nmetadata:\n  author: Someone\n  links:\n    - docs/a.md\nsummary: |\n  Line one\n  Line two\n---\n\n# Document\n\nParagraph.  \n\n```yaml\n---\nexample: value\n---\n```\n\n"
      result (p/update-frontmatter raw "priority" "P0")]
  (prn {:before-frontmatter (:frontmatter (p/parse-frontmatter raw))
        :after-frontmatter (:frontmatter (p/parse-frontmatter result))
        :input-characters (count raw)
        :output-characters (count result)
        :nested-author-preserved? (str/includes? result "  author: Someone")
        :multiline-value-preserved? (str/includes? result "  Line one")
        :body-output (:content (p/parse-frontmatter result))}))
```

Observed result:

```clojure
{:before-frontmatter {:uuid "doc-1", :status "incoming", :metadata "", :summary "|"}
 :after-frontmatter {:uuid "doc-1", :status "incoming", :metadata "", :summary "|", :priority "P0"}
 :input-characters 186, :output-characters 145
 :nested-author-preserved? false, :multiline-value-preserved? false
 :body-output "# Document\n\nParagraph.  \n\n```yaml\n\n---\nexample: value\n---\n\n```"}
```

Adding a field should change the source; equal total length is not the required
invariant. Loss of unrelated values and altered unaffected body spacing are the
observed defect. The successful probe process means the observation ran; it
does not mean preservation passed.

## Bounded finding

**Derived F1:** At the inspected Rheos baseline, a parsed-field round trip is
insufficient evidence that a metadata edit preserves a richer Markdown
document. O1 reproduces unrelated data loss; O2 shows why the narrower assertion
does not detect it. A source-preserving edit contract should compare unchanged
metadata and body content at the actual supported write boundary.

**Derived F2:** Qualification is specific to the boundary exercised. O3–O5
support separating mocked decision/control flow from real file reading,
writing, rereading and durable recording. Later content-review work should
name which of those capabilities was exercised rather than inheriting a
general “working” claim. The historical evidence establishes an unqualified
persistence seam; it does not establish the date of a runtime failure or prove
that every such defect was a regression.

Confidence is high for F1's one reproduced fixture and F2's inspected function
map, and moderate for their design implications. This assessment comes from
direct reproduction and corroborated source inspection, not citation count,
coverage percentage or a numerical model score.

## Contrary evidence and limitations

Rheos's baseline does support and test narrower flat-card operations. Promethean
reports real file-cache/content-manager tests as well as mocks. These findings
therefore do not condemn mocks, deny that any file operations worked, or
generalize one fixture to all content. Source inspection cannot establish the
behavior of an unexecuted installed CLI, HTTP/MCP transport, browser, restart
path or event ledger. No proposed fix is qualified by this baseline finding.

## Architectural evidence gap

The inspected evidence does not determine whether compatible provider maps are
the right common abstraction, who owns their contracts, how non-Git authority
works, which volatile payloads need retention, how discussion binds to evidence,
or how write authorization and recovery should work. The design's wider provider
and retention model remains provisional; its four named decision candidates
are not answered by F1 or F2.

Before making an architectural acceptance or implementing the affected
integration, scope decision-support research that compares at least a
source-native review seam with a shared compatible-map seam, and a bounded
capture/pinned-reproduction option with ephemeral read-only use. Record real
source guarantees, failure/recovery cases, contrary evidence, preservation costs,
and an explicit decision disposition. That research needs prospective board
readiness and its own bounded question where it exceeds the local review card;
this finding does not silently authorize it.

## Disposition and authority

F1 and F2 may inform targeted Markdown preservation and a verification plan
that distinguishes read/write/persistence boundaries. They do not decide the
provider or retention architecture, promote shared law, clear open decisions,
qualify a candidate implementation, or mark the local card done. The qualified
project review process may accept this finding only for its named bounded use;
no such acceptance has been recorded here.
