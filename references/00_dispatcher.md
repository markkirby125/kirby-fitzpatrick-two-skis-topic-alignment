# Two Skis Topic Alignment — Technical Operational Dispatcher

**Framework Author**: William Fitzpatrick (*Writer Science*)  
**Source Lecture**: [4 Writing Hacks that Readers Love](https://www.youtube.com/watch?v=_X0UERlwz7Y)  
**Parent Collection**: [Master Collection](../../kirby-fitzpatrick-writers-collection/SKILL.md) | [Global Help](../../../kirby-help/SKILL.md)  

---

## 1. Cognitive Foundation: The Two Skis — Local Subject Parallel to Global Theme

### 1.1 The mechanism in one sentence

A reader tracks two ski tips at the same time:

* **Ski 2 (global theme)** — the single entity-and-predicate axis the *paragraph* is about. It is the paragraph's fall line: the reason every sentence in the block exists. It is normally announced by the paragraph's opening sentence or by the heading above it.
* **Ski 1 (local subject)** — the grammatical subject of each individual sentence (or whatever noun phrase occupies the sentence-initial slot: the fronted adverbial is Ski 1 whether you meant it or not).

Fluency is the paragraph-level analogue of parallel skiing:

> **Ski 1 must ride Ski 2's fall line. Every sentence subject must be the theme itself, a facet of the theme, or a participant the theme's opening sentence explicitly licensed. Mid-paragraph subject jumping is a crashed run.**

The forbidden move is not a bad word or a clumsy clause — it is a *subject exchange*. The writer's attention leaves the module they are describing, traverses the system graph to a neighbouring component, and drops that neighbour into the subject slot. The sentence is grammatically fine and semantically true. It is still a fall: the reader's discourse model was anchored on component A, and sentence N hands them component B with no re-anchoring, no boundary, and no declared reason why B now owns the sentence.

### 1.2 The fall line: parallel vs. divergent subjects

```text
✅ PARALLEL — Ski 1 rides Ski 2's fall line

  Ski 2  ═════════════════════════════════════════════════════════════════════►
         (paragraph theme held constant, announced in sentence 1)

  Ski 1    ▐▐▐  ▐▐▐  ▐▐▐  ▐▐▐  ▐▐▐  ▐▐▐   every subject inside the same track
           S1   S2   S3   S4   S5   S6      (theme / facet / licensed participant)

  Reader model: one entity, progressively enriched.  Zero model rebuilds.


❌ DIVERGENT — mid-paragraph subject jump (the snowplow crash)

  Ski 2  ═══════════════════════════════════════════════════════════►

  Ski 1    ▐▐▐  ▐▐▐  ╱╱╱ ✗  ╱╱╱ ✗  ▐▐▐
                    └── skis cross: subject leaves the namespace
                        reader tumbles, flushes the anchor, re-indexes

  Reader model: N fragments.  Each jump costs one model rebuild.
```

### 1.3 Before / after on a real module

```text
❌ BEFORE — three themes, one paragraph, two vertigo events in four sentences

  Ski 2 (declared): "how the retry queue achieves idempotency"

  S1  [ The retry queue ]      ................ [ derives an idempotency key per tenant ]   ✓
  S2  [ The schema migration ] ................ [ adds the tenants.salt column ]             ✗ JUMP
  S3  [ Our CI pipeline ]      ................ [ now runs the integration suite nightly ]   ✗ JUMP
  S4  [ That key ]             ................ [ scopes to the tenant salt on every replay ] ✓

  Ski trace:  ────────╲╱──────────╲╱────    thread broken twice; S4 pays for all three anchors


✅ AFTER — one axis per paragraph (fission, not fusion)

  PARAGRAPH 1 · Ski 2 = the retry queue
    S1  The retry queue derives an idempotency key per tenant.              ✓ theme
    S2  That key scopes to the tenant's salt before it is stored.           ✓ facet of the key
    S3  The queue evicts stored keys after 24 hours of inactivity.          ✓ theme again

  PARAGRAPH 2 · Ski 2 = the schema migration        ← new heading / new fall line
    S1  The migration adds a tenants.salt column.                           ✓ new theme stated first
    S2  It backfills existing rows from the tenant ID.                      ✓ facet

  PARAGRAPH 3 · Ski 2 = CI verification
    S1  CI now runs the integration suite nightly.                          ✓ new theme stated first
```

### 1.4 Developer vertigo: why prose docs are uniquely exposed

A software system's true model is a **graph**. The writer's honest impulse is to *narrate the graph*: the moment a neighbour becomes relevant, traverse to it and make it the subject. That instinct produces correct sentences and unreadable paragraphs, because prose is a **linear serialization of a graph** and cannot carry more than one focal node at a time. Two consequences:

* **Human cost.** Ski 2 is the reader's answer to "why am I being told this?" Dropping a stranger into the subject slot deletes that answer mid-paragraph. The engineer experiences it as the physical discomfort of holding three modules in flight while nothing binds — vertigo — and resolves it by skimming to the next heading. In RFC review this is how a correct objection goes unread.
* **Agent cost.** An LLM reading the same paragraph resolves subjects the same way and, worse, *fills the gaps*. An unaligned subject is an invitation to invent the causality you skipped, and the fabricated connective tissue then propagates into generated code.

Two-Skis alignment is therefore the cheapest available repair: it changes no facts, only *which entity is permitted to own a sentence*, until the paragraph's axis is exhausted.

### 1.5 Boundary discipline

Two Skis is a **paragraph-scoped** protocol and sits one level above its siblings. It does not govern slot order inside a clause ([Topic Comment Elaboration](../../kirby-fitzpatrick-topic-comment-elaboration/SKILL.md)), nor the promotion of one sentence's payload into the next sentence's opening ([Linear Relay Linking](../../kirby-fitzpatrick-linear-relay-linking/SKILL.md)), nor the upfront announcement of a fixed component set ([Cathedral Taxonomy](../../kirby-fitzpatrick-cathedral-taxonomy/SKILL.md)), nor the sequence preview at paragraph opening ([Theme Preview Roadmap](../../kirby-fitzpatrick-theme-preview-roadmap/SKILL.md)). It constrains the one thing those protocols assume is already decided: **which entities may occupy subject position at all, across the whole paragraph.**

---

## 2. Core Transformation Protocols

### Rule 1 — Declare the Fall Line (Ski 2 first)
Before the paragraph's first content sentence, be able to state its theme as a single noun phrase plus axis: *"how the retry queue achieves idempotency."* If you cannot, the paragraph is not one paragraph — it is a section, and it needs headings and subdivisions. The opening sentence must make Ski 2 unambiguous to a reader who skipped the previous paragraph.

### Rule 2 — The Namespace Admission Test (three legal Ski 1 relations)
A sentence subject is legal only if it satisfies at least one of:

1. **Identity** — it *is* the theme entity (`The retry queue…`).
2. **Facet** — it is a field, method, derived value, or state of the theme entity (`That key…`, `The eviction window…`, `Its storage cost…`).
3. **Licensed participant** — it was explicitly named as a participant by the paragraph's opening sentence (`The retry queue and its salt provider…`).

Anything else is **subject smuggling**: a stranger parked in the subject slot. Demote it to the predicate of a theme-owned sentence (`The queue delegates salting to the tenant provider`) or fission the paragraph (Rule 6).

### Rule 3 — No Mid-Paragraph Jump (one axis per paragraph)
A paragraph declares exactly **one** Ski 2. Coordination of two peers (`The retry queue does X, while the schema migration does Y`) is a jump even when both facts are true, because the reader must hold two competing anchors in one segment. Contain them by subordination, or split them.

### Rule 4 — Never Re-Name the Ski
Once Ski 2 is chosen, hold that noun phrase verbatim. Do **not** elegant-vary the theme referent:

* ❌ *"The retry queue … This component … The worker … Our dedup layer … The service …"*
* ✅ *"The retry queue … The retry queue … The retry queue …"*

Synonym rotation is the single most common cause of paragraph-level vertigo, because each new label forces an identity check ("is the worker the same thing as the dedup layer?"). Anaphora (`it`, `that key`) and the definite article are the only permitted shortenings, and only when the antecedent is unambiguous in the paragraph.

### Rule 5 — Licensed Participants Only (introduce before you subject)
When a paragraph genuinely needs to mention an adjacent component, introduce it **inside a sentence owned by the theme**, in the predicate (or non-initial) position. Only after that introduction may it occupy a subject slot, and only if Rule 2 authorizes it. Never let a first mention of an entity appear in subject position.

### Rule 6 — Fission Over Fusion
When the material genuinely bifurcates — two themes of comparable weight — the repair is a paragraph break plus, in long documents, a heading. One paragraph = one slope. Do not splice two runs into a single block by adding a transition word: `Meanwhile`, `Also`, `Additionally`, `Separately` used as the whole connective tissue are smoke detectors for an un-split paragraph, not repairs.

### Rule 7 — Axis Change Only at the Seam
Any change of Ski 2 requires: (a) a paragraph boundary, and (b) an explicit re-anchor at the head of the new paragraph that re-states the new topic in full — the new theme's noun phrase, not a pronoun. A pronoun-only paragraph opening (`This also…`, `It then…`) is an axis change disguised as continuity.

### Rule 8 — Subject Readback Audit
Extract only the sentence subjects, in order, and read them as a list. A compliant paragraph yields a monotone facet list of one entity (`The retry queue … That key … The eviction window … The stored-key index …`). Any entry that requires the phrase *"wait — when did we start talking about…"* is a jump, and the fix is Rule 2, 6, or 7.

### 2.1 Transformation Table

| # | Anti-Pattern (Ski divergence) | Defect | Clean Replacement (Ski 1 back on Ski 2) |
|---|---|---|---|
| 1 | *"The retry queue derives its idempotency key per tenant. The schema migration then adds the `tenants.salt` column."* | Two themes coordinated in one paragraph; the second sentence abandons the axis mid-run. | *"The retry queue derives its idempotency key per tenant. It reads the salt it needs from the `tenants.salt` column."* → migration gets its own paragraph. |
| 2 | *"The auth middleware rejects replayed refresh tokens. CI now fails the build on lint errors. The token rotation window is 15 minutes."* | Three unlicensed subjects in three consecutive sentences; Ski 2 never declared. | Split into three paragraphs, each opening with its own theme stated in full: `The auth middleware…` / `CI…` / `The token rotation window…` |
| 3 | *"The cache layer … This component … The eviction worker … Our hot path …"* | Synonym rotation of one referent; each label triggers an identity check. | *"The cache layer … The cache layer … The cache layer…"* — keep the noun phrase verbatim (Rule 4). |
| 4 | *"The scheduler drains low-priority queues. Meanwhile, the metrics exporter batches to disk every 30s."* | `Meanwhile` as the sole connective; paragraph splicing instead of fission. | *"The scheduler drains low-priority queues. The metrics exporter has no part in that decision — see the exporter section."* → separate paragraph. |
| 5 | *"We added a `TenantSaltProvider`. The billing service also caches invoices. That provider read-through caches salts."* | Stranger (`billing service`) in subject slot; the new entity's first appearance was also a subject. | *"We added a `TenantSaltProvider` that read-through caches tenant salts. (Billing caching is a separate change — next paragraph.)"* |
| 6 | *"It also retries. This also applies to uploads. That is true for webhooks too."* | Pronoun-only subjects with no intra-paragraph antecedent; every sentence is an axis change disguised as continuity. | *"The retry loop also covers uploads and webhooks: it retries each of them three times before surfacing the error."* |
| 7 | *"Under sustained contention, the scheduler starves low-priority jobs, which is why the `JobRunner` timeouts, and the alerting rules changed last sprint."* | Fronted adverbial becomes Ski 1, then the trail jumps twice by trailing clause. | *"Under sustained contention, the scheduler starves low-priority jobs. It holds them for up to 90s."* → `JobRunner` timeout and alerting become their own paragraphs. |
| 8 | *"The migration adds a salt column. Backfilled rows must be re-hashed. The rollout is gated behind a flag."* | Three themes presented as one run; the reader cannot tell which is Ski 2. | Three paragraphs: `The migration adds a salt column.` / `The backfill must re-hash existing rows.` / `The rollout is gated behind a flag.` |

### 2.2 Repair Procedure (60-second pass)

1. **Name the axis.** Write Ski 2 for the paragraph as one noun phrase plus verb axis, in the margin. If you cannot, delete the paragraph's first sentence and see whether a theme emerges.
2. **Underline every subject** — including fronted adverbials, which occupy the slot whether you intended them to or not.
3. **Run the Namespace Admission Test** (Rule 2) on each. Legitimate strangers are candidates for demotion into the predicate, not for deletion.
4. **Cut at every second theme.** Insert a paragraph break; write a full noun-phrase opening for the new block (Rule 7). Add headings if the document is longer than a screen.
5. **De-vary the theme referent.** Replace every synonym of Ski 2 with Ski 2 itself; keep pronoun/anaphora only where the antecedent is unambiguous.
6. **Read the subjects alone.** The list must scan as a monotone facet inventory of one entity. If it reads as a table of contents of the whole system, the paragraph is still fused.

---

## 3. Engineering Application Scenarios

### 3.1 Code Reviews

A review comment is a paragraph whose Ski 2 is *the risk at the reviewed site*. The reviewer's own reasoning chain — how they noticed it, what they were reading before, which unrelated concern the same file also has — is the most common source of subject jumps, and it forces the author to re-derive the anchor per sentence.

* **Violation**: *"The retry loop here can double-deliver on a 504. Also we should rename `handleRetry` because the naming is inconsistent with the rest of the package, and while I was in there I noticed the CI job doesn't run the integration suite."*
* **Repair** — three comments, one axis each, each anchored to a line: *"The retry loop can double-deliver on a 504 — it retries before the idempotency key is committed. Commit the key first, or gate the retry on `Retry-After`."* / separate comment: *"`handleRetry` vs. `handle_retry` — which convention wins in this package?"* / separate comment: *"CI skips the integration suite on the `main` branch."*
* **Severity framing keeps the axis**: *"The `retryBudget` counter resets per request. Under a failure loop it never exhausts."* — both sentences ride the counter.
* **Thread hygiene**: one concern per comment *is* the two-skis rule applied to review tooling. A comment that opens on `Close` and pivots to naming is the snowplow crossing; the author resolves one anchor and is then asked to switch to another before the first binding commits.

### 3.2 PR Descriptions

The PR has exactly one Ski 2 at the document level — *the behavioural change this branch makes* — and each paragraph holds one sub-axis that instantiates it. The reviewer's anchor is the module as it existed before the branch; anything that pulls Ski 1 off the branch's change set is a jump.

```text
❌ FUSED (subject jumps inside each paragraph)

  "A new TenantSaltProvider interface plus the hashing change are in, which changes the
   retry queue's dedup semantics, and the schema migration adds the salt column, and CI
   was updated. Also the README got a new table."

✅ AXIS-PER-PARAGRAPH (each paragraph's theme declared, one run per block)

  Paragraph 1 · Ski 2 = the new dedup semantics
    "The retry queue now derives idempotency keys per tenant."

  Paragraph 2 · Ski 2 = the interface that enables it
    "The TenantSaltProvider interface is what the queue calls to resolve a tenant salt."

  Paragraph 3 · Ski 2 = the migration
    "The migration adds a tenants.salt column and backfills existing rows."

  Paragraph 4 · Ski 2 = verification
    "CI now runs the integration suite nightly; the suite covers the replay path."
```

* **Risk sections** keep their axis on the affected component for the entire paragraph: *"The `PaymentRouter` p99 rises ~40 ms during the backfill because…"* — not `"There is a potential latency concern"`, which declares no axis and licenses every subsequent subject in the paragraph.
* **Reviewer routing** follows the axis: if a paragraph's subjects drift into a module the routed reviewer does not own, the description will be triaged to the wrong specialist. Re-splitting the paragraph fixes both the prose and the assignment.

### 3.3 Architecture RFCs / ADRs

RFC sections are read out of order and months later, so each paragraph must re-declare its Ski 2 from a shared anchor rather than relying on reading momentum. The classic failure is a **"Motivation" section with three competing axes** — business goal, current implementation, intended end state — where reviewers lose the thread and object to the wrong thing.

```text
❌ MOTIVATION SECTION — one paragraph, three axes

  S1  The nightly cron re-index holds a table lock for ~11 minutes.   ✓ axis A: the cron
  S2  Our growth target is 4× writes within 18 months.                 ✗ axis B: business
  S3  A queue-driven re-index would remove the write-path bottleneck.  ✗ axis C: proposal
  S4  Operations currently tolerate the lock because traffic is low.   ✗ back to axis A

✅ THREE PARAGRAPHS — one run each, axes in dependency order

  ¶1 axis = the cron              The nightly cron re-index holds a table lock for ~11 minutes.
  ¶2 axis = the lock under load   That lock blocks write traffic for the duration of the pass.
  ¶3 axis = the consequence       Under the 4× growth target, the lock becomes the write-path bottleneck.
  ¶4 axis = the decision          We replace the cron with a queue-driven re-index.  ← new Ski 2 declared
```

* **ADR framing maps 1:1 to runs**: *Context* = one paragraph per known-state entity; *Decision* = one paragraph whose axis is the new mechanism; *Consequences* = one paragraph per consequence class, each declaring its own axis.
* **Options comparison is an axis invariant, not a subject invitation**: every option paragraph must open on the *same* evaluation axis — *"Under a 4× write load, Option A caps the queue at… Option B caps the queue at…"* — so the reader's anchor survives the whole comparison. Variants that each open on a different noun force a re-anchor per bullet; that is exactly where reviewers stop comparing and start skimming.
* **Migration plans**: one paragraph per state, axis = the state currently deployed. Opening a migration step on the target state makes operators apply step 3 to a system still in step 1.

---

## 4. Verification Checklist

- [ ] **Ski 2 declared** — Can you state each paragraph's theme as a single noun phrase plus axis, and does the paragraph's opening sentence make it unambiguous without the preceding paragraph?
- [ ] **Namespace admission** — Does every sentence subject satisfy the Identity / Facet / Licensed-participant test, with no strangers smuggled into the subject slot?
- [ ] **No axis change inside a run** — Does any paragraph coordinate two peer themes, or change axis without a paragraph break and a re-anchoring noun phrase?
- [ ] **Referent stability** — Is the theme entity named with one consistent noun phrase, with synonym rotation (`this component`, `the worker`, `our dedup layer`) eliminated?
- [ ] **Subject readback** — Reading only the subjects in order, does each paragraph yield a monotone facet inventory of one entity rather than a table of contents of the system?