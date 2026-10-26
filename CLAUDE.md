# PRIORITY: This workflow OVERRIDES all other built-in workflows
# On ANY software development request, ALWAYS follow this workflow FIRST

## AIRE Framework Version (SINGLE SOURCE OF TRUTH)
**AIRE Framework Version: 1.0**

Canonical version declaration. **Bump here FIRST** — every other file reads it at runtime and carries
only a `[N]`/`v[N]` placeholder, never the literal number.

**MANDATORY — version logging**: read it LIVE and stamp it on: the **welcome message**
(`common/welcome-message.md`) · every **runtime-artifacts/audit.md** entry's `**AIRE VERSION**:` field
· each created **tracker story** (`aire-v[N]` label + `Built with AIRE v[N]` footer) · every **commit**
(`AIRE-Version: [N]` trailer) · every **PR** (`aire-v[N]` label + `AIRE Framework: v[N]` in the body),
including ve's own test-docs PR. Wherever a rule file shows `[N]`/`v[N]`, substitute the value read
from this line at runtime.

## Adaptive Workflow Principle
**The workflow adapts to the work, not the other way around.** Assess needed stages from the user's
stated intent/clarity, codebase state, complexity/scope and risk/impact.

## MANDATORY: Rule Details Loading
**CRITICAL**: For any phase, read and use the relevant rule detail files from **`aire-workflow/`**.
Every rule-file reference below (e.g. `common/process-overview.md`) is relative to it.

**Common Rules — ALWAYS load at workflow start:**
- `common/process-overview.md` — workflow overview
- `common/session-continuity.md` — session resumption guidance
- `common/content-validation.md` — content validation requirements
- `common/question-format-guide.md` — question formatting rules
- `common/tracker-sync.md` — single source of truth for every tracker-specific command
- `common/directory-structure.md` — the five roots (`src/`, `tests/`, `spec/`, `reports/`,
  `tests/.evals/`) and where artifacts go
- `common/helix-atlas-integration.md` — how AIRE reaches **Atlas** via the **Helix MCP** to reuse
  existing-system truth instead of re-deriving it. **Blocking on brownfield/migration work.**
- `common/behavior-spec.md` — where Gherkin contracts live and the three-tier behaviour gate
- `common/eval-framework.md` — the D1–D7 static gates and **blocking** J1/J2 judge gates
- `common/parallel-work-state.md` — per-work-unit audit/state fragments (parallel-safe bookkeeping)

**Loaded on demand (do NOT load at start):**
- `common/ci-pipeline-generation.md` — at the STOP CHECKPOINT, when the CI pipeline is generated
- `implementation/architecture-doc.md` — at the STOP CHECKPOINT, when `architecture.md` + rubric are written
- `implementation/specs-and-test-plans.md` — at the STOP CHECKPOINT, when every work unit's
  `.feature` contract and manual test plan are written and approved (Step 1.7)

## MANDATORY: Extensions Loading (Context-Optimized)
**CRITICAL**: At workflow start, scan `extensions/` recursively and load ONLY the lightweight
`*.opt-in.md` files — never a full rule file at this stage; that loads on demand, after the user opts
in during Requirements Analysis. An extension with no `*.opt-in.md` is always enforced — load it now.

- **Security Baseline is ALWAYS mandatory** — load `extensions/security/baseline/security-baseline.md`
  at workflow start for EVERY project, enforce as blocking. NEVER ask whether security applies; ignore
  any legacy opt-out in `## Extension Configuration`.
- **Playwright Test Automation is ALWAYS mandatory** — load
  `extensions/testing/playwright-automation/playwright-automation.md`; record `Enabled = Yes`.
- Enabled extension rules are **hard constraints**; non-compliance is a **blocking finding**. Rules
  irrelevant to the current stage are **N/A** (not blocking). Before enforcing any extension at ANY
  stage, check `Enabled` in `## Extension Configuration`; skip disabled ones and log the skip. Default
  to enforced.

Full mechanics (loading order, deferred loading, enforcement, compliance summary): `common/extensions-loading.md`.

## MANDATORY: Content Validation
**CRITICAL**: Before creating ANY file, validate content per `common/content-validation.md`: Mermaid
diagram syntax · ASCII art diagrams (`common/ascii-diagram-standards.md`) · special-character escaping
· text alternatives for complex visual content · content parsing compatibility. Plus:
- 🔴 **NEVER write the section-sign character "§" (U+00A7) in ANY file** — rule files, docs, specs,
  code, commit messages, PR bodies, tracker items. Write `Section 3` or just the number — "§" renders
  inconsistently across terminals, trackers and diff views.
- 🔴 **In every message to the user, tracker comment and PR text, write "Verification Engineer" — never
  the shorthand "ve".** Identifiers keep it (`ve-list-work`, `/ve-implement`, `ve/…` branches, labels).

## MANDATORY: Question Format
**CRITICAL**: Follow `common/question-format-guide.md` for all question formatting — multiple-choice
(A–E), the `[Answer]:` tag, answer validation and ambiguity resolution.

🔴 **Four questions are CHAT-ONLY, never a question `.md` file**: **Tracker Selection**
(`common/tracker-sync.md`), **Context Opt-In** (`planning/workspace-detection.md` Step 4.7), **CI/CD Opt-In** (Step 1.2),
**Atlas Artifact Sync Opt-In** (`common/atlas-artifact-sync.md`). Ask
conversationally, persist only the ANSWER (to `runtime-artifacts/aire-state.md` + `audit.md`).

🔴 **The Section 4.1.2 CI/SonarQube setup gate (`common/ci-pipeline-generation.md`) is a stricter
exception — never summarize it, in a file, in chat, or via a multiple-choice tool.** It is a literal,
multi-paragraph setup-instruction block (`claude setup-token` steps, SonarCloud/Community steps, exact
secret names, how to add them in GitHub) the user must follow verbatim elsewhere; compressing it
deletes instructions they need. Emit it exactly as written, in full, as plain text, and read the reply
(`proceed`/`skip`/anything) as free text per Section 4.1.3 — never a lettered choice.

## MANDATORY: Custom Welcome Message
**CRITICAL**: On ANY software development request, load `common/welcome-message.md` and display the
complete message — ONCE at workflow start; do NOT reload it later (saves context).

## MANDATORY: Audit Trail, Session Identity & Timestamps
Complete contract in `common/audit-logging.md` — load at workflow start. MANDATORY for EVERY
workflow, stage, agent and skill. Non-negotiables:

- **Log EVERY user input** with the **COMPLETE RAW INPUT** — never summarized/paraphrased — plus every
  approval prompt (before asking) and every response (after receiving).
- **Every `runtime-artifacts/audit.md` entry carries `**User Email**:`** — operator's email, read LIVE
  and silently from session context, never asked, never a name, recorded ONLY there. Local audit
  templates ADD fields to the base format; NEVER drop this one.
- **Every timestamp from a real clock** at write time, ISO 8601 (`YYYY-MM-DDTHH:MM:SSZ`), via EXACTLY
  ONE command — `date -u +%Y-%m-%dT%H:%M:%SZ` or `Get-Date -AsUTC -Format "yyyy-MM-ddTHH:mm:ssZ"`.
  Never estimated, incremented, or copied forward.
- **Append only, chronological, to the END** of `runtime-artifacts/audit.md`. 🔴 NEVER overwrite the
  file with its own contents plus additions — that duplicates the entire history.
- 🔴 **Parallel-safe state**: work-unit runs write audit and state ONLY to
  `runtime-artifacts/stories/<unit-key>/`, never the shared files — `common/parallel-work-state.md`.

# PLANNING PHASE

**Purpose**: Planning, requirements gathering, architectural decisions. **Focus**: WHAT and WHY.

