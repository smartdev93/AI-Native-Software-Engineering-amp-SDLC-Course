# Module 4: Cognitive debt, code review, and responsible AI governance

!!! abstract "Core focus"
    Keeping human understanding of a system as AI writes more of it, designing code review with AI in the loop, defending agents against prompt injection and supply-chain attacks, and knowing the governance, copyright, and authorship rules that apply. The module ends with the capstone.

!!! warning "Version note (as of October 2026)"
    Checked October 8, 2026. Security incidents, OWASP lists, open-source AI policies, and EU AI Act dates change often. The Linux kernel's attribution tag was under discussion again in 2026, and the EU AI Act Omnibus entry-into-force date differs between sources. Recheck the links before teaching.

## Learning objectives

By the end of this module, students can:

1. Distinguish individual "cognitive debt" (Kosmyna et al.) from team-level cognitive debt (Storey) and state the limits of the evidence for each.
2. Use the Anthropic skill-formation RCT to choose AI interaction patterns that preserve learning.
3. Design a review gate that uses AI review without replacing human approval, based on field evidence.
4. Explain prompt injection, the lethal trifecta, and package hallucination, and analyze a real incident.
5. Name the main governance documents (NIST AI 600-1, SP 800-218A, OWASP lists) and the US Copyright Office position on AI-generated material.
6. Write Architecture Decision Records that capture why an AI-proposed design was accepted.

## Lecture topics

### 1. Cognitive debt

