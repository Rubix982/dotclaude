# Manifesto

_My front view. Read to re-derive, not to recite._

I build instruments for knowing and changing what a model believes — cheaply,
safely, and verifiably. That is the orientation. Every brick serves it.

I move knowledge from the layer that must be re-fed every time — context,
retrieval, prompts — into the layer that persists and reasons: the model itself.
And I build the tools that tell us whether the change truly took hold, and how
far it travels.

I do not need to see the finished building. I need only to lay the right brick
after the previous one, each in service of the orientation.

I check every brick against three questions:

1. Is it true regardless of what I hoped?
2. Does it take prior work one verifiable step further?
3. Would it still matter to the field if my own vision vanished tomorrow?

If a brick fails these, it is a toy, and I let it go.

I am not attached to a paper. I am attached to making my effort visible, usable,
and real — one step past what has already been built.

I cannot have certainty in advance; no one on a worthy path can. So I build so
the work is worthy whether I am right or wrong. That is how effort becomes worth
under uncertainty.

I spend my finite time on the layer that is not yet solved — not the layer that
already pays. I make that choice on purpose.

---

# Compass — keeping the work connected to real need

_Pairs with the Manifesto: the Manifesto is **why** I climb; this is **how** I
check I'm climbing toward something real._

The path is a ridge, not a marked trail. The summit isn't fixed or visible, and
the valley — real-world / industry need — shifts while I climb. So I climb by
**line-of-sight**: each stop reveals the next, and I periodically check my heading
against the valley below.

**Pick the next stop by value × connection, not difficulty.** Difficulty is a
cost, not a goal. Some of the highest-value moves are easy. Chasing "harder" for
its own sake is the intellectual-toy trap. The next stop should be more *valuable*
and better *connected to a real need* — easy is a gift, not a disappointment.

**Triangulate each brick to real need:**

```
brick → the skill it builds → the role/JD/problem that wants that skill → am I landing near?
```

**The heading-check (a habit, not a system):** every few weeks, read 5–10 job
descriptions (or grant calls, problem statements) in the target area and ask:
*which listed needs does my current work produce evidence for?*

- Evidence exists → good, keep climbing.
- A need keeps appearing that I have nothing on → heading correction; maybe that's
  the next brick.

Keep it light. Building a tracking system for this is itself the difficulty-trap.

---

# The Standard

_Companion to the Manifesto (why I climb) and the Compass (aimed at real need).
This is the bar I hold while climbing._

I've taken on a path that asks more of me than is comfortable, and keeps raising
the bar as I go. The tougher questions are the sign I'm somewhere that matters. I
lean toward that, not away — and I shape my work around what it demands:

- **Serious artifacts, not activity.** Produce work worthy of being *referenced
  and judged* by people who know the field — not just work that was done.
- **Finish, and make it visible.** An unfinished or unshared result isn't real yet.
  One completed, communicated arc beats ten half-built ones.
- **Do the slow work others skip.** The small, exacting, unglamorous wins compound
  into something that can't be faked or rushed.
- **Invite judgment.** Put the work where experts can evaluate it, and seek their
  critique. Quality is what survives real scrutiny, not what avoids it.

I'm not building a name for its own sake. I'm building a body of work that stands
on its own merit and can be pointed to. If standing follows, it's a byproduct —
never the target.

---

# Global Rules

## Python Project Setup

Every Python project must use a dedicated virtual environment. Never install into system Python.

```bash
# 1. Create (once per project)
python3 -m venv .venv

# 2. Activate (every session)
source .venv/bin/activate

# 3. Install existing deps
pip install -r requirements.txt

# 4. After adding a new package
pip install <package>
pip freeze > requirements.txt   # pin immediately
```

Before running any `pip install`, confirm the venv is active: `which python` must point to `.venv/bin/python`.

---

# Agentic Work Template

This template applies to every project. When starting a new project or resuming
work, initialize this structure and follow these conventions.

---

## Directory Structure