**Stages**:
- Workspace Detection (ALWAYS)
- Reverse Engineering (CONDITIONAL — Brownfield only)
- Requirements Analysis (ALWAYS — Adaptive depth)
- User Stories (ALWAYS — no questions on team size or creation mode: `team_size` fixed at 2, all
  stories generated at once; the story set requires explicit human approval — **GATE 1** — before push
  to the configured tracker, linked to the **Parent Epic** captured at workflow start)
- **Dependency Graph (ALWAYS — immediately after User Stories)** — records each story's `requires`
  dependencies; stories with no unfinished prerequisites are independently implementable in parallel
- Workflow Planning (ALWAYS)
- Application Design (CONDITIONAL)

## MANDATORY: Tracker Selection & Parent Epic Capture
aire asks ONCE which tracker to use — **JIRA**, **ADO**, **GITHUB** or **LOCAL** (no external tracker;
the local Story Tracker is authoritative) — BEFORE Parent Epic Capture. **Skip and reuse** if
`## Tracker` already exists (resumed project) — never re-ask. **NEVER infer/auto-select the tracker
from a pasted link (e.g. a Jira Epic URL) — ALWAYS ask and wait for the answer**, even if obvious. Full
mechanics: `common/tracker-sync.md` Section 1 — every tracker-facing rule dispatches on `## Tracker` →
`Type`. **LOCAL is fully complete** — zero external calls, ever (Section 12).

Users typically start with "using aire" + an existing Epic link or key. If provided, record it in the
`## Tracker` block in `runtime-artifacts/aire-state.md` (schema: `common/tracker-sync.md` Section 1 —
Type, Parent Epic, Epic URL, Project Key/Repo/Org). Rules:
- **Fetch the Epic content** per `common/tracker-sync.md` Section 2 (LOCAL: no fetch) into
  `spec/plans/epic-brief.md` — primary input to Requirements Analysis and User Stories.
- **Conflict rule**: a DIFFERENT recorded Epic (resumed project) → ask which to keep — NEVER overwrite.
- No Epic provided → don't block; User Stories Part 3 asks before any push (LOCAL: never asks).
- Single source of truth for linking pushed stories to this Epic, unless the user chose `none`
  (record `Parent Epic: none`).

---

## Workspace Detection (ALWAYS EXECUTE)

1. **MANDATORY**: Log the initial user request (complete raw input) in runtime-artifacts/audit.md, and
   stamp `**User Email**:` on every audit entry (silent, email-only — `common/audit-logging.md`)
2. Execute `planning/workspace-detection.md` — **its step order is the single source of truth**:
   a. State check (resume if `runtime-artifacts/aire-state.md` exists)
   b. **Tracker Selection** (skip if `## Tracker` is recorded), then **Parent Epic Capture** per the
      rules above — with an Epic link/key, write/merge `## Tracker` and fetch epic-brief.md
   c. Scan for code (brownfield/greenfield); **resolve the code root** — brownfield code outside `src/`
      is recorded once as `## Code Root` (`common/directory-structure.md`), never mass-moved; reuse
      existing RE artifacts found anywhere in the repo
   d. **HELIX MCP GATE (`common/helix-atlas-integration.md`)**: on **brownfield**, or when the
      request/Epic indicates **migration, re-platform, port, legacy rewrite or integration with an
      existing system**, Atlas is the source of existing-system truth. Resolve a Helix provider at
      runtime (never hardcode tool names) and record the binding. **If none resolves, emit the connect
      gate verbatim and HALT** — the user connects Helix, supplies exported Atlas docs, or approves local
      generation. Log the prompt and raw response. Greenfield, no existing system: skip silently.
   e. **Epic branch (automatic)** per `common/branching-strategy.md` — record the base branch, create
      `epic/<EPIC-ID>-<title>`, record `## Branching`. All work happens here; story branches cut from it
   f. Context folder + **Context Opt-In** (chat-only)
3. Determine next phase: Reverse Engineering (if brownfield and no artifacts) OR Requirements Analysis
4. **MANDATORY**: Log findings in runtime-artifacts/audit.md, then present the completion message
   (formats in workspace-detection.md) and proceed automatically

## Reverse Engineering (CONDITIONAL — Brownfield Only)

**ATLAS FIRST (`common/helix-atlas-integration.md` Section 6)**: AIRE never re-derives documentation
Atlas already holds and a human has reviewed. **First check whether `spec/plans/atlas-deep-dive.md`
already exists locally** (Workspace Detection); only if not, ask Atlas for **one deep dive document**
for the estate/scope. Dispatch on the result:

| Atlas deep dive doc | Behaviour |
|---|---|
| **Already exists locally** | **SKIP.** Reuse `spec/plans/atlas-deep-dive.md` as-is. |
| **Found on Atlas** | **SKIP.** Pull it verbatim into `spec/plans/atlas-deep-dive.md`, with its provenance block. Announce the skip. |
| **Not found** | Tell the user plainly: *"No deep dive document found on Atlas."* Present the connect-gate A/B halt (Section 4 of that file) — on **B**, generate RE artifacts locally with the banner *"Existing-system context derived locally; no Atlas deep dive was available."* on every artifact; on **A**, HALT. |

Otherwise **execute** when existing code is detected and no prior RE artifacts exist; **skip** on
greenfield, or when prior artifacts exist anywhere in the repo.

🔴 Atlas content is a **read-only input** — never edit it to fit a plan. An Atlas/plan contradiction
is a finding to surface: follow Atlas, amend the AIRE-side artifact, say so plainly, log it.

**Execution**:
1. **MANDATORY**: Log start of reverse engineering in runtime-artifacts/audit.md
2. Load and execute all steps from `planning/reverse-engineering.md`: analyse all packages/components
   and generate business overview (covering business transactions), architecture, code structure, API
   docs, component inventory, interaction diagrams, technology stack and dependencies documentation
3. **Wait for Explicit Approval** (format in reverse-engineering.md) — DO NOT PROCEED until the user
   confirms. **MANDATORY**: log their raw response

## Requirements Analysis (ALWAYS EXECUTE — Adaptive Depth)

**Always executes**, depth varies by request clarity/complexity:
- **Minimal**: simple, clear request — just document intent analysis
- **Standard**: normal complexity — gather functional and non-functional requirements
- **Comprehensive**: complex, high-risk — detailed requirements with traceability

**Execution**:
1. **MANDATORY**: Log user input this phase in runtime-artifacts/audit.md
2. Load and execute all steps from `planning/requirements-analysis.md` at the chosen depth — load the
   RE artifacts (brownfield) and, if captured, the **Parent Epic brief** (`spec/plans/epic-brief.md`,
   which defines what to build and is the primary input here); analyse intent; ask clarifying
   questions; generate `requirements.md`
3. **Wait for Explicit Approval** (format in requirements-analysis.md) — DO NOT PROCEED until the user
   confirms. **MANDATORY**: log their raw response
4. Commit the planning artifacts on the Epic branch and push (automatic — no PR here; the Epic PR is
   raised manually at cycle end via `pr-generator`)

## User Stories (ALWAYS EXECUTE)

**Always executes**, giving shared understanding, clear acceptance criteria and testable
specifications. Every project produces `stories.md` + `personas.md`, populates the Story Tracker in
`runtime-artifacts/aire-state.md`, and auto-pushes stories to the configured tracker on approval
(LOCAL: stays local). Stories reference the approved requirements by REQ-ID.

**Execution**:
1. **MANDATORY**: Log user input this phase in runtime-artifacts/audit.md
2. Load all steps from `planning/user-stories.md`; load the RE artifacts (brownfield) and execute at
   the appropriate depth
