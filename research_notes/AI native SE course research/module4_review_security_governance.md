# Module 4: cognitive debt, AI code review, AI security and governance, ADRs

Research date: October 8, 2026. All items below were found via web search/fetch in this session. Items marked "(search snippet)" were seen only in search-result summaries, not on a fetched primary page. Verify those before quoting them on slides.

## Q1. Cognitive debt: origin, definitions, evidence quality

### Takeaway
The term "cognitive debt" in its current AI sense comes from the MIT Media Lab preprint by Kosmyna et al. (June 2025). Margaret-Anne Storey (Feb 2026) carried it into software engineering as a team-level loss of shared understanding. The MIT study is a small EEG study of essay writing, not coding, and it has not been peer reviewed. Present it as a provocative signal, not settled evidence.

### Cited findings
- Title: "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task." Authors: Nataliya Kosmyna, Eugene Hauptmann, Ye Tong Yuan, Jessica Situ, Xian-Hao Liao, Ashly Vivian Beresnitzky, Iris Braunstein, Pattie Maes. arXiv:2506.08872 — [arXiv](https://arxiv.org/abs/2506.08872); [MIT Media Lab](https://www.media.mit.edu/publications/your-brain-on-chatgpt/)
- Submitted June 10, 2025. Latest version v2 is dated December 31, 2025. The paper runs 216 pages. It is an arXiv preprint, and the abstract page shows no journal acceptance — [arXiv](https://arxiv.org/abs/2506.08872)
- Design: 54 participants did SAT-style essays in three groups (LLM, Search Engine, Brain-only) over 3 sessions. Only 18 completed session 4, where the LLM and Brain-only groups swapped conditions. Methods were EEG, NLP analysis, and scoring by human teachers and an AI judge — [arXiv](https://arxiv.org/abs/2506.08872)
- Participants were undergraduates from MIT, Harvard, Tufts and similar schools, and the study ran for four months (search snippet) — [arXiv v1](https://arxiv.org/abs/2506.08872v1)
- Finding: "LLM users consistently underperformed at neural, linguistic, and behavioral levels." The LLM group also showed lower brain connectivity and lower self-reported ownership of their essays — [arXiv](https://arxiv.org/abs/2506.08872)
- The authors define cognitive debt by analogy to technical debt: short-term efficiency gains are paid for with long-term costs (search snippet) — [MIT Media Lab](https://www.media.mit.edu/publications/your-brain-on-chatgpt/)
- Storey, "How Generative and Agentic AI Shift Concern from Technical Debt to Cognitive Debt," February 9, 2026. She defines cognitive debt as the erosion of a team's shared theory of what the software does and how to change it. She builds on Peter Naur's "programming as theory building" and credits MIT Media Lab research for the term's recent traction — [margaretstorey.com](https://margaretstorey.com/blog/2026/02/09/cognitive-debt/)
- Storey's anecdote: a student startup team stalled in weeks 7–8 because no one could explain the design decisions or how the parts fit together. They blamed messy code, but the real problem was cognitive debt — [margaretstorey.com](https://margaretstorey.com/blog/2026/02/09/cognitive-debt/)
- Storey's mitigations:
  - At least one human must fully understand each AI-generated change before it ships.
  - Document why a change was made, not only what changed.
  - Hold regular checkpoints (reviews, retrospectives, knowledge sharing).
  - Watch for warning signs: people hesitate to modify code, knowledge stays tribal, the system becomes a black box.
  
  Source: [margaretstorey.com](https://margaretstorey.com/blog/2026/02/09/cognitive-debt/)
- Follow-up post "Cognitive debt revisited" (February 18, 2026) reports that practitioners confirmed the problem — [margaretstorey.com](https://margaretstorey.com/blog/2026/02/18/cognitive-debt-revisited/)
- Simon Willison linked and discussed the concept on February 15, 2026 — [simonwillison.net](https://simonwillison.net/2026/Feb/15/cognitive-debt)
- A related paper, "From Technical Debt to Cognitive and Intent Debt: Rethinking Software Health in the Age of AI," arXiv 2603.22106, defines "intent debt" as the erosion of explicit rationale, goals and constraints (search snippet; authorship not verified, possibly Storey) — [alphaXiv](https://www.alphaxiv.org/abs/2603.22106)
- Practitioner amplification: DX newsletter, "Cognitive debt: The hidden risk in AI-driven software development" — [DX](https://getdx.com/blog/cognitive-debt-the-hidden-risk-in-ai-driven-software-development/)

### Inferences
- For the course, keep two meanings apart. Kosmyna's is individual and neurocognitive, from essay writing. Storey's is team-level and about the software system. ADRs (Q6) are a direct answer to Storey's "intent debt."
- The MIT result should not be generalized to developers. The task was essay writing, only 18 people completed the crossover session, the participants came from elite universities, and the paper is a preprint.

### Gaps
- I did not fetch the limitations section of the full 216-page PDF. The authors' own stated limitations are not quoted here.
- I could not confirm that the term was used in an SE context before 2025.

## Q2. Comprehension and skill effects of AI assistance

### Takeaway
Two studies give the best evidence:
- **Anthropic RCT (January 2026):** AI help reduced immediate understanding of a new library (50% vs 67% quiz score) with no significant speed gain. How people used AI mattered.
- **Microsoft/CMU survey (CHI 2025):** people who trust GenAI more report thinking less critically.

### Cited findings
- **Anthropic, "How AI assistance impacts the formation of coding skills"**
  - Published January 29, 2026, by Judy Hanwen Shen and Alex Tamkin. Paper: arXiv:2601.20245 — [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills); [arXiv](https://arxiv.org/abs/2601.20245)
  - Design: randomized controlled trial with 52 mostly junior engineers learning Trio, a Python async library they had not used before — [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills)
  - Quiz scores: AI group 50%, control group 67% (Cohen's d = 0.738, p = 0.01). The biggest gap was on debugging questions — [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills)
  - Speed: the AI group finished about 2 minutes faster, which was not statistically significant — [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills)
  - Usage patterns:
    - Low scores went with delegating to AI, relying on it more over time, and debugging by iterating with it.
    - High scores went with generating code and then working to understand it, asking for code plus explanation, and asking conceptual questions.
    
    Source: [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills)
  - Limitations stated by the authors: small sample, a quiz taken right after the task, and no evidence about long-term skill — [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills)
- **Lee et al., "The Impact of Generative AI on Critical Thinking: Self-Reported Reductions in Cognitive Effort and Confidence Effects From a Survey of Knowledge Workers" (CHI 2025, Microsoft Research + CMU)**
  - Survey of 319 knowledge workers, who gave 936 examples of GenAI use — [Microsoft Research](https://www.microsoft.com/en-us/research/?p=1135061); [PDF](https://advait.org/files/lee_2025_ai_critical_thinking_survey.pdf)
  - Higher confidence in GenAI went with less critical thinking. Higher self-confidence went with more — [PDF](https://advait.org/files/lee_2025_ai_critical_thinking_survey.pdf)
  - GenAI shifts critical thinking toward verifying information, integrating responses, and "task stewardship" — [PDF](https://advait.org/files/lee_2025_ai_critical_thinking_survey.pdf)

### Inferences
- The teaching point: AI use is not inherently harmful. The interaction pattern decides the outcome. Students can practice "generate, then explain" and conceptual questioning as habits.
- Lee et al. measure what people report about themselves, not their actual skill. Anthropic measures skill, but only right after the task. Neither study measures long-term skill loss.

### Gaps
- No longitudinal study of professional developers' skill erosion was found.

## Q3. AI code review: tools, practice, evidence

### Takeaway
Major platforms now ship AI review (GitHub Copilot code review GA on April 4, 2025; the Claude Code GitHub Action). Copilot only comments; it cannot approve or block. Field evidence is mixed:
- **Google:** ML-suggested edits resolve about 7.5% of reviewer comments.
- **Industrial study:** 73.8% of AI comments were resolved, but PR close time grew from about 5h52m to 8h20m.

Human sign-off stays the gate.

### Cited findings
- **GitHub Copilot code review**
  - Generally available April 4, 2025 — [GitHub changelog](https://github.blog/changelog/2025-04-04-copilot-code-review-now-generally-available/)
  - Support for customizing it with copilot-instructions.md became GA on August 6, 2025 — [GitHub changelog](https://github.blog/changelog/2025-08-06-copilot-code-review-copilot-instruction-md-support-is-now-generally-available/)
  - Copilot leaves "Comment" reviews only, never "Approve" or "Request changes," so it does not block merges (search snippet from a third-party guide; confirm against GitHub docs) — [morphllm guide](https://www.morphllm.com/copilot-code-review)
- **Claude Code GitHub Actions**
  - Official docs — [code.claude.com](https://code.claude.com/docs/en/github-actions)
  - Setup: run `claude install-github-app`, or add the workflow by hand with an ANTHROPIC_API_KEY secret. It can review PRs and implement fixes (search snippet) — [code.claude.com](https://code.claude.com/docs/en/github-actions)
- **Google, "Resolving code review comments with ML"** (May 23, 2023; Frömmgen and Kharatyan)
  - Authors address 7.5% of reviewer comments by applying an ML-suggested edit — [Google Research](https://research.google/blog/resolving-code-review-comments-with-ml/)
  - The paper version appeared at ICSE-SEIP 2024 — [ICSE 2024](https://conf.researchr.org/details/icse-2024/icse-2024-software-engineering-in-practice/35/Resolving-Code-Review-Comments-with-Machine-Learning)
- **"Automated Code Review In Practice"** (arXiv 2412.18531)
  - 3 industrial projects, 4,335 PRs, 1,568 of them auto-reviewed. 73.8% of AI comments were resolved.
  - Average PR close time rose from 5h52m to 8h20m.
  - Practitioners reported small quality gains, plus faulty or unnecessary comments.
  
  Source: [arXiv](https://arxiv.org/abs/2412.18531v2)
- **"Does AI Code Review Lead to Code Changes? A Case Study of GitHub Actions"** (arXiv 2508.18771)
  - More than 22,000 comments across 178 repositories and 16 AI review tools. Effectiveness varied widely.
  - Comments were more likely to lead to changes when they were concise, included code snippets, were triggered manually, and were tied to a specific code hunk.
  
  Source: [arXiv](https://arxiv.org/abs/2508.18771v1)

### Inferences
- A good gate design for the capstone: AI review runs as a required but non-blocking check. A human CODEOWNER gives the final approval. This matches how Copilot is designed and the evidence that AI reviews also produce false positives and slow PRs down.

### Gaps
- CodeRabbit and Graphite (Diamond / Graphite Agent): I did not fetch vendor docs or independent effectiveness data. Do not cite numbers for them without further research.
- Vendor claims about bug-catch rates were not verified.

## Q4. Prompt injection, agentic threats and real incidents

### Takeaway
Prompt injection is #1 in OWASP's LLM Top 10 (2025). OWASP published a separate Agentic Top 10 on December 9, 2025. Simon Willison's "lethal trifecta" (June 16, 2025) is the clearest way to teach it. 2025 brought several real compromises involving coding agents and AI CLIs.

### Cited findings
- **OWASP Top 10 for LLM Applications 2025** (v2.0, published November 18, 2024). LLM01:2025 is Prompt Injection, kept at #1. New entries are System Prompt Leakage and Vector and Embedding Weaknesses (search snippet, secondary sources) — [OWASP GenAI](https://genai.owasp.org/); [Coralogix summary](https://coralogix.com/ai-blog/owasp-top-10-for-llm-applications/)
- **OWASP Top 10 for Agentic Applications 2026**
  - Released December 9, 2025, with risks ASI01–ASI10, built with more than 100 experts — [OWASP GenAI](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026); [Palo Alto Networks](https://www.paloaltonetworks.com/blog/?p=349925)
  - It extends the LLM Top 10 rather than replacing it (search snippet) — [OWASP GenAI](https://genai.owasp.org/owasp-top-10-for-agentic-applications)
- **Willison, "The lethal trifecta for AI agents"** (June 16, 2025)
  - The three ingredients: access to private data, exposure to untrusted content, and the ability to communicate externally — [simonwillison.net](https://simonwillison.net/2025/jun/16/the-lethal-trifecta/)
  - He calls guardrails that stop "95%" of attacks a failing grade — [simonwillison.net](https://simonwillison.net/2025/jun/16/the-lethal-trifecta/)
- **GitHub MCP exploit (Invariant Labs, late May 2025; DevClass coverage May 27, 2025)**
  - A malicious public GitHub issue hijacks an agent, such as Claude Desktop with the GitHub MCP server, into leaking private-repo data through a PR on the public repo. Invariant calls this a "toxic agent flow."
  - It is an architectural issue, not a bug in the server code. Users who choose "always allow" make it worse.
  
  Sources: [Invariant Labs](https://invariantlabs.ai/blog/mcp-github-vulnerability); [DevClass](https://www.devclass.com/ai-ml/2025/05/27/researchers-warn-of-prompt-injection-vulnerability-in-github-mcp-with-no-obvious-fix/1623458)
- **Amazon Q Developer VS Code extension (July 2025)**
  - On July 13, 2025, a commit from user "lkmanka58" planted a prompt telling the agent to wipe local files and AWS resources.
  - It shipped in v1.84.0 on July 17 and was replaced by v1.85.0 on July 19. The payload reportedly did not work because of formatting errors.
  
  Sources (secondary press; AWS's own bulletin not fetched): [CyberInsider](https://cyberinsider.com/amazons-ai-coding-assistant-for-vsc-infected-with-data-wiper/); [SC World](https://scworld.com/news/amazon-q-extension-for-vs-code-reportedly-injected-with-wiper-prompt)
- **Nx "s1ngularity" (August 26, 2025)**
  - Malicious Nx npm versions stole tokens, SSH keys and wallets. Wiz describes it as the first known supply-chain attack to use installed AI CLIs (Claude, Gemini, Q) for reconnaissance.
  - Over 1,000 valid GitHub tokens were leaked. GitHub disabled the attacker's repos on August 27 at 09:00 UTC.
  - In a second wave, more than 400 users/orgs and more than 5,500 repos were made public.
  
  Source: [Wiz](https://www.wiz.io/blog/s1ngularity-supply-chain-attack)
- **Shai-Hulud npm worm (September 2025)**
  - A self-replicating worm compromised more than 500 npm packages, harvesting GitHub PATs and cloud keys and republishing packages as the compromised maintainers.
  - CISA alert dated September 23, 2025. CISA advises pinning to releases from before September 16, 2025, and rotating credentials.
  
  Sources: [CISA](https://www.cisa.gov/news-events/alerts/2025/09/23/widespread-supply-chain-compromise-impacting-npm-ecosystem); [The Record](https://therecord.media/cisa-urges-software-reviews-malicious-packages)
  - A second wave ("Shai-Hulud 2," late November 2025) reportedly hit more than 800 packages (search snippet; figures not verified at a primary source) — [Assured](https://assured.co.uk/2025/ai-autopsy-how-shai-hulud-2-is-rewriting-the-rules-of-supply-chain-security/)

### Inferences
- Shai-Hulud is a classic supply-chain worm, not an AI-specific attack. s1ngularity is the case where AI CLIs themselves were weaponized. Teach them as distinct.
- Teaching sequence: lethal trifecta, then the MCP exploit (trifecta in action), then Amazon Q (poisoned agent instructions in the supply chain), then s1ngularity (attackers using local AI agents as tools).

### Gaps
- AWS's official security bulletin for Amazon Q (often cited as AWS-2025-015) was not fetched. Treat that ID as unverified.
- The OWASP Agentic ASI01–ASI10 item names were not fetched from the primary PDF.

## Q5. Slopsquatting and package hallucination

### Takeaway
Spracklen et al. (USENIX Security 2025) found:
- 16 LLMs and 576,000 code samples.
- Average hallucination rates of at least 5.2% for commercial models and 21.7% for open-source models.
- 205,474 unique fake package names.

Seth Larson (PSF) coined "slopsquatting" in April 2025.

### Cited findings
- Title: "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs." Authors: Spracklen, Wijewickrama, Sakib, Maiti, Viswanath, Jadliwala. Venue: USENIX Security 2025 — [arXiv](https://arxiv.org/abs/2406.10279)
- Figures: 16 LLMs, 576,000 samples in Python and JavaScript, commercial models at least 5.2% and open-source at least 21.7% average hallucination rate, and 205,474 unique hallucinated package names — [arXiv](https://arxiv.org/abs/2406.10279)
- A frequently quoted repetition figure: 58% of hallucinated packages recur when the same prompt is rerun (search snippet; I did not confirm the exact wording in the paper) — [arXiv HTML](https://arxiv.org/html/2406.10279v3)
- The commonly cited "19.7% overall" figure did NOT appear in what I retrieved. Do not use it without checking the paper.
- Seth Larson, PSF Developer-in-Residence, coined "slopsquatting" in April 2025. The word combines "AI slop" and "typosquatting" — [Wikipedia](https://en.wikipedia.org/wiki/Slopsquatting); [IEEE Spectrum](https://spectrum.ieee.org/ai-cyberattacks-llm-slop-squatting)
- A follow-up preprint, "The Range Shrinks, the Threat Remains," re-evaluates hallucination rates for 2026 frontier models (arXiv 2605.17062; not read) — [arXiv](https://arxiv.org/pdf/2605.17062)

### Inferences
- Capstone controls: an allow-list or lockfile, and a CI check that rejects new dependencies that don't exist or were registered recently.

### Gaps
- Larson's original post or toot was not located. The attribution rests on Wikipedia and IEEE Spectrum.

## Q6. Governance frameworks and authorship policies

### Takeaway
- **US frameworks:** NIST provides voluntary ones: the AI RMF and its GenAI Profile (AI 600-1, July 2024), and SSDF 800-218A (July 26, 2024).
- **EU AI Act:** the Digital Omnibus (in force July 2026) pushed Annex III high-risk obligations to December 2, 2027.
- **Copyright:** the US Copyright Office says purely AI-generated material is not copyrightable.
- **Open source:** policies range from bans (Gentoo, NetBSD, QEMU) to disclosure (the Linux kernel's "Assisted-by" tag). In every case a human remains accountable.

### Cited findings
- **NIST AI 600-1 (Generative AI Profile), July 2024**
  - A profile of AI RMF 1.0 with 12 GenAI risks, including "confabulation." It is voluntary — [DLA Piper](https://www.dlapiper.com/en/insights/publications/ai-outlook/2024/nist-releases-its-generative-artificial-intelligence-profile); [NIST draft PDF](https://airc.nist.gov/docs/NIST.AI.600-1.GenAI-Profile.ipd.pdf)
  - The final version is at doi.org/10.6028/NIST.AI.600-1 (link not fetched).
- **NIST SP 800-218A, "Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile"** (July 26, 2024). It augments SSDF v1.1 and was written in response to EO 14110 — [NIST CSRC](https://csrc.nist.gov/pubs/sp/800/218/a/final); [PDF](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218A.pdf)
  - Note: EO 14110 was rescinded in January 2025. This is my own knowledge and was not verified in this session; check it before stating.
- **EU AI Act under the AI Omnibus (FPF, July 28, 2026, updated August 5, 2026)**
  - Process: Parliament adopted it June 16, 2026, and the Council June 29, 2026. Entry into force: July 24, 2026 per FPF, but July 27, 2026 per a search snippet. The sources conflict; check the Official Journal.
  - Dates that apply:
    - August 2, 2026: general applicability.
    - December 2, 2026: Art. 50(2) transparency for some existing systems.
    - August 2, 2027: GPAI models placed on the market before August 2, 2025.
    - December 2, 2027: Annex III high-risk systems.
    - August 2, 2028: Annex I high-risk systems.
  
  Source: [Future of Privacy Forum](https://fpf.org/?p=250371)
- **US Copyright Office, Copyright and AI Part 2: Copyrightability (January 29, 2025)**
  - Purely AI-generated material, including output from prompts alone, is not copyrightable. Assistive use and human selection, arrangement or modification can be protected — [copyright.gov](https://copyright.gov/newsnet/2025/1060.html); [LOC blog](https://blogs.loc.gov/copyright/2025/02/inside-the-copyright-offices-report-copyright-and-artificial-intelligence-part-2-copyrightability/)
- **Linux kernel**
  - An AI coding-assistant policy was committed December 23, 2025, by Sasha Levin and approved by Jonathan Corbet, following the 2025 Maintainers Summit — [FOSSID](https://fossid.com/articles/linux-kernel-ai-coding-policy-enterprise-accountability/)
  - Rules: use the tag format `Assisted-by: AGENT_NAME:MODEL_VERSION [tools]`. AI must not add Signed-off-by; only a human certifies the DCO and takes responsibility — [FOSSID](https://fossid.com/articles/linux-kernel-ai-coding-policy-enterprise-accountability/)
  - In 2026 developers discussed revising or dropping the attribution tag (status unresolved) — [Phoronix](https://phoronix.com/news/Linux-AI-Attribution-Again)
- **Gentoo:** the council banned contributions made with NLP AI tools on April 14, 2024 (search snippet) — [SSD Nodes summary](https://www.ssdnodes.com/learn/ai-assisted-code-open-source-policies)
- **NetBSD:** May 2024 commit guidelines treat LLM output as "tainted code" that needs prior written approval from core (search snippet) — [SSD Nodes summary](https://www.ssdnodes.com/learn/ai-assisted-code-open-source-policies)
- **QEMU:** the code-provenance policy declines AI-generated contributions. A proposal by Paolo Bonzini would allow limited AI-assisted patches with disclosure (status as of August 2026 per search snippet) — [feedbagel](https://feedbagel.com/post/qemu-may-relax-its-ban-on-ai-generated-contributions)

### Inferences
- A common rule across policies: the human who signs off owns the change. This fits a capstone rule of "AI may draft, a named human is accountable."

### Gaps
- **ISO/IEC 42001:2023** (AI management system standard, published December 2023): not fetched this session. The likely URL is https://www.iso.org/standard/81230.html, which is unverified.
- The primary sources for the Gentoo, NetBSD and QEMU policies were not fetched (only secondary summaries). The Linux kernel doc URL (likely docs.kernel.org/process/coding-assistants.html) was not fetched.

## Q7. Architecture Decision Records for the capstone

### Takeaway
Michael Nygard proposed ADRs on November 15, 2011, with the sections Title, Context, Decision, Status and Consequences. MADR (from 2017) is the most widely used Markdown template, and adr.github.io is the community hub.

### Cited findings
- Nygard, "Documenting Architecture Decisions," Cognitect blog, November 15, 2011 — [Cognitect](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions); [arc42 FAQ](https://faq.arc42.org/questions/C-9-3/)
- Template sections: Title, Context, Decision, Status (proposed / accepted / deprecated / superseded), Consequences — [arc42 FAQ](https://faq.arc42.org/questions/C-9-3/)
- MADR started in 2017 and draws on Nygard and on Olaf Zimmermann's Y-statements — [ozimmer.ch MADR primer](https://ozimmer.ch/practices/2022/11/22/MADRTemplatePrimer.html)
- Community hub — [adr.github.io](https://adr.github.io/); Martin Fowler's overview — [martinfowler.com](https://www.martinfowler.com/bliki/ArchitectureDecisionRecord.html)

### Inferences
- In the capstone, require an ADR for every significant AI-proposed design choice, written and owned by a human. This targets Storey's cognitive and intent debt directly.

### Gaps
- The current MADR version number (e.g., 4.0.0) and the adr.github.io template list were not fetched.