```
project-root/
├── plan.md                  ← orchestrator's source of truth
├── agents/
│   ├── shared/              ← read by all agents
│   │   ├── findings.md      ← researcher writes here
│   │   ├── decisions.md     ← engineer writes here
│   │   └── glossary.md      ← all agents append
│   ├── orchestrator/
│   │   └── tickets.md
│   ├── researcher/
│   │   ├── tickets.md
│   │   └── findings/        ← raw research, internal only
│   ├── engineer/
│   │   ├── tickets.md
│   │   └── workspace/       ← scratch code, spikes
│   └── documentor/
│       ├── tickets.md
│       └── drafts/          ← docs in progress
```

**Rules:**

- `agents/shared/` is read by all agents, written only by the agent that owns it
- Raw agent output stays inside `agents/<role>/` — never in `agents/shared/`
- `plan.md` is owned by the orchestrator; no other agent writes to it

---

## Ticket ID Scheme

Global, unique, never reused.

| Prefix | Agent        |
| ------ | ------------ |
| `O-`   | Orchestrator |
| `R-`   | Researcher   |
| `E-`   | Engineer     |
| `D-`   | Documentor   |

Format: `<PREFIX><zero-padded number>` — e.g. `E-007`, `R-012`, `O-001`

IDs increment monotonically. Closed tickets keep their ID. Never reassign.

---

## Ticket Format

```markdown
### E-007 · Implement ONNX embedding microservice

**Status:** in-progress
**Type:** implement
**Priority:** high
**Created:** 2026-04-05
**Updated:** 2026-04-05

**Description:**
Build a Python FastAPI microservice that accepts text input and returns
ONNX-generated embeddings. Must be containerized and expose a /embed endpoint.

**Blockers:**

- R-004 (researcher must finalize model choice before implementation begins)

**RCA:** _(fill only when a ticket is re-opened after a failure)_

> What broke, why it broke, what was misunderstood. One paragraph max.

**Artifacts:**

- engineer/workspace/embed_service/
- agents/shared/decisions.md → "Embedding Model Selection"

**Closed:** —
```

---

## Ticket Status Lifecycle

```
open → in-progress → [blocked] → in-progress → closed
                                              ↘ re-opened (attach RCA)
```

- **open** — identified, not started
- **in-progress** — actively being worked
- **blocked** — waiting on another ticket or external input; blockers field must be filled
- **closed** — done; artifacts recorded
- **re-opened** — was closed, found incomplete or broken; RCA required before resuming

---

## Ticket Types

| Type         | Meaning                                                                       |
| ------------ | ----------------------------------------------------------------------------- |
| `research`   | Investigate, read, synthesize — output goes to `agents/shared/findings.md`           |
| `implement`  | Write or modify code — output stays in `engineer/workspace/`                  |
| `coordinate` | Orchestrator only — open/close/sequence tickets across agents                 |
| `review`     | Evaluate prior output; may result in re-open + RCA                            |
| `document`   | Write user-facing or internal docs                                            |
| `spike`      | Time-boxed exploration with uncertain outcome; always time-box in description |

---

## Shared Surface Conventions

### `agents/shared/findings.md`

Owned by: **Researcher**
Format:

```markdown
## [R-003] Finding: NSF Award Data Schema

_Date: 2026-04-05_

One paragraph summary of what was found.
Key facts, links, or data points as a short list.
Confidence: high / medium / low
```

### `agents/shared/decisions.md`

Owned by: **Engineer**
Format:

```markdown
## [E-007] Decision: Embedding Model Selection

_Date: 2026-04-05_

**Decision:** Use all-MiniLM-L6-v2 via ONNX runtime.
**Rationale:** Fastest inference at acceptable quality for semantic search.
**Alternatives rejected:** OpenAI ada-002 (cost), BGE-large (too slow for batch).
**Revisit if:** Retrieval quality drops below threshold in eval.
```

### `agents/shared/glossary.md`

Owned by: **all agents** (append-only, no edits to prior entries)

```markdown
- **NDIF**: National Deep Inference Fabric — Bau Lab's inference infrastructure
- **NSF Ranker**: Saif's portfolio project for RA outreach
```

---

## `plan.md` (Orchestrator's Root Document)