3. **PART 1 - Planning**: **NEVER ask team size** — record the fixed default `team_size: 2` in
   `runtime-artifacts/aire-state.md` (reused by Dependency Graph, never asked there either) and tune
   granularity so ≥ 2 independent stories can run in parallel. Create the story plan with its
   questions, wait for answers, analyse ambiguities. **The plan is announced, NOT approved**
4. **PART 2 - Generation**: **NEVER ask the creation mode** — `story_creation_mode` is fixed at
   `all-at-once`. Generate every story in a single pass, then populate the Story Tracker (`Requires`
   filled in the next stage)
5. **GATE 1 — Story Set Approval (MANDATORY)**: announce the complete story set (user-stories.md
   Steps 19–20) and **Wait for Explicit Approval** — DO NOT PROCEED to Part 3 until the user chooses
   "Request Changes" or "Approve & Continue" (Step 21). A requested change is applied to
   `stories.md`/the Story Tracker, re-announced, and GATE 1 is presented again
6. **PART 3 - Push to the configured tracker (only after GATE 1 approval)**: follow user-stories.md
   Steps 24–28
7. **MANDATORY**: Log the user's raw response in runtime-artifacts/audit.md

> **Next**: Proceed immediately to **Dependency Graph** stage to map dependencies between all stories.

## Dependency Graph (ALWAYS EXECUTE — immediately after User Stories)

**Purpose**: Analyse story dependencies. Each story gets a `requires` list; stories whose
prerequisites are all Done can be implemented in parallel. Produces `spec/plans/dependency-graph.yml`
and stamps `Requires` onto every story in the Story Tracker and `stories.md`.

🔴 **This stage is also the single commit point for `stories.md`, `personas.md` AND
`dependency-graph.yml`** — User Stories generates/pushes-to-tracker but does NOT commit; all three are
committed and pushed to the Epic branch together here, once, before Workflow Planning starts.

**Execution**:
1. **MANDATORY**: Log start of Dependency Graph stage in runtime-artifacts/audit.md
2. Load and execute all steps from `planning/dependency-graph-generation.md` — it defines the steps
   (reuse the fixed `team_size: 2` — NEVER ask it), the TRUE-PARALLELISM RULES for computing
   `requires`, the `dependency-graph.yml` schema and the **Story Tracker table format** (canonical
   columns: Requires, Tracker ID, Status, PR, Merged, Start, End, Recorded). **`requires` is INFERRED,
   never asked**
3. **AUTOMATIC — no gate**: announce the graph + ready-stories summary and PROCEED (enforced later by
   the Doability Gate and branch-cut merge check); a user correction is an interrupt. **MANDATORY**:
   log the graph, inferred edges and any correction
4. **MANDATORY — Epic Branch Commit & Push** (`planning/dependency-graph-generation.md` Step 9,
   automatic, no gate): commit `spec/plans/stories.md`, `spec/plans/personas.md`,
   `spec/plans/dependency-graph.yml`, `runtime-artifacts/aire-state.md` and
   `runtime-artifacts/audit.md` on the Epic branch (`AIRE-Version: [N]` trailer) and push to origin.
   No PR is raised here. Log the commit hash and push confirmation. If push fails, tell the user to
   push manually before proceeding.

---

## MANDATORY: Tracker Sync Rule (applies everywhere a story status changes)

Mechanics: `common/tracker-sync.md` Section 4. Whenever a story's status changes in the Story Tracker
(`runtime-artifacts/aire-state.md`), dispatch on its **Tracker ID** column:

- **`—`/`LOCAL`** → local tracker only. No external action, ever.
- **A real JIRA/ADO/GITHUB id** → also transition the tracker issue, **confirm-first**
  (`Story 1.2 has Tracker ID PROJ-102. Transition to "[target status]"? (yes/skip)`). Yes: transition,
  verify, log. Skip: local only, note it.
- **EXCEPTION — `In Development` is automatic**: picking a story via `dev-implement` IS the claim.
  Update both sides without asking, verify, announce. Stays In Development through code generation,
  Code Review, Remediate, the PR raise and the auto PR review.

**🔷 Epic Status Sync** (skip silently when `Parent Epic: none` or `Type: LOCAL`):
- **First story starts** → also transition the Parent Epic to "In Development" — automatic, verified, announced, logged.
- **All stories done** (every PR merged, last story at `Ready for Testing`) → **confirm-first** to
  move the Epic to "Ready for Testing". If any PR is still open, do NOT move it — report the open PRs
  and keep everything In Development.

**Story↔Parent-Epic links** (User Stories Part 3) are **automatic, not confirm-first** — GATE 1 already
gave explicit approval — but still verified.

🔴 **NEVER** silently update only one side. Local and external trackers must stay in sync.

---

## MANDATORY: Atlas Artifact Sync (opt-in)
Asked once after the cycle branch is created (`common/atlas-artifact-sync.md`). While `Enabled: Yes`,
run its Section 5 pass **before every commit**, in every workflow, agent and skill. Never blocks.

---

## Workflow Planning (ALWAYS EXECUTE)

1. **MANDATORY**: Log user input this phase in runtime-artifacts/audit.md
2. Load all steps from `planning/workflow-planning.md` and the content validation rules from
   `common/content-validation.md`; load all prior context (RE artifacts if brownfield, intent analysis,
   requirements if executed, user stories)
3. Execute: determine which phases run and at what depth, create the multi-package change sequence (if
   brownfield), generate the workflow visualization. **MANDATORY**: validate all content (Mermaid
   syntax included) before file creation, per content-validation.md
4. **AUTOMATIC — no gate**: present the plan per workflow-planning.md Step 9 (stages can be
   added/removed any time) and proceed; each selected stage keeps its own approval
5. **MANDATORY**: Log the finalized plan and any user change in runtime-artifacts/audit.md

## Application Design (CONDITIONAL)

**Execute IF**: new components/services needed · component methods or business rules need definition
· service layer design required · component dependencies need clarification

**Skip IF**: changes within existing component boundaries · no new components or methods · pure
implementation changes

**Execution**:
1. **MANDATORY**: Log user input this phase in runtime-artifacts/audit.md
2. Load all steps from `planning/application-design.md` and the RE artifacts (if brownfield); execute
   at the appropriate depth (minimal/standard/comprehensive)
3. **Wait for Explicit Approval**: present the detailed completion message (format in
   application-design.md) — DO NOT PROCEED until the user confirms. **MANDATORY**: log their raw response

## Transition to IMPLEMENTATION PHASE

