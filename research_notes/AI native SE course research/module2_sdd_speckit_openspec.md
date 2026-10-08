# Module 2: Specification-driven development tooling and practice (Spec Kit, OpenSpec, EARS, Kiro)

Research date: October 8, 2026. Version numbers were current on that date and will change. Primary sources (raw repo files, official docs) were fetched directly. Claims that come only from secondary sources are labeled.

## 1. GitHub Spec Kit: current version, install, `specify init` flags, exact slash commands, /checklist and /converge

### Takeaway
Spec Kit's latest release is **v1.1.2, dated October 7, 2026**. The CLI is installed with `uv tool install specify-cli`. The core SDD process has 10 agent commands: constitution, specify, clarify, plan, checklist, tasks, analyze, implement, **converge** and taskstoissues. **`/speckit.converge` is real and is a core command**. It was added in v0.11.2 (June 18, 2026), so any material written before mid-2026 will not mention it. `/speckit.checklist` is also a core command, used as an optional quality gate. The docs write commands in dotted form (`/speckit.plan`). How you actually type them depends on the agent: GitHub Copilot's default skills mode uses `/speckit-plan`, Codex uses `$speckit-plan`, and Kimi uses `/skill:speckit-plan`.

### Cited findings

**Version and history**
- Latest CHANGELOG entry: `## [1.1.2] - 2026-10-07`. Earlier entries include 1.1.1 (2026-10-06), 1.1.0 (2026-10-02) and 1.0.0 (2026-08-21) — [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md); [Releases](https://github.com/github/spec-kit/releases)
- v1.1.0 deprecated the core `taskstoissues` command in favor of a bundled `github` extension, and v1.0.13 (2026-09-29) added "bundled `github` extension for taskstoissues (#4488)" — [Releases](https://github.com/github/spec-kit/releases); [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)
- CHANGELOG v0.11.2 (2026-06-18): "feat: add /speckit.converge command (#3001)". v0.11.10 (2026-06-29): "Docs: Document /speckit.converge command (#3181)". v1.0.9 (2026-09-21): "fix: clarify converge assessment of completion claims (#4621)" — [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)
- CHANGELOG v0.11.8 (2026-06-24): "docs: run /speckit.checklist after /speckit.plan in quickstart (#3108)" — [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)
- CHANGELOG v0.12.12 (2026-07-13): "docs: document copilot skills mode (--skills) and markdown deprecation (#3313)" — [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)
- Older CHANGELOG entries also use unprefixed names, for example "feat(claude): run /analyze in a forked subagent (#2511)" and "/constitution's Sync Impact Report (#4431)". The `/speckit.clarify` row in the command table says "(formerly /quizme)" — [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md); [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- Announcement post: "Spec-driven development with AI: Get started with a new open source toolkit" by Den Delimarsky, published September 2, 2025. It describes four phases: Specify, Plan, Tasks, Implement — [GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- Microsoft developer blog post on Spec Kit, dated September 15, 2025 (seen in search results only; not fetched) — [Microsoft Developer Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit/)

**Install (quoted from the docs)**
- Prerequisites: "Python 3.11+, uv, and a supported AI coding agent on Linux, macOS, or Windows" — [README](https://github.com/github/spec-kit)
- Quick install, quoted:
  ```
  uv tool install specify-cli
  specify init my-project --integration copilot
  cd my-project
  ```
  — [README](https://github.com/github/spec-kit)
- Pinned source install, quoted: `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z`. The docs say to "keep the leading v". PyPI alternatives are `pipx install specify-cli` and `pip install specify-cli`. To pin a version: `uv tool install specify-cli==0.12.11`. Check the install with `specify version`; `specify self check` reports whether a newer release exists — [Installation guide](https://github.github.io/spec-kit/installation.html)
- Officially supported agents listed on the install page: "Claude Code, GitHub Copilot, CodeBuddy CLI, Gemini CLI, Pi Coding Agent, or Oh My Pi". The integrations reference has the full list of keys — [Installation guide](https://github.github.io/spec-kit/installation.html); [Integrations](https://github.github.io/spec-kit/reference/integrations.html)

**`specify init` flags (quoted from the docs)**
- `--integration <key>`, for example `claude`, `gemini`, `copilot`, `codebuddy`, `pi`, `omp`. Non-interactive runs "default to GitHub Copilot unless you pass --integration" — [Installation guide](https://github.github.io/spec-kit/installation.html)
- `--script sh|ps|py`. The default is `ps` on Windows and `sh` elsewhere — [Installation guide](https://github.github.io/spec-kit/installation.html)
- `--non-interactive` and `--ignore-agent-tools`. Example: `specify init my-project --non-interactive --ignore-agent-tools` — [Installation guide](https://github.github.io/spec-kit/installation.html)
- `--here` and `--force` for a non-empty or existing directory. Example: `specify init --here --force --non-interactive --integration claude` — [Installation guide](https://github.github.io/spec-kit/installation.html); [Existing projects](https://github.github.io/spec-kit/guides/existing-projects.html)
- Skills mode: "Integrations that support optional skills mode can select it with `--integration-options="--skills"`; some integrations, including Copilot, use skills by default." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- Git is now optional: "Git initialization and feature branches are optional and are managed by the git extension. Add it with `specify extension add git`." The active feature is tracked in `.specify/feature.json` (or the `SPECIFY_FEATURE_DIRECTORY` environment variable), not by the git branch — [Existing projects](https://github.github.io/spec-kit/guides/existing-projects.html); [Quickstart](https://github.github.io/spec-kit/quickstart.html)

**Exact command list (from the command overview in the Agentic SDD reference)**

| Command (reference notation) | Agent skill name | Purpose (quoted) |
|---|---|---|
| `/speckit.constitution` | speckit-constitution | Establish or update project principles |
| `/speckit.specify` | speckit-specify | Define requirements and user stories |
| `/speckit.plan` | speckit-plan | Create the technical implementation plan |
| `/speckit.tasks` | speckit-tasks | Break the plan into actionable tasks |
| `/speckit.implement` | speckit-implement | Execute the tasks |
| `/speckit.converge` | speckit-converge | Assess implementation against the artifacts and append remaining work |
| `/speckit.taskstoissues` | speckit-taskstoissues | Optionally convert tasks into GitHub issues |
| `/speckit.clarify` | speckit-clarify | Resolve ambiguity before planning (optional quality gate; formerly /quizme) |
| `/speckit.analyze` | speckit-analyze | Check artifact consistency after tasks and before implementation (optional quality gate) |
| `/speckit.checklist` | speckit-checklist | Generate requirements-quality checklists (optional quality gate) |

— [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)

- Canonical full order: `/speckit.constitution -> /speckit.specify -> /speckit.clarify -> /speckit.plan -> /speckit.checklist -> /speckit.tasks -> /speckit.analyze -> /speckit.implement -> /speckit.converge` — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- How names are typed in each agent, quoted: "Commands are written in /speckit.* form throughout this page. GitHub Copilot's default skills mode uses /speckit-*; some other agents use $speckit-* (e.g. Codex, ZCode) or /skill:speckit-* (e.g. Kimi)." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- The README examples use the hyphen form, and the README says: "These are agent skills, not terminal commands." — [README](https://github.com/github/spec-kit)
- **`/speckit.converge` behavior, quoted:** "Assesses the codebase against the feature's spec, plan, and tasks to confirm nothing was missed. It is append-only: it never edits or deletes code, and its only possible write is adding tasks to tasks.md. Run it only after /speckit.implement has run on the current tasks.md." There are two outcomes. **Converged**: "tasks.md is left byte-for-byte unchanged". **Tasks appended**: new tasks go "under a Convergence section in tasks.md ... Run /speckit.implement again ... then /speckit.converge once more. Each pass finds fewer items; repeat until it reports converged." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- README: "Repeat **implement → converge** until convergence reports **Converged**." — [README](https://github.com/github/spec-kit)
- **`/speckit.checklist` behavior:** described as "unit tests for your requirements". It checks whether the spec "is complete, clear, unambiguous, and consistent". Custom checklists are "reviewer-owned"; `[x]` "does not mean implementation work is complete". `/speckit.implement` reads checklist state as a gate and "asks before proceeding when any are unchecked". `/speckit.specify` also keeps a built-in `checklists/requirements.md` — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- **`/speckit.analyze`**: "read-only cross-artifact consistency and quality analysis across spec.md, plan.md, and tasks.md ... It never edits files." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
- **`/speckit.clarify`**: "Asks up to five targeted questions about underspecified areas of the current spec and encodes your answers back into spec.md." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)

**Extensions and catalog**
- Bundled opt-in extensions: `specify extension add bug` (`/speckit-bug-assess`, `/speckit-bug-fix`, `/speckit-bug-test`, writing to `.specify/bugs/<slug>/`), `specify extension add assess` (`/speckit-assess-intake|research|define|shape|decide`, writing to `.specify/assessments/<slug>/`), `specify extension add github` and `specify extension add git` — [README](https://github.com/github/spec-kit); [Reference overview](https://github.github.io/spec-kit/reference/overview.html)
- The extension system has four primitives: extensions, presets, workflows and bundles. There is a community catalog of extensions, presets, bundles and walkthroughs — [README](https://github.com/github/spec-kit); [Community overview](https://github.github.io/spec-kit/community/overview.html); [Reference overview](https://github.github.io/spec-kit/reference/overview.html)
- An experimental `specify mcp` command exists. v1.1.2 added MCP "artifact list" and "version" tools — [Reference overview](https://github.github.io/spec-kit/reference/overview.html); [CHANGELOG.md](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)

**Brownfield and spec persistence in Spec Kit**
- Spec Kit's own brownfield guidance: run `specify init --here --force --integration <key>` from a clean baseline. Write a constitution "with principles that are already true for the repository". Pick a bounded first change. Quoted: "The new spec.md defines the change you intend to make, not a retroactive specification of every existing behavior." — [Existing projects](https://github.github.io/spec-kit/guides/existing-projects.html)
- Three spec persistence models:
  - **Flow-forward**: a new feature directory for each change; old directories are kept as history.
  - **Living spec**: spec.md is the contract; plan and tasks are regenerated from it.
  - **Flow-back**: discoveries made during implementation flow back into the artifacts, which are then reconciled using `/speckit.analyze`.
  — [Evolving specs](https://github.github.io/spec-kit/guides/evolving-specs.html)

### Inferences
- Course materials should teach the dotted names (`/speckit.plan`), because the reference uses them. Students should then be told to type whatever form their agent accepts. Bare names such as `/constitution` and `/specify` are a pre-namespacing form that appears in older tutorials and the CHANGELOG; they should not be taught as current.
- The lab's "convergence loop" maps directly onto `/speckit.implement` → `/speckit.converge`, repeated until the agent reports Converged. No made-up command is needed.
- Spec Kit releases very often: several releases a week in September and October 2026. Pin the lab to a specific tag (for example `@v1.1.2`) so command behavior does not change during the term.

### Gaps
- I did not find the exact release in which bare `/specify` and `/plan` were renamed to `/speckit.*`. Only CHANGELOG references to unprefixed `/analyze` and `/constitution` were seen.
- I did not fetch the full integration-key list from the integrations page (431 lines). Use [Integrations](https://github.github.io/spec-kit/reference/integrations.html) for the complete list.
- The GitHub Releases page shows days but no years for the latest releases. Years were taken from the CHANGELOG.

## 2. Is "intent.md" a real artifact in any SDD tool?

### Takeaway
`intent.md` is **not** an artifact in Spec Kit, OpenSpec or Kiro. It is the standard file name in **IntentSpec**, an open format (MIT license) maintained by **Pathmode**. IntentSpec sits upstream of OpenSpec, and an adapter (`pathmode-intent`) feeds `intent.md` into OpenSpec. A separate site, IntentDocs, also promotes an "INTENT.md" format. Spec Kit's methodology document uses the phrase "intent-driven development" but defines no intent.md file.

### Cited findings
- "IntentSpec: an open format for **product intent**: what a change is for, who it affects, what must be true when it is done, and how anyone will know it worked." Canonical file: `intent.md`. Stewarded by Pathmode; "usable by anyone under MIT"; specification v1.2 (normative) — [pathmodeio/intentspec](https://github.com/pathmodeio/intentspec)
- intent.md sections: objective, outcomes, constraints, edgeCases, verification — [pathmodeio/intentspec](https://github.com/pathmodeio/intentspec)
- IntentSpec "is not a workflow". The `pathmode-intent` adapter carries `intent.md` into OpenSpec (intent.md → proposal → specs → design → tasks). The README content I fetched does not mention Spec Kit or Kiro — [pathmodeio/intentspec](https://github.com/pathmodeio/intentspec)
- IntentDocs describes "INTENT.md — an open format for developer intent": a single root file covering what is being built, for whom, and what done means, within an "Intent Loop" (Intent → Specification → Plan → Implementation → Verification → Updated intent). I saw this in a search snippet only and did not fetch the page — [IntentDocs](https://www.intentdocs.com/intent-md)
- Spec Kit's spec-driven.md: "The intent of the development team is expressed in natural language ("**intent-driven development**")". The file defines no intent.md artifact — [spec-driven.md](https://github.com/github/spec-kit/blob/main/spec-driven.md)

### Inferences
- If the syllabus lists "intent.md" next to spec.md, plan.md and tasks.md as if it were a Spec Kit artifact, the syllabus is wrong. Present it as an optional upstream "product intent" document (IntentSpec), or as a course convention, and label it as such.

### Gaps
- I could not confirm whether IntentDocs and Pathmode are the same organization, or how widely either format has been adopted.

## 3. OpenSpec (Fission-AI/OpenSpec): what it is, commands, delta-spec format, install, version

### Takeaway
OpenSpec is a lightweight SDD framework built for brownfield work. It keeps current truth in `openspec/specs/` and proposed changes in `openspec/changes/<change>/` (proposal.md, specs/ deltas, design.md, tasks.md). Archiving a change merges its delta specs (ADDED/MODIFIED/REMOVED) into the main specs. The current npm version is **1.14.1** (published October 5, 2026). Commands now use the **OPSX** form. The legacy `/openspec:proposal`, `/openspec:apply` and `/openspec:archive` are replaced by `/opsx:propose`, `/opsx:apply` and `/opsx:archive`.

### Cited findings
- Description: "spec-driven development (SDD) for AI coding assistants". The README describes it as brownfield-ready, tool-agnostic (30+ assistants) and plain Markdown — [OpenSpec README](https://github.com/Fission-AI/OpenSpec)
- Install, quoted: `npm install -g @fission-ai/openspec@latest` or `brew install openspec`, then `cd your-project` and `openspec init`. "Requires Node.js 20.19.0 or higher." — [OpenSpec README](https://github.com/Fission-AI/OpenSpec)
- Version: the npm dist-tag `latest` is 1.14.1, published 2026-10-05T23:28:44Z. There is also a beta tag, 1.6.0-beta.1 — [npm registry](https://registry.npmjs.org/@fission-ai/openspec); [CHANGELOG](https://github.com/Fission-AI/OpenSpec/blob/main/CHANGELOG.md)
- Default (`core` profile) commands: `/opsx:propose`, `/opsx:explore`, `/opsx:apply`, `/opsx:update`, `/opsx:sync`, `/opsx:archive`. Expanded commands: `/opsx:new`, `/opsx:continue`, `/opsx:ff`, `/opsx:verify`, `/opsx:bulk-archive`, `/opsx:onboard`. The expanded set is enabled with `openspec config profile` and then `openspec update` — [docs/commands.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/commands.md); [README](https://github.com/Fission-AI/OpenSpec)
- Conflict: the README's quick path lists only explore, propose, apply and archive, while commands.md also lists update and sync in the default profile — [README](https://github.com/Fission-AI/OpenSpec); [docs/commands.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/commands.md)
- Spelling varies by tool: "`/opsx:propose` is the canonical name; your tool may spell it `/opsx-propose` (Cursor, GitHub Copilot), `@opsx-propose` (Amazon Q) or `$openspec-propose` (Codex)." — [README](https://github.com/Fission-AI/OpenSpec)
- Mapping from legacy commands: `/openspec:proposal` → `/opsx:propose` (or `/opsx:new` then `/opsx:ff`); `/openspec:apply` → `/opsx:apply`; `/openspec:archive` → `/opsx:archive` — [docs/migration-guide.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/migration-guide.md)
- Directory layout: `openspec/specs/` holds the source of truth. `openspec/changes/<name>/` contains `proposal.md`, `specs/`, `design.md` and `tasks.md`. Completed changes move to `openspec/changes/archive/` — [README](https://github.com/Fission-AI/OpenSpec)
- Delta spec example (quoted excerpt):
  ```
  ## ADDED Requirements
  ### Requirement: Two-Factor Authentication
  The system MUST support TOTP-based two-factor authentication.
  #### Scenario: 2FA enrollment
  - GIVEN a user without 2FA enabled
  - WHEN the user enables 2FA in settings
  - THEN a QR code is displayed for authenticator app setup
  ## MODIFIED Requirements
  ### Requirement: Session Expiration
  The system MUST expire sessions after 15 minutes of inactivity.
  (Previously: 30 minutes)
  ## REMOVED Requirements
  ### Requirement: Remember Me
  (Deprecated in favor of 2FA. Users should re-authenticate each session.)
  ```
  — [docs/concepts.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)
- What happens to each section on archive:
  - ADDED: "Appended to main spec".
  - MODIFIED: "Replaces existing requirement".
  - REMOVED: "Deleted from main spec". Removing the last requirement retires the capability when `retire_capabilities: true` is set.
  - `## Purpose`: seeds a new spec.
  — [docs/concepts.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)
- Why OpenSpec uses deltas (quoted headings): clarity, conflict avoidance, review efficiency, and "Brownfield fit. Most work modifies existing behavior. Deltas make modifications first-class." — [docs/concepts.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md)
- Writing rules: "One statement, one `SHALL`/`MUST`". Requirements must be "Observable". OpenSpec uses RFC 2119 keywords — [docs/writing-specs.md](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)
- CI hook: `openspec validate <change> --strict`. Since 1.14.1, a requirement over 500 characters is a warning and fails strict mode — [CHANGELOG](https://github.com/Fission-AI/OpenSpec/blob/main/CHANGELOG.md)

### Inferences
- OpenSpec's delta model ("propose → apply → archive merges deltas") is the clearest way to teach brownfield change management. Spec Kit's brownfield answer is a choice among persistence models (flow-forward, living, flow-back), not a merge mechanism.
- OpenSpec scenarios use GIVEN/WHEN/THEN (BDD style), not EARS. EARS-style sentences can still be written inside the requirement line.

### Gaps
- The README fetch reported 71.3k stars. I did not check this independently, and it is not needed for teaching.

## 4. Other SDD tools: AWS Kiro, Tessl, BMAD

### Takeaway
Kiro uses three spec files: `requirements.md` (or `bugfix.md`), `design.md` and `tasks.md`. Requirements are written in EARS form (`WHEN ... THE SYSTEM SHALL ...`). Kiro supports Requirements-First, Design-First and Quick Spec workflows. Tessl is the example of "spec-as-source" in Böckeler's taxonomy. BMAD is an agile framework built around agent personas. I only gathered a light, secondary-sourced profile of Tessl and BMAD.

### Cited findings
- Kiro: specs are "structured artifacts that formalize the development process for features and bug fixes". The files are requirements.md (or bugfix.md), design.md and tasks.md. Spec types are Feature (Requirements-First, Design-First), Quick Spec and Bugfix — [Kiro Specs docs](https://kiro.dev/docs/specs/)
- Kiro feature specs: requirements follow `WHEN [condition/event] THE SYSTEM SHALL [expected behavior]` (EARS) for clarity, testability and traceability. This came from a summarized fetch; check the wording on the page before quoting — [Kiro Feature Specs](https://kiro.dev/docs/specs/feature-specs/)
- Related Kiro pages: [/docs/specs/bugfix-specs/](https://kiro.dev/docs/specs/bugfix-specs/), [/docs/specs/quick-spec/](https://kiro.dev/docs/specs/quick-spec/), [/docs/specs/best-practices/](https://kiro.dev/docs/specs/best-practices/) — [Kiro Specs docs](https://kiro.dev/docs/specs/)
- Tessl: Böckeler classifies the Tessl Framework as aiming for "spec-as-source" — [martinfowler.com](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html). The registry has a spec-driven-development tile and a spec-format page. Tessl docs include "Existing code to spec" — [Tessl registry tile](https://tessl.io/registry/tessl-labs/spec-driven-development/files); [Tessl docs](https://docs.tessl.io/common-workflows/existing-code-to-spec)
- BMAD-METHOD: canonical repo [github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD), docs at [docs.bmad-method.org](https://docs.bmad-method.org). It is an MIT-licensed, agent-persona-based agile framework installed with `npx bmad-method install`. **Secondary source only** (search summary; repo not fetched).

### Inferences
- Comparison along the spec lifecycle:
  - Kiro: EARS requirements, tied to the IDE.
  - Spec Kit: agent-agnostic, constitution plus quality gates, with a convergence loop.
  - OpenSpec: delta specs, built for brownfield.
  - Tessl: spec-as-source.
  - BMAD: persona/agile orchestration.

### Gaps
- **Unverified:** a search summary claimed that Tessl "in early 2026 shifted its product focus from specs to skills". This needs confirmation from tessl.io before use.
- **Unverified:** BMAD version (a secondary source said "v6.8") and star count. Not confirmed.
- The exact EARS wording on Kiro's page came from a summarized fetch. Verify it on the page before quoting.

## 5. EARS: origin, patterns, authoritative sources

### Takeaway
EARS was created by Alistair Mavin and colleagues at Rolls-Royce plc while they analyzed airworthiness regulations for a jet-engine control system. It was published at RE'09, the 17th IEEE International Requirements Engineering Conference (Atlanta, 2009, pp. 317–322). It has five basic patterns (ubiquitous, state-driven, event-driven, optional feature, unwanted behavior) plus combinations of them, called "complex".

### Cited findings
- Origin, quoted: "Mav and colleagues at Rolls-Royce PLC developed EARS whilst analysing the airworthiness regulations for a jet engine's control system." First published in 2009 — [alistairmavin.com/ears](https://alistairmavin.com/ears/)
- Paper: A. Mavin, P. Wilkinson, A. Harwood, M. Novak, "Easy Approach to Requirements Syntax (EARS)", 2009 17th IEEE International Requirements Engineering Conference (RE'09), Atlanta, GA, pp. 317–322 — [IEEE Xplore](https://ieeexplore.ieee.org/document/5328509); [University of Manchester record](https://research.manchester.ac.uk/en/publications/easy-approach-to-requirements-syntax-ears/). Bibliographic details came from search results. Confirm the IEEE document number (the search returned 6050274 for a related record) before citing.
- Generic syntax: "While <optional pre-condition>, when <optional trigger>, the <system name> shall <system response>". A requirement has zero or more preconditions, zero or one trigger, one system name, and one or more responses — [alistairmavin.com/ears](https://alistairmavin.com/ears/)
- Patterns:
  - **Ubiquitous**: "The <system name> shall <system response>"
  - **State-driven** (While): "While <precondition(s)>, the <system name> shall <system response>"
  - **Event-driven** (When): "When <trigger>, the <system name> shall <system response>"
  - **Optional feature** (Where): "Where <feature is included>, the <system name> shall <system response>"
  - **Unwanted behaviour** (If/Then): "If <trigger>, then the <system name> shall <system response>"
  - **Complex**: "While <precondition(s)>, When <trigger>, the <system name> shall <system response>"
  — [alistairmavin.com/ears](https://alistairmavin.com/ears/)
- The paper addressed common problems with natural-language requirements such as ambiguity, complexity and vagueness. Its rules let all requirements be expressed with five templates — [Wikipedia: EARS](https://en.wikipedia.org/wiki/Easy_Approach_to_Requirements_Syntax) (secondary source)

### Inferences
- Teach EARS as the "requirement sentence" layer and GIVEN/WHEN/THEN scenarios (OpenSpec) or acceptance criteria (Spec Kit) as the "example/test" layer. Kiro puts EARS directly into requirements.md.

### Gaps
- IEEE Xplore was not fetched directly, so the exact document number and DOI are unconfirmed.

## 6. Critiques and limits of SDD

### Takeaway
The strongest primary critique is Birgitta Böckeler's article "Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl" on martinfowler.com (October 15, 2025). It covers workflow size mismatch, markdown review overhead, agents ignoring specs, and the echo of model-driven development. Thoughtworks' Technology Radar placed "Spec-driven development" in **Assess** (November 2025) with a "bitter lesson" warning.

### Cited findings
- Title: "Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl". Author: Birgitta Böckeler. Date: October 15, 2025 — [martinfowler.com](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)
- Taxonomy of three levels:
  - **Spec-first**: the spec is written first and discarded after the task.
  - **Spec-anchored**: the spec is kept and evolves with the feature.
  - **Spec-as-source**: humans edit only the spec (Tessl's aspiration).
  — [martinfowler.com](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)
- Critiques:
  - Fixing a small bug in Kiro produced 4 user stories and 16 acceptance criteria ("sledgehammer to crack a nut").
  - "I'd rather review code than all these markdown files."
  - "Just because the windows are larger, doesn't mean that AI will properly pick up on everything."
  - Agents ignored some instructions and over-applied others.
  - The line between functional and technical content is unclear.
  - Spec-as-source risks combining "inflexibility *and* non-determinism", as model-driven development (MDD) did.
  — [martinfowler.com](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html) (wording from a summarized fetch; check quotes against the page)
- Thoughtworks Radar blip "Spec-driven development", ring **Assess**, dated Nov 05, 2025. Tools named: Amazon Kiro, GitHub spec-kit, Tessl Framework. Quoted: "We may be relearning a bitter lesson — that handcrafting detailed rules for AI ultimately doesn't scale." The page says the blip "is not on the current edition of the Radar" — [Thoughtworks Radar](https://www.thoughtworks.com/radar/techniques/spec-driven-development)
- **Conflict on volume number:** the fetched page reported "Volume 34", but a secondary search summary said "33rd edition (November 2025)". By the usual cadence (Vol 32 April 2025, Vol 33 November 2025), Volume 33 looks more likely. Check the page before citing.
- Tool-maintainer counterweights to the "too heavy" critique:
  - Spec Kit's own docs make clarify, checklist and analyze optional quality gates and offer a "Shorter path — for smaller features".
  - Spec Kit has a brownfield guide that warns against retroactively specifying the whole system.
  — [Quickstart](https://github.github.io/spec-kit/quickstart.html); [Existing projects](https://github.github.io/spec-kit/guides/existing-projects.html)
- A secondary search summary claimed that in January 2026 "Kent Beck and Martin Fowler" criticized writing the entire specification up front. **Unverified**; I found no primary source.

### Inferences
- Böckeler's critique was written against Spec Kit as of October 2025, before converge, extensions and the brownfield guides existed. Present it as still valid on review overhead and agent reliability, with the caveat that tooling has since responded on workflow sizing.

### Gaps
- Whether the Radar blip reappeared in Vol 34 (April 2026) or Vol 35 (autumn 2026) was not confirmed.
- No primary source found for the Kent Beck / Fowler January 2026 claim.

## 7. A realistic step-by-step Spec Kit lab (commands quoted from the official docs)

### Takeaway
Every command below comes from the Spec Kit README, Quickstart or Agentic SDD reference. The "refine specs only" rule is the lab's own constraint, not a Spec Kit feature: students may change only spec.md, plan.md, the constitution and checklist artifacts, and must drive code changes through `/speckit.implement` and `/speckit.converge`.

### Cited findings (lab sequence; terminal commands vs agent-chat skills)
1. Terminal: `uv tool install specify-cli==<pinned>` (or `--from git+https://github.com/github/spec-kit.git@vX.Y.Z`), then `specify version` — [Installation guide](https://github.github.io/spec-kit/installation.html)
2. Terminal: `specify init taskify --integration copilot` (or `--integration claude`), then `cd taskify`. Add `--non-interactive` in CI — [Quickstart](https://github.github.io/spec-kit/quickstart.html)
3. Optional: `specify extension add git` for numbered feature branches — [Existing projects](https://github.github.io/spec-kit/guides/existing-projects.html)
4. Agent chat: `/speckit-constitution Taskify is a "Security-First" application. All user inputs must be validated. ...` — [Quickstart](https://github.github.io/spec-kit/quickstart.html)
5. `/speckit-specify Develop Taskify, a team productivity platform where predefined users create projects, assign tasks, comment, and move tasks across Kanban columns (To Do, In Progress, In Review, Done). ...` — [Quickstart](https://github.github.io/spec-kit/quickstart.html)
6. `/speckit-clarify Focus on task card behavior — status changes, comment permissions, and user assignment.` (up to 5 questions per run; repeat as needed) — [Quickstart](https://github.github.io/spec-kit/quickstart.html); [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
7. `/speckit-plan <stack, architecture, constraints>` — [Quickstart](https://github.github.io/spec-kit/quickstart.html)
8. `/speckit-checklist`, then a human reviewer marks items `[x]` — [Quickstart](https://github.github.io/spec-kit/quickstart.html)
9. `/speckit-tasks` produces phases: Setup, Foundational, one phase per user story, then Polish — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
10. `/speckit-analyze`. If it finds issues, fix them at the source (specify/clarify, plan or tasks) and re-run until it comes back clean — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
11. `/speckit-implement`. For large features, scope it, for example "Implement only the Setup and Foundational phases ..." — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
12. `/speckit-converge` → repeat `/speckit-implement` + `/speckit-converge` until it reports "✅ Converged" — [Agentic SDD reference](https://github.github.io/spec-kit/reference/agentic-sdd.html)
13. Change-request round (living-spec model): edit spec.md (or run `/speckit-clarify`), then re-run plan → tasks → analyze → implement → converge — [Evolving specs](https://github.github.io/spec-kit/guides/evolving-specs.html)

### Inferences
- Suggested lab grading evidence:
  - Git diffs showing that students edited only artifacts.
  - The number of converge passes needed to reach Converged.
  - The `/speckit-analyze` reports.
  - The reviewed checklists.
  - For a brownfield extension, repeat the change in OpenSpec (`/opsx:propose` → `/opsx:apply` → `/opsx:archive`, plus `openspec validate --strict`) to contrast delta specs with Spec Kit's persistence models.

### Gaps
- I did not run the commands. Agent output varies between runs, and the number of converge passes cannot be predicted.
- Copilot is the default integration. Each student's agent needs to be checked against the "Command invocation" table before the lab.