```markdown
# Project: <name>

_Last updated: 2026-04-05 by O-001_

## Objective

One sentence. What does done look like?

## Current Phase

Phase 1 — Research & Scoping

## Active Tickets

| ID    | Agent      | Title                   | Status      |
| ----- | ---------- | ----------------------- | ----------- |
| R-003 | Researcher | NSF Award Schema        | in-progress |
| E-005 | Engineer   | Scraper worker skeleton | open        |

## Blocked

| ID    | Blocked By |
| ----- | ---------- |
| E-007 | R-004      |

## Completed This Session

- R-001 · Survey of NSF award API endpoints
- O-001 · Initialize project structure

## Next Orchestrator Action

Open E-005 once R-003 closes.
```

---

## Resume Prompt (paste at session start)

```
You are the <ROLE> agent for project: <PROJECT NAME>.

Your workspace: agents/<role>/
Your tickets: agents/<role>/tickets.md
Shared surface (read-only for you): agents/shared/

Your role:
<one line description of what this agent does and does not do>

Open your tickets.md. Find the highest-priority in-progress ticket.
If none, find the highest-priority open ticket and begin.
If blocked, state the blocker and do nothing until it resolves.

Do not touch agents/shared/ unless your role owns it.
Do not open tickets outside your prefix.
Report what you completed and what you're opening next.
```

---

## RCA Format (when re-opening a closed ticket)

```markdown
**RCA (Re-open: 2026-04-05):**

> E-006 was marked closed but the /embed endpoint returned incorrect dimensions
> for batch inputs > 32. Root cause: ONNX session was initialized without
> dynamic axes. Fix: re-initialize with dynamic_axes on input. Ticket re-opened.
```

RCA is one paragraph. It answers: what broke, why, what was wrong in the original understanding.

---

## `CHANGELOG.md` (Root-level, Orchestrator appends)

One entry per closed ticket, per session. Written by the orchestrator at session end — never mid-session.

```markdown
# Changelog

## 2026-04-05 · Session 3

- [R-003] Finalized NSF award data schema — findings in agents/shared/findings.md
- [E-005] Scraper worker skeleton complete — code in engineer/workspace/scraper/
- [O-002] Opened E-007, unblocked after R-003 close

## 2026-04-03 · Session 2

- [R-001] Surveyed NSF award API endpoints
- [R-002] Identified rate limit constraints on public API
- [E-004] Re-opened (see RCA on ticket) — embedding dimension bug
```

**Rules:**

- Append only. Never edit prior sessions.
- One line per closed ticket: `[ID] Title — where the artifact lives`
- Orchestrator coordination tickets (`O-`) are logged with what they opened or unblocked.
- If a session closes zero tickets, log it anyway: `## 2026-04-06 · Session 4 — no tickets closed (blocked on R-004)`

---

## Advanced Concepts

### 1. Agent Disagreement — `agents/shared/disputes.md`

When an agent reads the shared surface and disagrees with a prior conclusion, it does not silently proceed on its own assumption. It appends to `agents/shared/disputes.md`.

Owned by: **any agent** (append-only)

```markdown
## [E-007] Dispute: Embedding Model Selection

_Raised by: Engineer · Date: 2026-04-05_
_Disputes: R-003 finding in agents/shared/findings.md_

**Disagreement:**
R-003 recommends all-MiniLM-L6-v2, but batch inference at our scale will exceed
latency budget. Proposing BGE-small-en instead.

**Proceeding as:** BGE-small-en (spike in E-008)
**Resolution needed by:** before E-009 opens
**Resolved:** — (orchestrator fills this when resolved)
```

The orchestrator reviews open disputes before sequencing new tickets. Unresolved disputes block dependent work.

---

### 2. Confidence-Gated Sequencing

Every finding in `agents/shared/findings.md` carries a confidence level. The orchestrator enforces these rules when opening dependent tickets:

| Confidence | Orchestrator action                                                                     |
| ---------- | --------------------------------------------------------------------------------------- |
| `high`     | Dependent tickets open normally                                                         |
| `medium`   | Dependent implement tickets become spikes first                                         |
| `low`      | No implement tickets open; researcher must do a follow-up R- ticket to raise confidence |

This makes confidence actionable, not decorative.

---