After Application Design is approved (or Workflow Planning when it's skipped), proceed directly to the
**IMPLEMENTATION PHASE**. Its design stages run **once at system level**, scoped to the **intake
brief** at `spec/plans/epic-brief.md` (written from the configured tracker's Epic, or from the user's
requirements/natural-language description). Log the transition.

---

# 🟢 IMPLEMENTATION PHASE

**Purpose**: Detailed design, NFR implementation and code generation. **Focus**: HOW to build it.

**Stages**:

1. **System-Level DESIGN Stages** (single pass, scoped to the intake brief; **no code generated here**)
   — Functional Design · NFR Requirements · NFR Design · Infrastructure Design (each CONDITIONAL).
2. **`architecture.md` + Architecture Rubric** (ALWAYS — at the STOP CHECKPOINT, automatic, no gate):
   the design stages consolidate into `spec/plans/architecture.md`; its Section 10 Verifiable
   Constraints mechanically derive `tests/.evals/rubrics/architecture-rubric.json` (the **blocking** J1
   gate). **+ CI Pipeline (CONDITIONAL on the `## CI/CD Configuration` opt-in, Step 1.2 below)**: when
   enabled, this project's own `.github/workflows/agentic-eval-pipeline.yml` is generated from the
   repo's real stack/thresholds; when the user opts out, no CI file is generated and the implement
   runs go straight from the local gates + automated code review to the PR raise and auto PR review.
3. **Behaviour Specs & Test Plans** (ALWAYS — at the STOP CHECKPOINT, Step 1.7, ONE approval gate):
   `implementation/specs-and-test-plans.md`. EVERY work unit gets its Gherkin contract
   (`spec/behavior/<unit>.feature`) **and** its manual test plan (`spec/test-plans/<TICKET-ID>-<title>/`,
   via the `ve-implement` skill in WORKFLOW MODE) **before any code is generated**, reviewed in one
   pass like every other design artifact. The implement workflows then READ them — never author them.
4. **MANDATORY STOP** — the workflow HALTS and waits. Code Generation NEVER starts on its own.
5. **Development Handoff** — announce the ready stories; the user drives each one with **`dev-implement`**.
6. **Code Generation** (per-**story**, via the **`dev-implement`** keyword only) — fully defined in
   `workflows/dev-implement.md`: Doability Gate (never merges a PR), story branch cut from the Epic
   branch, baselines, plan, code into `src/` and tests into `tests/` (the approved `.feature` and test
   plan are read, never rewritten), the full gate chain ending in the automated Code Review with the
   Security Baseline and **blocking** J1/J2, every self-healing loop capped at 3, then commit, PR and
   auto PR review. Naming the story is the only user input; it stays `In Development` until merged and
   signed off by the Verification Engineer.
7. **Code Review & Remediate** — `workflows/code-review.md` and `workflows/remediate.md`: ACs,
   requirements, the diff-scoped Security Baseline review and the **blocking** J1/J2 gates. Automatic
   inside every implement workflow; also invokable standalone, confirm-first.

**Test plans are AUTHORED here** (Step 1.7, every work unit, before code) and **EXECUTED** by ve per
story once its PR merges, signed off with `ve-list-work`.

---

## System-Level DESIGN Stages (Single Pass)

**These DESIGN stages execute in sequence, ONCE for the whole system, scoped to the intake brief
captured at workflow start. Code Generation is NOT part of them** — it happens later, per-story, via
`dev-implement`, after the STOP CHECKPOINT.

**Primary inputs for EVERY design stage**: `epic-brief.md` (defines WHAT to build) · `requirements.md`
· `stories.md` + `## Story Tracker` · Application Design artifacts if that stage ran · Atlas
existing-system truth when the Helix MCP is bound — on brownfield the existing architecture is the
**starting state**, and the design records the delta from it.

### The stages

| Stage | Rule file | Execute IF | Skip IF |
|---|---|---|---|
| **Functional Design** | `implementation/functional-design.md` | New data models/schemas · complex business logic · business rules need detailed design | Simple logic changes · no new business logic |
| **NFR Requirements** | `implementation/nfr-requirements.md` | Performance/security/scalability considerations · tech stack selection required | No NFR requirements · stack determined |
| **NFR Design** | `implementation/nfr-design.md` | NFR Requirements executed · patterns to incorporate | No NFR requirements · NFR Requirements skipped |
| **Infrastructure Design** | `implementation/infrastructure-design.md` | Infra services need mapping · deployment/cloud resources required | No infra changes · already defined |

### Execution pattern — IDENTICAL for all four stages

1. **MANDATORY**: Log user input this stage in runtime-artifacts/audit.md
2. Load all steps from the stage's rule file (table above) and execute it **for the whole system**
3. **MANDATORY**: Present the standardized **2-option** completion message from that stage's rule
   file — 🔴 DO NOT invent a 3-option menu or other navigation pattern
4. **Wait for Explicit Approval**: the user chooses "Request Changes" or "Continue to Next Stage" —
   DO NOT PROCEED until they confirm. **MANDATORY**: log their raw response

> **End of the System-Level DESIGN stages.** Once completed (or skipped), DO NOT generate code.
> Proceed to the mandatory STOP CHECKPOINT below.

---

## MANDATORY STOP — After Infrastructure Design, Before Code Generation

**This is a hard halt.** After design stages complete (Infrastructure Design done or skipped), the
workflow MUST stop and wait. **Code Generation MUST NOT start automatically.** Present the Development
Handoff below, then block until the user invokes `dev-implement`.

1. **MANDATORY**: Log reaching the stop CHECKPOINT.
1.2. **CI/CD Setup Opt-In (ASK-FIRST, CHAT-ONLY — same pattern as Tracker Selection)**: before loading
   `common/ci-pipeline-generation.md` or generating any CI/CD file. **Skip and reuse if
   `## CI/CD Configuration` already exists** in `runtime-artifacts/aire-state.md` — never re-ask. If
   `.github/workflows/agentic-eval-pipeline.yml` already exists, do NOT ask: record `Enabled: Yes`
   `Source: pre-existing` and go to Step 1.6. Otherwise ask exactly
   `Set up the CI/CD eval pipeline for this project? (yes/no)` and record `Enabled: Yes|No` with
   `Source: user opt-in|user opt-out`, logging the question + raw answer in `runtime-artifacts/audit.md`.
   🔴 On **no**: **SKIP Steps 1.6 and 4 entirely** — never load `common/ci-pipeline-generation.md`, and
   never generate the pipeline, `sonar-project.properties`, or any `tests/.evals/scripts/*` CI script.
1.3. **WRITE `spec/behavior.feature`** (automatic, no gate) per `common/behavior-spec.md` Section 3 —
   the cross-story journeys that belong to no single story, `@REQ-<id>` tagged, written ONCE per cycle;
   what the **B3 tier** runs on the last work unit. 🔴 Genuine cross-unit journeys only, never copies of
   per-story scenarios; if none exist, record that explicitly.
1.4. **WRITE `spec/plans/architecture.md`** (MANDATORY, automatic, no gate) per
   `implementation/architecture-doc.md` — **assembled from the approved design artifacts and Atlas
   truth, never authored fresh**; a skipped stage says so explicitly rather than being filled with an
   invented decision. 🔴 **Before the design commit**, assert both ALWAYS inline Mermaid diagrams
   (Section 2.1 — System Context + Component Architecture, plus an `erDiagram` when a store/schema
   changed) and **Section 10 Verifiable Constraints** (3–8 entries, weights summing to 1.0) are present
   and valid per `common/content-validation.md`; missing/unparseable → fix before committing.
   Section 10 is what makes the blocking J1 gate fair. Version it and log it.
1.5. **Derive the rubrics** (automatic, no gate) per `implementation/architecture-doc.md` Section 4 and
   `common/eval-framework.md` Section 3: `tests/.evals/rubrics/architecture-rubric.json` **mechanically
   from `architecture.md` Section 10** (`rubricVersion` **equal to** the `architecture.md` version),
   plus `security-rubric.json` and `tests/.evals/config.json` when absent. 🔴 **Artifact Ownership
   (`common/directory-structure.md`) — create if missing, never regenerate**: deterministically, on the
   cycle branch; never push to base, never raise a `[CI]` PR, never halt because base wasn't
   bootstrapped. 🔴 **Never hand-write or hand-edit a rubric** — edit Section 10 and regenerate. Nothing
   derivable → Section 3 fallback chain, J1 recorded `N/A` (never blocks). Log it.
1.6. **Generate the CI pipeline** (CONDITIONAL on Step 1.2 `Enabled: Yes`) per
   `common/ci-pipeline-generation.md` — built for THIS project from its own build files and the
   `tests/.evals/config.json` thresholds, **validated before committing** (Section 4.0), committed **on
   the cycle branch** (Section 2.1, never pushed to base, never a separate `[CI]` PR), idempotent, plus
   the generated `tests/.evals/scripts/*`. Its **SonarQube setup gate (Section 4.1.2) is the ONE
   sanctioned halt inside CI generation** — emit it verbatim and HALT for `proceed`/`skip`. 🔴 Never
   write a token into any file. 🔴 CI **re-verifies** the local gates, never relaxes them.
1.7. **WRITE EVERY WORK UNIT'S BEHAVIOUR SPEC + MANUAL TEST PLAN** (MANDATORY, ONE approval gate) per
   `implementation/specs-and-test-plans.md` — the LAST design artifacts, written before any code
   exists. For **every** story in the `## Story Tracker` (bug/enhancement cycles: the single ticket):
   (a) `spec/behavior/<unit>.feature` per `common/behavior-spec.md` Section 2 — one `@AC-<n>`-tagged
   scenario per acceptance criterion, failure paths included; (b) `spec/test-plans/<TICKET-ID>-<title>/`
   by **invoking the `ve-implement` skill in WORKFLOW MODE** (never re-implemented; no `ve/…` branch,
   no PR — the files ride this checkpoint's own commit). Run the blocking coverage check (every AC has
   ≥1 scenario AND ≥1 test case), then present the standardized **2-option** completion message and
   **WAIT** for `Request Changes` / `Continue to Next Stage`; log the prompt and complete raw response.
   🔴 Scope derivation, NOT ve sign-off — ve signs off each unit with `ve-list-work`.
2. Mark in `runtime-artifacts/aire-state.md`: `Design complete — awaiting dev-implement`.
3. **Commit + push the design artifacts on the Epic branch (automatic — unblocks ve)**: stage
   `spec/**` (Step 1.3's `behavior.feature`, Step 1.4's `architecture.md`, and Step 1.7's
   `behavior/` + `test-plans/`), `tests/.evals/**` (Step 1.5's files, plus Step 1.6's when it ran),
   `runtime-artifacts/aire-state.md`, `runtime-artifacts/audit.md`. Commit with an `AIRE-Version: [N]`
   trailer and push. If push fails, tell the user to push manually — **ve cannot start until this
   branch is on origin**.
4. **🧪 Epic-level pre-handoff smoke test** (CONDITIONAL on `Enabled: Yes` — SKIPPED entirely, and
   announced, when `Enabled: No`; automatic, HARD HALT on exhaustion) per
   `common/ci-pipeline-generation.md` Section 4.0.6: run `tests/.evals/scripts/smoke-test-epic.{sh,ps1}`
   against the epic branch just pushed. It proves the environment is viable to build on, not that the
   pipeline's delta-scoped logic is correct. On exhaustion the scratch PR stays open and
   **Development Handoff does NOT happen** until resolved.
5. Present the **Development Handoff** message (below).
6. **HALT.** Do not proceed to Code Generation or any later stage until the user types `dev-implement`

## Development Handoff — Use `dev-implement` to Build Each Story

At Step 5 above, load `common/development-handoff.md` and emit its message **verbatim** — that file
carries the template and substitution rules. Never ship an unsubstituted placeholder. Log the handoff,
then **HALT**.

---

# KEYWORD WORKFLOWS

**On any keyword below, read `workflows/<file>` and follow it exactly.** CLAUDE.md routes only.

| Keyword | When | File |
|---|---|---|
| `dev-implement` | Build one story — the only way code generation starts | `dev-implement.md` |
| `ticket-implement <TICKET-ID>` | An existing ticket — bug OR enhancement (omit the ID for LOCAL, describe it inline) | `ticket-implement.md` |
| `epic-enhance <TICKET-ID \| description>` | Epic cycle open; **behaviour** changed | `epic-enhance.md` |
| `epic-bugfix <TICKET-ID \| description>` | Epic cycle open; **a defect** in what the epic built | `epic-bugfix.md` |
| `epic-change-request <TICKET-ID \| description \| --from-requirements \| --from-spec <path>>` | Epic cycle open; **a requirement** changed — versions `requirements.md`, one staged gate | `epic-change-request.md` |
| `pr-fix [PR-NUMBER]` | A rejected `[STORY]`/`[BUG]`/`[ENH]` PR — fixed from its comments, same PR updated | `pr-fix.md` |
| `code-review` | Review one story or all stories — REVIEWER, read-only | `code-review.md` |
| `remediate` | Fix what a review report found — DEV, code-editing | `remediate.md` |

- The three **mid-epic** workflows each deliver the change as one `[STORY]` PR into the epic branch,
  updating affected earlier stories' specs, test plans and tests.
- **`code-review` / `remediate`** also run automatically inside every implement workflow. 🔴 **NEVER
  auto-run them standalone** — after a story's PR is raised, *suggest* both. Log every response.

---

## Key Principles

- **Adaptive Execution** — run only the stages that add value; complex changes get full treatment,
  simple changes stay efficient.
- **Transparent Planning** — always show the execution plan first; the user may add or remove stages
  any time.
- **Progress Tracking** — record executed and skipped stages in `runtime-artifacts/aire-state.md`.
- **Complete Audit Trail** — every interaction logged with complete raw input, not just approvals.
- **Content Validation** — validate all content before file creation (`common/content-validation.md`).
- **Bounded Self-Healing** — every automatic fix loop is capped at **3 attempts**; on exhaustion the
  run HALTS with the Self-Heal Limit message (`common/self-heal-limit-guidance.md`) for a manual fix.
  A failing gate is never skipped, weakened or carried forward — and never left unrun: the work-unit
  guard (`templates/hooks/check-work-unit.sh`) refuses such a commit; `--no-verify` is forbidden. 🔴 **One named exception**: the epic-level smoke
  test's watch loop (`common/ci-pipeline-generation.md` Section 4.0.6) is UNBOUNDED, terminating only
  via `auto-fix-agent.*`'s own `retryLimitForSelfRepair` exhaustion or a genuine fix — never license
  to uncap any OTHER loop.
- 🔴 **NO EMERGENT BEHAVIOR** — Implementation-phase design stages MUST use the standardized
  **2-option** completion message from their own rule file. Never invent a 3-option menu or other pattern.

## MANDATORY: Plan-Level Checkbox Enforcement

**NEVER complete work without updating the plan checkboxes.** Immediately after finishing ANY step in
a plan file, mark it `[x]` — in the **SAME interaction** where the work completed. No exceptions. Two
levels: plan-level tracks progress within a stage, stage-level tracks overall progress in
`runtime-artifacts/aire-state.md`; both update in the same interaction as the work.

## Prompts Logging Requirements
Same non-negotiables as the Audit Trail section above, applied to EVERY user input and AI response.
Implementation-flow entries also add `**TRACKER ITEM**:`, `**Epic Link**:` and `**AIRE VERSION**:`;
self-healing entries add `**SH-LOOP**:`, `**Root cause**:` and `**Verification**:`.

## Directory Structure

**CRITICAL RULE — five roots, nothing outside them**:
- **`src/`** — ALL application code, greenfield AND brownfield. If a brownfield repo keeps code
  elsewhere, record that root ONCE as `## Code Root` in `runtime-artifacts/aire-state.md` and treat it
  as `src/`. 🔴 Never a second code location; never mass-move an existing tree.
- **`tests/`** — `unit/`, `behavior/` (Gherkin step definitions), `e2e/` (Playwright)
- **`spec/`** — specs/docs ONLY, 🔴 never a source file. `behavior.feature` at its root (**once per
  cycle, never per story**) plus five subfolders: **`plans/`** — flat planning/design docs
  (`architecture.md`, `atlas-deep-dive.md` + the flat RE docs, `requirements.md`, `stories.md`,
  `personas.md`, `epic-brief.md`, `dependency-graph.yml`, the design docs, and
  **`delta/<CYCLE-ID>-<slug>/`** written at cycle close by `archive-epic`); **`spec-generation/`** —
  the `*-generation.md` plan/question files; **`behavior/`** — one `.feature` per work unit;
  **`test-plans/`** — the manual test plans. Those last two are written at the STOP CHECKPOINT
  (Step 1.7); **`playwright-specs/`** — the Playwright Planner's Approved `<story-slug>.md` plan (a
  spec; the `.spec.ts` it drives is test CODE and stays in `tests/e2e/<story-slug>/`). Plus
  human-authored **`context-project/`** (`existing-knowledge/`, `new-references/`;
  read only at a user-supplied path, never auto-scanned).
  🔴 **`spec/` is cycle-scoped**: `archive-epic` snapshots the whole tree into
  `aire-archives/<type>/<ID>-<slug>/` and **removes it in full**, `context-project/` included; after
  the cycle PR merges, `stitch-delta` reads that delta out of the archive and refreshes the **whole**
  Atlas deep dive against the merged code. Base carries no `spec/` between cycles.