- **The MIT preprint.** Kosmyna et al., "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task," arXiv:2506.08872, submitted June 10, 2025 (v2 December 31, 2025). 54 participants wrote SAT-style essays in three groups (LLM, search engine, brain-only) over three sessions; only 18 completed the fourth, crossover session. Methods: EEG, NLP analysis, and scoring by teachers and an AI judge. Finding: "LLM users consistently underperformed at neural, linguistic, and behavioral levels." ([arXiv](https://arxiv.org/abs/2506.08872); [MIT Media Lab](https://www.media.mit.edu/publications/your-brain-on-chatgpt/))
- **Limits to state in class:** it is a preprint with no journal acceptance shown; the task was essay writing, not coding; only 18 people completed the crossover; participants came from a small set of elite universities. Do not generalize it to developers. (The authors' own limitations section was not reviewed for this course.)
- **Storey's software-engineering meaning.** Margaret-Anne Storey, "How Generative and Agentic AI Shift Concern from Technical Debt to Cognitive Debt" (February 9, 2026), defines cognitive debt as the erosion of a team's shared theory of what the software does and how to change it, building on Peter Naur's "programming as theory building." Her example: a student startup team stalled in weeks 7–8 because no one could explain the design decisions; they blamed messy code, but the problem was lost understanding. ([margaretstorey.com](https://margaretstorey.com/blog/2026/02/09/cognitive-debt/))
- **Storey's mitigations:** at least one human fully understands each AI-generated change before it ships; document why, not only what; hold regular checkpoints; watch for warning signs (people hesitate to modify code, knowledge stays tribal, the system becomes a black box). ([margaretstorey.com](https://margaretstorey.com/blog/2026/02/09/cognitive-debt/); follow-up: [Cognitive debt revisited](https://margaretstorey.com/blog/2026/02/18/cognitive-debt-revisited/))

### 2. Skill formation and critical thinking

- **Anthropic RCT.** Judy Hanwen Shen and Alex Tamkin, "How AI assistance impacts the formation of coding skills" (January 29, 2026; [arXiv:2601.20245](https://arxiv.org/abs/2601.20245)). 52 mostly junior engineers learned Trio, an unfamiliar Python async library. Quiz scores: AI group 50%, control 67% (Cohen's d = 0.738, p = 0.01), with the biggest gap on debugging questions. The AI group finished about 2 minutes faster, which was not statistically significant. ([Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills))
- **How you use AI matters.** Low scores went with delegating to AI, relying on it more over time, and debugging by iterating with it. High scores went with generating code then working to understand it, asking for code plus explanation, and asking conceptual questions. Stated limits: small sample, a quiz right after the task, no evidence on long-term skill. ([Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills))
- **Self-reported critical thinking.** Lee et al. (CHI 2025, Microsoft Research and CMU) surveyed 319 knowledge workers (936 examples): higher confidence in GenAI went with less critical thinking; higher self-confidence went with more. This measures self-report, not skill. ([PDF](https://advait.org/files/lee_2025_ai_critical_thinking_survey.pdf))

Teaching point: AI use is not inherently harmful; the interaction pattern decides the outcome. Practice "generate, then explain."

### 3. Code review with AI in the loop

- **Tools.** GitHub Copilot code review became generally available on April 4, 2025 ([changelog](https://github.blog/changelog/2025-04-04-copilot-code-review-now-generally-available/)), with `copilot-instructions.md` customization GA on August 6, 2025 ([changelog](https://github.blog/changelog/2025-08-06-copilot-code-review-copilot-instruction-md-support-is-now-generally-available/)). Claude Code has official [GitHub Actions](https://code.claude.com/docs/en/github-actions) that can review PRs. A third-party guide says Copilot leaves "Comment" reviews only and never approves or blocks; this is **to verify** against GitHub's docs.
- **Field evidence:**
    - Google's ML-suggested edits: authors address 7.5% of reviewer comments by applying the suggested edit (May 23, 2023; paper at ICSE-SEIP 2024). ([Google Research](https://research.google/blog/resolving-code-review-comments-with-ml/))
    - "Automated Code Review In Practice": 3 industrial projects, 4,335 PRs (1,568 auto-reviewed); 73.8% of AI comments were resolved, but average PR close time rose from 5h52m to 8h20m; practitioners reported faulty or unnecessary comments alongside small quality gains. ([arXiv:2412.18531](https://arxiv.org/abs/2412.18531v2))
    - "Does AI Code Review Lead to Code Changes?": 22,000+ comments across 178 repositories and 16 tools; comments were more likely to lead to changes when concise, containing code snippets, manually triggered, and tied to a specific hunk. ([arXiv:2508.18771](https://arxiv.org/abs/2508.18771v1))
- **Gate design used in the capstone** (course choice): AI review runs as a required but non-blocking check; a named human CODEOWNER gives final approval.

### 4. Prompt injection and agentic threats

- **OWASP.** Prompt injection is LLM01 in the [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/). OWASP released a separate [Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026) on December 9, 2025, with risks ASI01–ASI10. (The ASI item names were not checked against the primary document for this course.)
- **The lethal trifecta.** Simon Willison ([June 16, 2025](https://simonwillison.net/2025/jun/16/the-lethal-trifecta/)): an agent that combines access to private data, exposure to untrusted content, and the ability to communicate externally can be made to leak data. He calls guardrails that stop "95%" of attacks a failing grade.
- **Incidents, in teaching order:**

| Date | Incident | What happened | Sources |
| --- | --- | --- | --- |
| Late May 2025 | GitHub MCP "toxic agent flow" | A malicious public issue hijacks an agent using the GitHub MCP server into leaking private-repo data through a PR on a public repo. An architectural issue, not a server bug; "always allow" makes it worse. | [Invariant Labs](https://invariantlabs.ai/blog/mcp-github-vulnerability); [DevClass, May 27, 2025](https://www.devclass.com/ai-ml/2025/05/27/researchers-warn-of-prompt-injection-vulnerability-in-github-mcp-with-no-obvious-fix/1623458) |
| July 13–19, 2025 | Amazon Q Developer VS Code extension | A planted prompt told the agent to wipe local files and AWS resources; it shipped in v1.84.0 (July 17) and was replaced by v1.85.0 (July 19). The payload reportedly did not work. Secondary press only. | [CyberInsider](https://cyberinsider.com/amazons-ai-coding-assistant-for-vsc-infected-with-data-wiper/); [SC World](https://scworld.com/news/amazon-q-extension-for-vs-code-reportedly-injected-with-wiper-prompt) |
| August 26, 2025 | Nx "s1ngularity" | Malicious Nx npm versions stole tokens and keys; Wiz describes it as the first known supply-chain attack to use installed AI CLIs (Claude, Gemini, Q) for reconnaissance. Over 1,000 valid GitHub tokens leaked. | [Wiz](https://www.wiz.io/blog/s1ngularity-supply-chain-attack) |
| September 2025 | Shai-Hulud npm worm | A self-replicating worm compromised 500+ npm packages and harvested credentials. CISA (September 23, 2025) advised pinning to releases from before September 16, 2025 and rotating credentials. A classic supply-chain worm, not an AI-specific attack. | [CISA](https://www.cisa.gov/news-events/alerts/2025/09/23/widespread-supply-chain-compromise-impacting-npm-ecosystem) |

### 5. Package hallucination and slopsquatting

- Spracklen et al., "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs" (USENIX Security 2025): 16 LLMs, 576,000 Python and JavaScript samples; average hallucination rates of at least 5.2% for commercial and 21.7% for open-source models; 205,474 unique hallucinated package names. ([arXiv:2406.10279](https://arxiv.org/abs/2406.10279))
- Seth Larson (PSF Developer-in-Residence) coined "slopsquatting" ("AI slop" + "typosquatting") in April 2025: attackers register package names that models hallucinate. ([IEEE Spectrum](https://spectrum.ieee.org/ai-cyberattacks-llm-slop-squatting); [Wikipedia](https://en.wikipedia.org/wiki/Slopsquatting))
- Controls: a lockfile or allow-list, and a CI check that rejects new dependencies that do not exist or were registered recently.

### 6. Governance, copyright, and authorship

- **NIST AI 600-1** (Generative AI Profile, July 2024): a voluntary profile of the AI RMF covering 12 GenAI risks, including "confabulation." ([NIST draft PDF](https://airc.nist.gov/docs/NIST.AI.600-1.GenAI-Profile.ipd.pdf); [DLA Piper summary](https://www.dlapiper.com/en/insights/publications/ai-outlook/2024/nist-releases-its-generative-artificial-intelligence-profile))
- **NIST SP 800-218A** (July 26, 2024): "Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile," augmenting SSDF v1.1. ([NIST CSRC](https://csrc.nist.gov/pubs/sp/800/218/a/final))
- **US Copyright Office, Copyright and AI Part 2: Copyrightability** (January 29, 2025): purely AI-generated material, including output from prompts alone, is not copyrightable; human selection, arrangement, or modification can be protected. ([copyright.gov](https://copyright.gov/newsnet/2025/1060.html))
- **Linux kernel policy.** A coding-assistant policy committed December 23, 2025 requires the tag `Assisted-by: AGENT_NAME:MODEL_VERSION [tools]`; AI must not add `Signed-off-by`, so only a human certifies the DCO and takes responsibility. ([FOSSID](https://fossid.com/articles/linux-kernel-ai-coding-policy-enterprise-accountability/)) Developers discussed revising the tag in 2026; status unresolved. ([Phoronix](https://phoronix.com/news/Linux-AI-Attribution-Again))
- **Other projects** range from bans (Gentoo, NetBSD, QEMU) to disclosure; those details come from secondary summaries and are to verify. ([SSD Nodes summary](https://www.ssdnodes.com/learn/ai-assisted-code-open-source-policies))
- **EU AI Act.** Under the 2026 Omnibus, Annex III high-risk obligations apply from December 2, 2027. ([Future of Privacy Forum](https://fpf.org/?p=250371)) The entry-into-force date differs between sources (July 24 vs. July 27, 2026); check the Official Journal.
- **Common rule across policies:** the human who signs off owns the change.

### 7. Architecture Decision Records (ADRs)

- Michael Nygard proposed ADRs in "Documenting Architecture Decisions" (November 15, 2011) with the sections Title, Context, Decision, Status (proposed, accepted, deprecated, superseded), and Consequences. ([Cognitect](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions); [arc42 FAQ](https://faq.arc42.org/questions/C-9-3/))
- **MADR** (from 2017) is a widely used Markdown template drawing on Nygard and Zimmermann's Y-statements. ([MADR primer](https://ozimmer.ch/practices/2022/11/22/MADRTemplatePrimer.html)) Community hub: [adr.github.io](https://adr.github.io/).
- ADRs answer Storey's concern directly: they record why a decision was made, so the team keeps its theory of the system even when an agent wrote the code.

## Required readings

| Title | Author / org | Date | Link |
| --- | --- | --- | --- |
| How Generative and Agentic AI Shift Concern from Technical Debt to Cognitive Debt | Margaret-Anne Storey | February 9, 2026 | <https://margaretstorey.com/blog/2026/02/09/cognitive-debt/> |
| Your Brain on ChatGPT (abstract and design) | Kosmyna et al. (MIT Media Lab) | June 10, 2025 (v2 December 31, 2025) | <https://arxiv.org/abs/2506.08872> |
| How AI assistance impacts the formation of coding skills | Judy Hanwen Shen, Alex Tamkin (Anthropic) | January 29, 2026 | <https://www.anthropic.com/research/AI-assistance-coding-skills> |
| Automated Code Review In Practice | arXiv 2412.18531 | v2 (date not recorded) | <https://arxiv.org/abs/2412.18531v2> |
| The lethal trifecta for AI agents | Simon Willison | June 16, 2025 | <https://simonwillison.net/2025/jun/16/the-lethal-trifecta/> |
| We Have a Package for You! | Spracklen et al. (USENIX Security 2025) | 2025 | <https://arxiv.org/abs/2406.10279> |
| OWASP Top 10 for Agentic Applications 2026 | OWASP GenAI Security Project | December 9, 2025 | <https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026> |
| Copyright and AI Part 2: Copyrightability | US Copyright Office | January 29, 2025 | <https://copyright.gov/newsnet/2025/1060.html> |
| Documenting Architecture Decisions | Michael Nygard | November 15, 2011 | <https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions> |

## Hands-on lab: threat-model your own agent setup

**Goal:** apply the lethal trifecta and slopsquatting controls to the agent configuration you built in Modules 1–3, and write your first ADRs.

!!! tip "No-cost path"
    Every step works with Gemini CLI or with no agent at all; the work is analysis and writing.

1. **List your agent's capabilities.** On the Claude track, run:

    ```bash
    claude mcp list
    ```

    On the Gemini track, list the `mcpServers` you configured in `~/.gemini/settings.json`. Add the built-in tools (file, shell, web) to the list.

2. **Trifecta table.** For each tool or server, mark whether it gives (a) access to private data, (b) exposure to untrusted content, (c) a way to communicate externally. Circle any session where all three are present at once.

3. **Break one leg.** For each trifecta you found, write the change that removes one leg (for example: scope an MCP server to `--scope local` only for a specific project, remove web fetch from a subagent, or require approval for pushes). Note which change is a hard control (permission, hook, sandbox) and which is only an instruction in a context file.

4. **Dependency check.** List every dependency the agent added during Modules 2–3. For each, record the registry page and first publish date. Flag any you cannot find or that were published very recently.

5. **ADRs.** Write two ADRs in the Nygard format (Title, Context, Decision, Status, Consequences) for design choices the agent proposed in your project. In Context, state that the option came from an agent and what alternatives you considered.

## Capstone project

### Brief

Teams of 2–4 deliver a small but complete software module (instructor-approved scope, for example a feature added to an existing open-source project or a new service) using the full workflow from this course. Every team member must be able to explain every part of it in the oral defense.

### Required artifacts

| Artifact | Requirement |
| --- | --- |
| Context | `AGENTS.md` (under 200 lines) and any per-tool files; MCP configuration if used |
| Specification | Spec Kit artifacts (constitution, `spec.md`, `plan.md`, `tasks.md`, reviewed checklists) or OpenSpec specs and archived changes; at least 10 requirements in EARS form |
| Convergence evidence | `/speckit-analyze` reports and converge pass log, or `openspec validate --strict` output |
| Verification | End-to-end tests traced to acceptance criteria; static gates; secret scanning; eval suite in CI that runs on rule/prompt changes |
| Review | Every merged PR has an AI review (non-blocking) and a named human approver |
| Security | Trifecta analysis and dependency check from the Module 4 lab, updated for the final system |
| Decisions | At least four ADRs, each owned by a named team member |
| AI-use log | Which agent and model did what, per PR (the Linux kernel's `Assisted-by:` format is a good model) |
| Reflection | One page per student: one place you lost understanding of the system and how you recovered it |

### Rubric (course-authored)

The capstone is worth 40% of the course grade (see [Assessment](../assessment.md)). Within the capstone:

| Dimension | Weight | Excellent (full marks) | Weak (little or no credit) |
| --- | --- | --- | --- |
| Specification quality | 20% | Requirements are testable, use EARS correctly, cover unwanted behaviour; spec matches what was built | Vague requirements; spec abandoned after the first implement pass |
| Verification | 25% | Every acceptance criterion traced to a passing automated test; gates block real defects; evals run on rule changes | Tests exist but do not map to the spec; gates missing or always bypassed |
| Security and governance | 15% | Trifecta analysis with hard controls; dependency check in CI; AI-use log complete | No threat analysis; unexplained dependencies; no AI-use log |
| Decisions and understanding | 20% | ADRs explain why, with alternatives; reflections are specific | ADRs restate what the code does; no alternatives |
| Process and review | 20% | Clean history; human approval on every PR; review comments acted on or rebutted | Large unexplained agent commits; rubber-stamp approvals |

The individual oral defense (graded separately) checks that each student understands the delivered system.

## Discussion questions

1. Kosmyna et al. and Storey both use "cognitive debt." What does each mean, and which matters more for a software team?
2. In the Anthropic RCT, the AI group was not significantly faster but scored 17 points lower. Under what conditions would you still choose to let a junior engineer use AI on an unfamiliar library?
3. One industrial study found AI review comments were mostly resolved but PRs took longer to close. Is that a good trade? What would you measure?
4. Pick one incident from the table. Which leg of the lethal trifecta would you remove, and what would it cost the developer?
5. The Linux kernel forbids AI from adding `Signed-off-by`. Should this course require an `Assisted-by:` line on every commit? Why or why not?