### 3. Time-Boxing as a Ticket Primitive

Every ticket may carry two optional fields:

```markdown
**Estimated:** 2h
**Spent:** 5h
```

If `Spent` exceeds `Estimated` by 2x or more and the ticket is not closed, the agent must not continue. It must either:

- Split the ticket (see below), or
- Re-scope the description and update the estimate with a note

This is not productivity tracking. It is scope detection. A ticket 3x over estimate was misunderstood, not slow.

---

### 4. Ticket Splitting

When a ticket grows beyond its original scope mid-execution, it is not closed as complete. It is closed as `split`.

**Status addition:** `split`

```markdown
**Status:** split
**Split into:** E-011, E-012, E-013
**Reason:** Implementation revealed three distinct subsystems; original scope
was underspecified.
```

Child tickets are opened normally with their own IDs. The parent ticket is never re-opened — it exists only as a record of the split decision. The changelog logs it as:

```markdown
- [E-007] Split into E-011, E-012, E-013 — scope underestimated at open
```

---

### 5. Cross-Project Memory — `~/.agent-memory/`

A lightweight directory that lives outside any single project. The orchestrator reads from it at project initialization and appends to it when a project closes or a significant reusable decision is made.

```
~/.agent-memory/
├── infrastructure/
│   ├── embedding-service.md
│   ├── docker-multistage.md
│   └── scraper-architecture.md
├── research/
│   └── nsf-api-constraints.md
└── index.md                  ← one-line summary per entry, for fast scanning
```

Entry format:

```markdown
## Embedding Service Design

_From: NSF Ranker · Date: 2026-04-05_

FastAPI + ONNX runtime. Dynamic axes required for batch inputs.
Port to Go attempted and abandoned — ONNX Go bindings immature as of 2026-04.
Use Python microservice, containerized, expose /embed endpoint.

**Reuse condition:** Any project needing local embedding inference.
```

The orchestrator's resume prompt should include: _"Check ~/.agent-memory/index.md for prior decisions relevant to this project before opening research tickets."_ This prevents re-researching solved problems across projects.

---

# Research Thread Tracking

For research and conceptual discussions (not routine implementation), an answer
to one question routinely spawns several sub-questions. Following one loses the
others, and verbose answers make it hard to come back. This mechanism captures
every branch so none is lost and any can be resumed later.

## Threads vs. tickets

A **ticket** is a unit of *work*. A **thread** is an open *question* — a line of
inquiry that would expand understanding. A thread may graduate into a ticket
when it becomes work; a ticket may spawn threads. They are tracked separately.

## Thread ID Scheme

`T-` prefix, zero-padded, monotonic, never reused (e.g. `T-004`). Independent of
the `O-/R-/E-/D-` ticket counters.

## The Ledger — `threads.md` (project root, parallel to `plan.md`)

Threads form a **tree** — every thread records its parent, so its place in the
inquiry is always visible. One thread per entry:

```markdown
### T-004 · How do we determine the "Rome-ness" of an activation?

**Status:** open
**Parent:** T-002
**Opened:** 2026-08-04
**Question:** IIA condition 1 (encoded in SOURCE) assumes we can tell an
activation carries the location variable. By what measure? (probe? DLA? logit
lens projection?)
**Answer:** — (link to findings/notes when resolved)
```

Thread statuses:

- **open** — identified, not yet explored
- **active** — currently being followed
- **answered** — resolved; answer linked to a findings entry or notes file
- **parked** — was active, set aside deliberately; MUST remain resumable
- **dropped** — abandoned on purpose; record one line of why

## The Threads Block (end of every branching research answer)

When an answer opens or touches multiple threads, end the answer with a compact
block — the index, not more prose, is the antidote to verbosity:

```
Threads
- ACTIVE  → T-004 · determining "Rome-ness" of an activation
- opened  → T-005 · how to verify what BASE downstream actually consumes
            T-006 · "downstream consumption is itself a kind of neighbour"
- parked  → T-002 · the two conditions for an IIA flip
            T-003 · why mismatched-layer patches collapse IIA
```

## Rules