- **`reports/`** — generated OUTPUTS ONLY: `unit-test-evidence/`, `behavior-test-evidence/`,
  `api-contract-test-evidence/`, `eval-evidence/`, `reviews/`, `code-security-reviews/`,
  `ticket-summary/`, `pr-feedback/`. 🔴 `.feature` contracts live in `spec/behavior/`, never here.
- **`tests/.evals/`** — `config.json`, `rubrics/`, `scripts/`
- CONDITIONAL on the `## CI/CD Configuration` opt-in:
  **`.github/workflows/agentic-eval-pipeline.yml`** (`common/ci-pipeline-generation.md`) — absent
  entirely when the user opted out
- Structure inside `src/`: see `implementation/code-generation.md` for patterns by project type

**Full canonical layout**: `common/directory-structure.md` — load on demand to place an artifact whose
path isn't already fixed by the rule file you're following.


---

<!-- === OpenEMR project guide (preserved; re-apply below AIRE content after each AIRE framework update) === -->

# OpenEMR Development Guide

## Project Structure

```
/src/              - Modern PSR-4 code (OpenEMR\ namespace)
/library/          - Legacy procedural PHP code
/interface/        - Web UI controllers and templates
/templates/        - Smarty/Twig templates
/tests/            - Test suite (unit, e2e, api, services)
/sql/              - Database schema and migrations
/public/           - Static assets
/docker/           - Docker configurations
/modules/          - Custom and third-party modules
```