1. **Never silently drop a thread.** If a branch surfaces mid-answer, it gets a
   `T-` id in the ledger even if we do not follow it now.
2. **The user chooses the next thread.** Present the menu; do not auto-pick.
3. **One sentence per ledger line.** Terseness is the whole point.
4. **Always record the parent** so the tree is navigable.
5. **Parked ≠ dropped.** A parked thread must carry enough context to resume cold.
6. At session end, open + parked threads are the resume point — surface them.

---

# Premise Dry-Run

_The generative step. `threads.md` asks what I don't understand; the dry-run
produces the reframe worth designing around; the Design Protocol converges it;
tickets execute. Skipping it means designing around the first framing I happened
to arrive at, which is almost never the best one available._

Run it by **sitting with the premise and walking it, in prose, before touching
code or lenses.** Not brainstorming and not note-taking — a deliberate attempt to
find the structure underneath an intuition I already trust. The multi-chain move,
the expansion/contraction reframe, and the sign error in a live metric spec were
all only reachable this way; none of them would have come from reading the code
or filling in a template.

Trigger it when: an intuition keeps recurring but stays a metaphor; a result is
about to be designed around a framing nobody has stress-tested; two of my own
documents disagree and I can't say which is right; or the work feels correct but
uninteresting (usually means the frame is too close to the surface).

**Stuck-and-hard vs stuck-and-flat.** These call for opposite responses and are
easy to confuse. Stuck-and-hard means I lack information — go get it. Stuck-and-
flat means the frame is *excluding* something I already have, and more
information will not help; the corrective is not missing, it is suppressed by a
one-sided framing. Flatness, not difficulty, is the dry-run's signal.

## The moves

Not a checklist — nine operations that reliably pay. Take whichever bite.

### REFRAME — is the framing the best one available?

1. **Find the structure under the intuition.** When something is "obviously worse"
   or "clearly harder," ask *why* and refuse to stop at the metaphor. The answer is
   usually a structural property, and naming it makes the intuition measurable.
   _("Reverse jenga is worse" -> grounds are conjunctive, consequences are not.)_
2. **Rename into an existing formalism.** Check whether my novel distinction is a
   known one under another name. This is a gain, not a loss: it inherits decades of
   results instead of restating an observation, and it is the difference between a
   contribution and a re-derivation.
   _(forward/backward -> expansion/contraction, which imports AGM wholesale.)_
3. **Invert the difficulty.** Actively ask what happens if the hard direction is
   the easy one. The direction that *sounds* intractable often has a natural
   stopping condition the "easy" one lacks — and finding that is what makes an
   instrument buildable at all. **The difficulty judgment is usually a property
   of the frame, not the problem**, which gives the move a trigger: inversion
   pays in proportion to how committed I have been to one direction. The more
   settled the difficulty feels, the more likely it is a projection of the frame
   that produced it.

### OPERATIONALIZE — does the reframe produce work?

4. **Turn the asymmetry into an independent variable.** A reframe that doesn't
   yield a per-item quantity I can vary is still a metaphor. Push until it names a
   number and a predicted direction.
5. **Run the mirror, don't re-derive.** If a published result establishes X in one
   direction/regime, the mirror is usually unrun and is not re-derivation. Say
   explicitly which published finding I am mirroring, so the distinction survives
   review.
6. **Replace enumeration with measurement.** Wherever I am hand-writing the set I
   intend to measure, I have built in circularity. Look for a causal or
   interventional definition that *discovers* the set instead. This is often the
   move that dissolves the blocking problem rather than working around it.

### AUDIT — what does the new frame break?

7. **Separate structural from incidental.** Ask whether the failure belongs to the
   specific artifact or to its whole class. Class-level claims are stronger,
   falsifiable in one shot, and usually the real result.
8. **Re-run my own specs against the new frame.** A reframe silently invalidates
   things I already wrote down. Go back and check sign conventions, success
   criteria, and control-vs-signal assignments — a metric can be exactly inverted
   and still look reasonable until the frame changes.
9. **Salvage the reason from anything I cut.** When an idea is cut for good cause
   (usually scope), check whether the intuition that motivated it survives on its
   own. Keep the epistemology, drop the machinery. Same for banned words: ban the
   word, re-house the content it was carrying. **Vehemence is the marker:** what
   I cut most decisively, with the most moral energy, is the likeliest to be
   carrying something I still need. Sort the cut list by how sure I was.

## Where the output lives

Prose, in the answer or a session note. The PASS is what matters; a file is
optional and usually premature. Every dry-run ends with a **Threads block** — the
moves generate branches faster than anything else I do, and rule 1 of thread
tracking applies with full force.

## Rules

1. **Dry-run before designing, not after.** The Design Protocol converges on a
   frame; if the frame was never stress-tested, the ten lenses will polish the
   wrong thing rigorously.
2. **The intuition is data, the metaphor is not the finding.** Jenga, chains, and
   trees earn their place only by being cashed out into structure (move 1).
3. **Prefer the reframe that makes the work smaller.** If the new frame implies
   fewer experiments or removes a dependency, that is the strongest signal it is
   right. Difficulty is a cost (see Compass).
4. **Hold generative tension; do not resolve on schedule.** Distinguish tensions
   that *block* work (resolve now) from tensions that are *producing* (hold).
   Two coherent readings giving opposite answers on the same quantity is often
   not a defect to adjudicate but the pair whose held tension yields a third
   position neither contained. Forcing the choice destroys it. Deadlines are a
   bad reason to collapse a contradiction that is still generating.
5. **Record what the reframe broke.** Move 8 findings go in writing, RCA-style —
   a silently corrected spec looks like it was never wrong.
6. **These frames govern how I think, never what I claim.** Depth-psychology and
   philosophy-of-science vocabulary belongs to the *method* layer. It must not
   cross into claims about a system under study — an LM has no unconscious, and
   a structural resemblance between a measured phenomenon and a psychological
   one is a false friend that explains nothing and costs credibility. When the
   analogy feels too good not to use in a talk, that is the signal to cut it.
   If a lineage must be cited, cite the defensible spine — Peirce on abduction
   as a distinct inferential mode, Polanyi on tacit knowledge, Bachelard on the
   epistemological obstacle — and keep the rest for myself.
7. **A dry-run may conclude the question is not worth designing.** That is a
   success, and it is cheaper here than at the WHY gate.
8. **This protocol is subject to its own moves.** A nine-move method I trust is
   prior knowledge grown comfortable, which is Bachelard's obstacle wearing the
   costume of a method. The dry-run does not *produce* breakthroughs; it removes
   what blocks them — necessary, not sufficient. Concluding that the protocol is
   itself the obstacle on some question, and that the right move is to skip it
   and run the experiment, is a valid outcome. Beware also the reflexive case:
   when the method mirrors its subject, every session generates instances of the
   theory and confirmation comes free. That is a source of hypotheses with zero
   evidential weight. The only check is external — a prediction someone else can
   run that could come back negative.

---

# Research Design Protocol