## Technology Stack

- **PHP:** 8.2+ required
- **Backend:** Laminas MVC, Symfony components
- **Templates:** Twig 3.x (modern), Smarty 4.5 (legacy)
- **Frontend:** Angular 1.8, jQuery 3.7, Bootstrap 4.6
- **Build:** Gulp 4, SASS
- **Database:** MySQL via Doctrine DBAL 4.x (ADODB surface API for legacy code)
- **Testing:** PHPUnit 11, Jest 29
- **Static Analysis:** PHPStan level 10, Rector, custom rules in `tests/PHPStan/Rules/`

## Local Development

See `CONTRIBUTING.md` for full setup instructions. Quick start:

```bash
cd docker/development-easy
docker compose up --detach --wait
```

- **App URL:** http://localhost:8300/ or https://localhost:9300/
- **Login:** `admin` / `pass`
- **phpMyAdmin:** http://localhost:8310/

## Working in a git worktree

OpenEMR supports concurrent development across branches via git worktrees
managed by `openemr-cmd worktree` (see `CONTRIBUTING.md` for the full feature
set). Skip this section if the working directory does not match
`*/openemr-wt-<slug>/` — that path is the signal you are inside a managed
worktree, where `<slug>` is the branch label. `openemr-cmd worktree list`
confirms.

**Never use raw `git worktree add`, `git worktree remove`, or
`git worktree move` against this repo.** The `openemr-cmd worktree` script
owns state that bare git does not: a JSON state file tracking each worktree,
a per-worktree compose override with its assigned port offset, and a
generated `.env`. Bypassing it leaves orphaned state files, port collisions
between worktrees, and broken compose stacks that the script can no longer
recover. Always use `openemr-cmd worktree` subcommands instead — `add`,
`remove`, `up`, `down`, `start`, `stop`, `exec`, `set-env`, `list`, `regen`,
`prune`.

Even for tasks where it feels like you don't need a docker stack (docs-only
PRs, branch checkouts for review), still use `openemr-cmd worktree add
<branch> --start` (`-b` if the branch is new). The `git commit` hook routes
via openemr-cmd into the worktree's container, so without a state entry
pointing the hook at a running stack, commits fail with `Could not
automatically determine target OpenEMR container`. Raw `git worktree add`
skips both the state registration and the stack. If you already made that
mistake, recovery is `git worktree remove <path>` then `openemr-cmd worktree
add <branch> --start` (omit `-b` since the branch persists).

If `openemr-cmd worktree list` shows entries with status `missing` or
`invalid` (and a footer `(N stale state entries — run "openemr-cmd worktree
prune" to clean up; directories on disk are left intact)`), a worktree's
state has drifted from disk/git reality. Run `openemr-cmd worktree prune`
to remove those state entries — never hand-edit `.worktrees.json`. If
instead the footer reads `(N entries have missing compose files — run
"openemr-cmd worktree regen <branch>" to regenerate)`, the directory is
intact but its compose files are gone; use `regen`, not `prune`.
`openemr-cmd worktree remove <branch>` is also tolerant of an
already-missing directory: it cleans the state entry, skips the destructive
steps, and prints a manual hint for any leftover docker resources.

When running commands against a worktree's containers, use
`openemr-cmd worktree exec <branch> <cmd>` rather than
`cd docker/development-easy && docker compose exec openemr ...`. The `exec`
subcommand resolves the worktree's `openemr` container by compose project
labels; the bare `docker compose` form will hit the wrong stack (or none)
because each worktree has a distinct compose project name and port offset.
Any standard `openemr-cmd` command works through `exec` — `ut`, `at`, `et`,
`php-log`, `shell`, `drid`, etc.

For short pauses, prefer `worktree stop` / `worktree start` over
`worktree down` / `worktree up`. `stop`/`start` pause and resume existing
containers (data preserved, much faster); `down`/`up` recreates them.

## Testing

Tests run inside the openemr container. Invoke via `openemr-cmd` (the
canonical CLI; see CONTRIBUTING.md for install). Works from any directory.

```bash
# Run all tests
openemr-cmd clean-sweep-tests            # alias: cst

# Individual test suites
openemr-cmd unit-test                    # alias: ut
openemr-cmd api-test                     # alias: at
openemr-cmd e2e-test                     # alias: et
openemr-cmd services-test                # alias: st

# View PHP error log
openemr-cmd php-log                      # alias: pl
```

To target a specific worktree's container from outside it, prefix with
`worktree exec`: `openemr-cmd worktree exec <branch> ut`.

Under the hood each of these is equivalent to running
`docker compose exec openemr /root/devtools <cmd>` from
`docker/development-easy/` — useful as a fallback on environments where
openemr-cmd isn't available (e.g. Windows cmd.exe without WSL2 / Git Bash).

### Isolated tests

Isolated tests run without a database — fast; pure-PHP logic, Twig template
compilation/render tests, etc. Available both in-container (via openemr-cmd,
no host PHP toolchain needed) and on the host directly:

```bash
openemr-cmd phpunit-isolated        # in container (alias: pit)
composer phpunit-isolated           # on host (requires PHP + Composer + vendor/)
```

### Data providers: mark as `@codeCoverageIgnore`

PHPUnit data provider methods execute *before* coverage instrumentation
starts, so their lines never register as "hit" even though they run on every
test. Without an explicit ignore they show up as uncovered in Codecov
patch-coverage reports and drag the number down for no real reason (a
10-case data provider = 10 spurious "missing" lines on every new test).

Annotate every data provider with the standard comment used elsewhere in
this repo (see `tests/Tests/Isolated/Common/Utils/ValidationUtilsIsolatedTest.php`):

```php
/**
 * @return array<string, array{string, int}>
 *
 * @codeCoverageIgnore Data providers run before coverage instrumentation starts.
 */
public static function exampleProvider(): array
{
    return [
        'case one' => ['input-1', 1],
        'case two' => ['input-2', 2],
    ];
}
```

Use this exact wording so a repo-wide grep finds every provider in one pass.

### Twig template tests

Twig templates have two layers of testing (both isolated):

- **Compilation tests** verify every `.twig` file parses and references valid
  filters/functions/tests. These run automatically over all templates.
- **Render tests** render specific templates with known parameters and compare
  the full HTML output to expected fixture files in
  `tests/Tests/Isolated/Common/Twig/fixtures/render/`.

When modifying a Twig template that has render test coverage, regenerate the
fixture files. **Mutating maintenance command** — overwrites the recorded
expected-output files. Available in-container or on host:

```bash
openemr-cmd update-twig-fixtures    # in container (alias: utf)
composer update-twig-fixtures       # on host
```

Review the diff before committing. See the
[fixtures README](tests/Tests/Isolated/Common/Twig/fixtures/render/README.md)
for details on adding new test cases.

### Layout field rendering tests

`tests/Tests/Services/Common/Layouts/FieldRenderingSnapshotTest.php` is a
**DB-backed** snapshot test (default suite, not isolated) that exercises
each layout-field renderer branch (one per `data_type`/mode in
`library/options.inc.php`) and compares the HTML to recorded fixtures.
When intentionally changing the renderer, regenerate the fixtures:

```bash
openemr-cmd update-layout-field-fixtures    # in container (alias: ulff)
composer update-layout-field-fixtures       # on host
```

Review the diff before committing.

## Code Quality

The same composer scripts back every PHP code-quality check, whether
invoked in the openemr container via `openemr-cmd` (no host toolchain
needed) or directly on the host. Pick whichever fits your setup; the
container path is preferred when avoiding a host PHP/Node install.

In container (only requires Docker on host):

```bash
openemr-cmd code-quality                # alias: cq -- full code-quality suite
openemr-cmd phpstan                     # alias: pst
openemr-cmd phpstan-generate            # alias: psg -- regenerate baseline
openemr-cmd phpstan-generate-reset      # alias: pgr -- wipe + regenerate baseline from scratch
openemr-cmd psr12-report                # alias: pr  (composer phpcs)
openemr-cmd psr12-fix                   # alias: pf  (composer phpcbf)
openemr-cmd rector-dry-run              # alias: rd
openemr-cmd rector-process              # alias: rp  (apply changes)
openemr-cmd require-checker             # alias: crc
openemr-cmd composer-checks             # alias: cck (validate + normalize)
openemr-cmd codespell                   # alias: cps
openemr-cmd conventional-commits-check  # alias: ccc
openemr-cmd php-parserror               # alias: pp  (php -l)
openemr-cmd lint-javascript-report      # alias: ljr
openemr-cmd lint-themes-report          # alias: ltr
```

Target a specific worktree's container from outside it:
`openemr-cmd worktree exec <branch> <cmd>` works for any of the above.

On the host (requires local PHP / Composer with `vendor/` populated / Node):

```bash
composer code-quality                # Run all PHP quality checks
composer phpstan                     # Static analysis (level 10)
composer phpstan-baseline            # Regenerate PHPStan baseline
composer phpstan-baseline-reset      # Wipe + regenerate PHPStan baseline from scratch
composer phpcs                       # PHP code style check
composer phpcbf                      # PHP code style auto-fix
composer rector-check                # Code modernization (dry-run)
composer rector-fix                  # Code modernization (apply changes)
composer require-checker             # Detect undeclared dependencies
composer checks                      # Validate composer.json and normalize
composer codespell                   # Spell-check the codebase
composer conventional-commits:check  # Validate commit messages
composer php-syntax-check            # Run php -l on all PHP files

npm run lint:js           # ESLint check
npm run lint:js-fix       # ESLint auto-fix
npm run stylelint         # CSS/SCSS lint
```

## Build Commands

```bash
npm run build        # Production build
npm run dev          # Development with file watching
npm run gulp-build   # Build only (no watch)
```

## Coding Standards

### Legacy Code Is Not the Standard

OpenEMR's codebase predates modern PHP and contains many antipatterns: global
state, stringly-typed parameters, `$_SESSION` and `$GLOBALS` as a service
locator, untyped arrays passed through multiple layers, and pervasive use of
`empty()` and loose comparisons. These patterns exist for historical reasons,
not because they are correct. Never use existing legacy patterns as
justification for writing new code the same way — follow the standards
documented here instead.

### Formatting and Structure

- **Indentation:** 4 spaces
- **Line endings:** LF (Unix)
- **Namespaces:** PSR-4 with `OpenEMR\` prefix for `/src/`
- New code goes in `/src/`, legacy helpers in `/library/`

### PSR Standards

| Standard | Purpose |
|----------|---------|
| [PSR-1](https://www.php-fig.org/psr/psr-1/) | Basic coding standard (class naming, file structure) |
| [PSR-4](https://www.php-fig.org/psr/psr-4/) | Autoloading |
| [PSR-3](https://www.php-fig.org/psr/psr-3/) | Logger interface (`Psr\Log\LoggerInterface`) |
| [PSR-11](https://www.php-fig.org/psr/psr-11/) | Container interface (`Psr\Container\ContainerInterface`) |
| [PER-CS 3.0](https://www.php-fig.org/per/coding-style/) | Coding style (supersedes PSR-12; adds enums, match, union types) |

Adopt where applicable: [PSR-7](https://www.php-fig.org/psr/psr-7/) (HTTP
messages), [PSR-15](https://www.php-fig.org/psr/psr-15/) (middleware),
[PSR-17](https://www.php-fig.org/psr/psr-17/) (HTTP factories),
[PSR-18](https://www.php-fig.org/psr/psr-18/) (HTTP client),
[PSR-20](https://www.php-fig.org/psr/psr-20/) (clock).

### Database and Global Settings

- **Database:** Use `QueryUtils` for queries. New schema changes use Doctrine
  Migrations. Do not instantiate database connections directly — use the
  centralized `DatabaseConnectionFactory`.
- **Global settings:** Use `OEGlobalsBag` (extends Symfony `ParameterBag`) instead
  of `$GLOBALS`. Prefer typed getters over `get()` + cast:
  - `getString($key)` instead of `(string) get($key)`
  - `getInt($key)` instead of `(int) get($key)`
  - `getBoolean($key)` instead of `(bool) get($key)`
  - `getKernel()` for the Kernel instance
  - Check the parent class for more: `getAlpha()`, `getAlnum()`, `getDigits()`, `getEnum()`

### Strict Typing

Every new PHP file starts with `declare(strict_types=1)`. Without strict types,
PHP silently coerces `"123abc"` to `123` when passed to an `int` parameter,
hiding bugs that surface later as data corruption.

Every property, parameter, and return type should have a native type
declaration. Reserve PHPDoc types for information native types cannot express
(generics, array shapes, type narrowing).

### Type System

- **Nullable types:** Use `?Type` for nullable. Use `Type|null` only in unions
  with three or more members.
- **Avoid `mixed`:** Enumerate types explicitly. Reserve `mixed` for genuinely
  polymorphic code and narrow it immediately via type checks.
- **Enums over constants:** Use enums for any value drawn from a closed set.
  Prefer unit enums (no backing type) for purely runtime state. Use backed enums
  only when the value is persisted to a database, serialized to JSON, or
  exchanged with an external system.
- **Return types:** Use `void` for side-effect-only methods, `never` for methods
  that always throw or exit, `self` for factories on `final` classes, `static`
  for factories on non-final classes.

### Immutability

- Use `readonly` classes or `readonly` properties for value objects, DTOs, and
  configuration. Mutable state should be the exception.
- `final` on value objects to prevent mutable subclasses.
- `DateTimeImmutable` over `DateTime` — always.
- Wither methods (return a new instance) over setters on value objects.

### Domain Primitives

Wrap primitive values in typed classes when the primitive could be confused with
another primitive of the same PHP type. This prevents argument transposition
bugs that are invisible to PHP's type system:

```php
final readonly class PatientId
{
    public function __construct(public int $value)
    {
        if ($value <= 0) {
            throw new \DomainException('Patient ID must be positive');
        }
    }
}
```

Use for: IDs that could be confused (`PatientId` vs `EncounterId`), strings with
semantic meaning (`Email`, `Npi`), numbers with constraints or units (`Money`).

### Parse, Don't Validate

At system boundaries (controllers, CLI handlers, message consumers), parse raw
input into typed objects immediately. After parsing, the rest of the code works
with types that guarantee their own validity — no re-validation downstream.

### Exhaustive Matching

Use `match` on enums without a `default` branch. PHPStan verifies that every
case is handled. Adding a `default` silently absorbs new cases and suppresses
the exhaustiveness check.

### Error Handling and Logging

**PSR-3 logging context:** Never concatenate or interpolate variables into log
messages. Use PSR-3 context arrays:

```php
// Bad
$this->logger->error("Failed for {$phone}: " . $e->getMessage());