Design is the CONVERGENT step between an open question and the work. `threads.md`
diverges (what don't we understand?); the **Premise Dry-Run** finds the framing
worth converging on; tickets execute (do the thing); design sits between and
decides *which tickets should exist at all*. Design the frame the dry-run
produced, not the first one that came to hand. Skipping design means
running experiments looking for results — the failure mode to avoid. Applies to
any non-trivial research/design question BEFORE opening implement tickets or
running experiments.

## The design lenses

Pass every research question through these ten lenses, **in order**. Each is a
question you must force-answer; the answers ARE the design. The order is a
workflow, not a checklist: establish it is *worth doing and not already taken*,
*then* pin the claim, *then* design a clean measurement, *then* bound the effort,
*then* stress-test. A question that has not passed the lenses is not ready to build.

### WHY — is it worth doing at all?

1. **Significance** — So what? Who benefits, and what *concretely* changes if the
   answer is yes versus no? If you cannot name what a confirmed result AND a denied
   result each change, the question is not worth designing. Rigor on a question
   nobody cares about is wasted rigor.
2. **Prior art & positioning** — What already exists, how does this differ in one
   sentence, and who is *actively* working on it (scoop risk)? Also: which datasets,
   models, and metrics should you adopt so your results are comparable to prior
   work? Do the literature search HERE (Asta, Semantic Scholar, ask an LLM) — before
   designing, not after. Prevents re-deriving a published result and being scooped.

### WHAT — what exactly is the claim?

3. **Completeness** — What sibling questions must also be answered for THIS answer
   to be *believed*? One result is an anecdote; the family is a study. The family
   defines the experiment set.
4. **Falsification** — What result would prove you WRONG? State confirm / deny /
   null outcomes in advance. If nothing could falsify it, it is not a hypothesis —
   it is a foregone conclusion. Pre-stating the null is what stops post-hoc
   rationalisation.

### HOW — can you measure it cleanly?

5. **Method & construct validity** — Two parts. (a) Enumerate EVERY method that
   could measure this; pick one; record the ones you are deferring and WHY (the
   reviewer trail). (b) Construct validity: does the chosen measure actually capture
   the construct you claim? ("Does cosine-at-layer *really* mean the facts are
   close?") Measuring the wrong thing precisely is still wrong.
6. **Confounds & controls** — What else co-varies with your independent variable?
   Name each confound and its control. An uncontrolled confound is a dead result —
   a reviewer names it and the finding evaporates.
7. **Baseline** — What is the dumbest explanation that could produce your result,
   and does your claim beat it? The real claim is usually "X predicts Y *after
   removing* the obvious baseline." No baseline = no result.

### HOW MUCH — can you actually do it?

8. **Scope & feasibility** — Two parts. (a) What is IN v1 and what is explicitly
   DEFERRED to v2? Bound the effort before starting, not after it overruns.
   (b) Feasibility: can you run it within your real compute / data / time budget
   (queue limits, dataset availability, hardware)? A complete design you cannot
   execute is not a design.

### CHECK — would it survive contact?

9. **Deliverable** — What is the ONE figure or number that carries the claim? Design
   backward from it; every experiment must serve producing it. Prevents collecting
   data with no destination.
10. **Adversary** — What would a hostile expert reviewer attack, and what is your
    pre-emption for each? Simulate the reviewer before the reviewer simulates you.
    This lens consumes the others: weak baseline (7)? uncontrolled confound (6)?
    cherry-picked setting? underpowered n? Answer each before it is asked.

## Where the output lives

The PASS is mandatory; the FILE is optional. For a small question the answers live
inline (in `plan.md`, a thread, or the ticket). For a full research project they
live in a `design.md` at project root (parallel to `plan.md`), structured by the
lenses. "No file yet" is never an excuse to skip the thinking.

## Rules

1. **Converge before you build.** No implement ticket opens and no experiment runs
   until its question has passed the lenses. (Research-shaped sibling of "no work
   without a ticket.")
2. **The WHY gate is a stop condition.** If Significance (1) or Prior art (2) fails
   — the question is trivial, already answered, or a scoop-in-progress — STOP. Do
   not design further. The cheapest place to kill a bad project is before it starts.
3. **No orphan experiments.** Every experiment must trace to a hypothesis (4) and
   serve the deliverable (9). If it serves neither, cut it.
4. **Record what you rejected.** Deferred methods (5) and known threats (10) are
   written down, not left implicit. Silence on them reads as "did not consider it."
5. **Design is iterative.** Results reshape the design; re-pass the affected lenses
   after each major result rather than treating the design as frozen.
6. **A wrong design decision is revisited, not buried.** When a choice proves wrong
   mid-project, note what broke and why (RCA-style), then change it — the same
   discipline as re-opening a ticket.

---

## Discipline Rules

1. Never mark a ticket closed without recording its artifacts.
2. Never open a ticket without a description — "figure it out" is not a description.
3. Blockers must reference a real ticket ID, not a vague concept.
4. Shared surface entries are append-only. Old entries are never edited.
5. An agent that touches another agent's workspace has broken the model.
6. RCA is required on every re-open. No exceptions.
7. **No work begins without a ticket open first.** Never start work and retroactively create a ticket.
8. **A ticket must be detailed enough for a second engineer to execute independently** — given only the ticket and agents/shared/, they should be able to implement correctly without asking questions. If not, split or add detail before starting.