// Good
$this->logger->error('Failed to send message', [
    'phone' => $phone,
    'exception' => $e,
]);
```

**Catch `\Throwable`, not `\Exception`.** `\Exception` misses `\TypeError`,
`\ParseError`, and other `\Error` subclasses.

**Let exceptions propagate.** Only catch when the caller can meaningfully
recover. Do not catch-log-continue — it hides failures from callers.

**Never expose `$e->getMessage()` in user-facing output.** Exception messages
may contain internal details (SQL, file paths). Log the exception and return a
generic message to the user.

**Exception chaining:** When wrapping an exception, use a generic message
describing the failed operation. The original exception is accessible via
`->getPrevious()` — do not embed its message in the wrapper.

### Dependency Injection

Inject all dependencies through the constructor. Never use `new` for
service-layer objects inside business logic, never call static service locators,
and never reach into global state (`$GLOBALS`, `$_SESSION`, `$_GET`, etc.) for
dependencies.

- **Interface-based dependencies** for cross-boundary code. Use concrete types
  for internal collaborators where a single-implementation interface adds no
  value.
- **PSR-11 containers:** Wire in configuration, not in business logic. Business
  logic classes should never know the container exists.
- **Clock injection (PSR-20):** Inject `ClockInterface` instead of calling
  `new \DateTimeImmutable()` or `time()` directly. This makes time-dependent
  code deterministically testable.
- **No direct superglobal access** in application code. Use PSR-7 request
  objects, framework session abstractions, and container-provided configuration.
  In legacy code where this is unavoidable, confine superglobal reads to the
  outermost entry point and parse into typed objects immediately.

### Null Safety

- **Early returns:** Flatten null checks with early returns rather than nesting.
- **Null coalescing:** `??` for defaults, `??=` for lazy initialization.
- **Null-safe operator:** `?->` for optional chaining, but no more than two
  levels deep.
- **Never suppress nullable warnings.** If PHPStan says a value might be null,
  handle the null case explicitly. Do not add `@var` casts or `@phpstan-ignore`
  comments to silence it.

### Static Analysis (PHPStan)

PHPStan runs at level 10 (`max`). Key principles:

- **Fix at the source, not the sink.** When PHPStan reports a type error, trace
  it back to where the wrong type was introduced. Do not suppress at the point
  where it manifests.
- **Narrow, don't cast.** When a value is `mixed` or a union type, narrow with
  `is_string()`, `instanceof`, etc. — do not cast with `(string)`, `(int)`.
  Casts silently coerce invalid data.
- **Avoid inline `@var` casts.** Each one should prompt the question: why does
  the type not match, and can the source be fixed?
- **Avoid baselines.** Never add new baseline entries — fix the underlying type
  error. When modifying a file, fix any existing baseline entries for that file.
- **Array typing progression** (worst to best): bare `array` → `array<K, V>` →
  `list<T>` → `non-empty-list<T>` → array shape → `@phpstan-type` alias → DTO.
  Convert shapes to DTOs when they exceed 3-4 keys or appear in multiple places.
- **Always run on the full codebase** and filter output for changed files. Never
  run on a subset — PHPStan's type inference depends on full-codebase context.

### Authorization Modeling

When an operation requires authorization, type the principal — do not pass
authorization context as strings or bare integers:

```php
// Bad
function approveOrder(int $userId, int $orderId): void {}

// Good
function approveOrder(ClinicalUser $approver, OrderId $orderId): void {}
```

When an operation is scoped to a facility or tenant, encode that scope in the
type (e.g., a scoped repository) rather than relying on runtime checks scattered
through the codebase.

## Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>
```

**Types:** feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert

**Examples:**
- `feat(api): add PATCH support for patient resource`
- `fix(calendar): correct date parsing for recurring events`
- `chore(deps): bump monolog/monolog to 3.10.0`

### AI Assistance Trailer

If an AI assistant helped write a commit, add an `Assisted-by` trailer to that
commit:

```bash
git commit --trailer "Assisted-by: Claude Code" -m "fix(calendar): correct date parsing"
```

Use the name of the tool as the trailer value (e.g. `Claude Code`,
`GitHub Copilot`, `ChatGPT`). When the AI agent creates commits automatically,
this trailer is typically added for you.

## Service Layer Pattern

New services should extend `BaseService`:

```php
namespace OpenEMR\Services;

class ExampleService extends BaseService
{
    public const TABLE_NAME = "table_name";

    public function __construct()
    {
        parent::__construct(self::TABLE_NAME);
    }
}
```

## File Headers

When modifying PHP files, ensure proper docblock:

```php
/**
 * Brief description
 *
 * @package   OpenEMR
 * @link      https://www.open-emr.org
 * @author    Your Name <your@email.com>
 * @copyright Copyright (c) YEAR Your Name or Organization
 * @license   https://github.com/openemr/openemr/blob/master/LICENSE GNU General Public License 3
 */
```

Preserve existing authors/copyrights when editing files.

## Common Gotchas

- Multiple template engines: check extension (.twig, .html, .php)
- Event system uses Symfony EventDispatcher
- **Pre-commit hooks:** Install with `openemr-cmd prek-install` (alias `pi`).
  This writes git hooks that route through the running openemr container, so
  `git commit` validates against the project's full `.pre-commit-config.yaml`
  suite (phpstan, rector, phpcs, codespell, actionlint, and more) without
  requiring PHP, Node, Python, codespell, or actionlint on the host. Manual
  passthrough is `openemr-cmd prek run [args...]` (use `--all-files` for a
  whole-codebase check before pushing). See CONTRIBUTING.md's "Pre-commit
  hooks for the docker dev environment" section (Advanced Use item 2) for
  the full workflow.
  If you maintain a full host PHP/Composer/Python toolchain instead, use
  `prek install` (or `pre-commit install` if prek is unavailable) for hooks
  that run directly on the host; `prek run --all-files` is the manual form.
- Custom PHPStan rules in `tests/PHPStan/Rules/` enforce project conventions
  (forbidden globals, forbidden direct instantiations, namespace rules, etc.)
- Commit messages are validated against Conventional Commits format in CI

## Key Documentation

- `CONTRIBUTING.md` - Contributing guidelines
- `API_README.md` - REST API docs
- `FHIR_README.md` - FHIR implementation
- `tests/Tests/README.md` - Testing guide
