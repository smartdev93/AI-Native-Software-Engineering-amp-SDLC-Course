# Assessment and grading

## What the evidence says (and does not say)

Geng et al., "Mapping the Emerging Curriculum for AI-Assisted Software Engineering via Syllabus Analysis" ([arXiv:2608.05898](https://arxiv.org/abs/2608.05898), submitted August 6, 2026; a preprint), analyzed 23 publicly available syllabi of US upper-division AI-assisted software engineering courses. Of those, **14** used conventional percentage grading. Across those 14 courses the paper reports these **descriptive** medians ([arXiv HTML](https://arxiv.org/html/2608.05898)):

| Category | Courses using it | Median | Range |
| --- | --- | --- | --- |
| Participation / engagement | 14 | 17.5% | 5–46% |
| Project / capstone | 12 | 50% | 30–85% |
| Coding assignments | 9 | 20% | 10–60% |
| Review / reflection | 6 | 19% | 10–40% |
| Paper presentation | 4 | 20% | 10–20% |
| Quiz / exam | 4 | 15% | 10–30% |

Proctored assessment appeared in only 3 of the 14 courses (median 20%). The paper does **not** recommend weights, and it states that course-material analysis "cannot show how instructors enacted these courses, how students used AI tools in practice, or how effective the courses were for learning." Its one design suggestion is that "process logs, code reviews, demonstrations or oral defenses, individual reflections, and in-class checkpoints may help preserve project-based work while strengthening assessment of individual understanding." No study found for this course compares learning outcomes across different weightings.

## This course's weights

!!! note "Course choice"
    These weights are this course's own design. They are benchmarked against the Geng et al. medians above, not derived from them.

| Component | Weight | What is assessed | Benchmark |
| --- | --- | --- | --- |
| Module labs (Modules 1–3) | 25% | Lab deliverables, graded with the [lab rubric](#lab-rubric): context files, specs, convergence logs, tests, eval suites | Coding assignments median 20% |
| Capstone project (team) | 40% | Graded with the [capstone rubric](modules/module-4.md#rubric-course-authored) | Project/capstone median 50%, range 30–85% |
| Individual oral defense | 20% | Individual understanding of the capstone (format below) | CMU 15-113 gives oral evaluations + quizzes 20%; Geng proctored median 20% |
| Reflections and AI-use logs | 10% | Per-lab reflections, capstone reflection, completeness and honesty of AI-use logs | Review/reflection median 19% |
| Participation and discussion | 5% | Discussion questions, peer review of specs and ADRs | Participation range 5–46% |
| **Total** | **100%** | | |

Why the capstone is below the 50% median: 20% is moved into an individually verified oral defense, so a strong team grade cannot hide a student who does not understand the system. Together the oral defense and reflections make 30% of the grade individual.

## Lab rubric

!!! note "Course-authored"
    This rubric is the course's own design. It grades the artifacts each lab produces, not how much code the agent wrote.

Each Module 1–3 lab is scored out of 100 on four dimensions of 25 points each. Not every dimension applies to every lab; where one does not, score the others and rescale to 100.

| Dimension | Points | Full marks | Little or no credit | Applies to |
| --- | --- | --- | --- | --- |
| Specification quality | 25 | Requirements are in correct EARS form, testable, and cover unwanted behavior (error states, boundaries); acceptance criteria match what was built | Vague requirements the agent has to guess at; spec abandoned after the first implement pass | Module 2, Module 3 |
| Context and boundaries | 25 | `AGENTS.md` is short and specific; rules that must hold are enforced by permissions, hooks, or gates, not only by instructions; MCP servers are scoped to what the task needs | Missing or bloated context file; the agent breaks project conventions or adds unapproved dependencies | All modules |
| Verification | 25 | Each acceptance criterion maps to a passing automated test; static and secret-scanning gates run; the student shows a gate catching a real break | Tests do not trace to the spec; gates missing, always bypassed, or never shown to fail | Module 3 (Module 2 where tests exist) |
| Process evidence | 25 | Git history shows spec-driven work (spec edits followed by implement/converge commits); the AI-use log and reflection are complete and specific | Large unexplained agent commits; no AI-use log; reflection is generic | All modules |

There is no coverage percentage or "zero findings" threshold. A target such as "85% coverage" rewards tests that execute code without checking the spec. Grade whether the tests trace to acceptance criteria and whether the gates catch real defects.

## Oral defense

**Format (course choice):** 15 minutes per student, held after capstone submission, with one instructor or TA.

1. **Walkthrough (5 min).** The student explains one requirement end to end: the EARS sentence, the plan decision, the code, the test that verifies it.
2. **Probe (5 min).** The examiner picks a file or ADR the student did not choose and asks why it is built that way and what alternatives were considered.
3. **Change (5 min).** The examiner proposes a small change. The student says which spec, tests, and ADRs would change, and may use an agent under observation to make it.

**Graded on:** process, transparency, and ownership (following CMU 15-113), on a 0–4 scale for each. A student who cannot explain code they submitted loses the ownership points regardless of whether the code works.

**Basis in practice and research:**

- **CMU 15-113/114 "Effective Coding with AI"** holds a TA interview after each project "in the style of a mock job interview," graded on process, transparency, and ownership; oral evaluations and quizzes are 20% of the grade, and submitting AI code you cannot explain is not allowed. ([course page](https://www.cs.cmu.edu/~113/))
- **Code Interviews** (Kannam, Yang, Dharm, Lin; SIGCSE TS 2025) replaced exam-style checks with discussions of students' own take-home work. Students discussed their work in more nuanced ways; peer formats reduced some stress but added other kinds; scaling was hard. ([arXiv:2410.01010](https://arxiv.org/abs/2410.01010))
- **The Conversational Exam** (Barba and Stegner, *Computer* 59(6):132–139, June 2026) used live-coding oral exams with supervised AI access, run for 58 students in small groups over two days. ([arXiv:2601.10691](https://arxiv.org/abs/2601.10691))

These are design and experience reports with small samples, not controlled outcome studies. Plan TA time accordingly: the Code Interviews authors found scaling was the main difficulty.

## AI-use policy

!!! note "Course choice"
    This policy is the course's own. It follows the pattern of the AI-native courses listed in [Real courses](resources/real-courses.md), which require agent use, rather than courses such as Harvard's CS50x, which [bans most AI tools](https://cs50.harvard.edu/x/honesty/) other than its own.

1. **AI use is required** in labs and the capstone. Using any coding agent is allowed unless a lab says otherwise.
2. **Document it.** Each lab and each capstone PR includes an AI-use log: which agent and model, what you asked it to do, and what you changed afterward. The Linux kernel's `Assisted-by: AGENT_NAME:MODEL_VERSION [tools]` tag ([FOSSID](https://fossid.com/articles/linux-kernel-ai-coding-policy-enterprise-accountability/)) is an acceptable commit format.
3. **You own what you submit.** A named human approves every merge. You must be able to explain any code, test, or spec you submit; the oral defense checks this. As in the kernel policy, an AI tool never signs off on your behalf.
4. **Do not feed secrets or other people's private data to an agent.** Use the secret scanning set up in Module 3.
5. **Readings and reflections are your own words.** You may use AI to find or summarize sources, but reflections and ADR rationale must be written by you.
6. **No-cost path.** No student is required to pay for a tool. Every lab has a Gemini CLI path on its free tier.
7. **Breaches** (undisclosed AI use, submitting work you cannot explain) are handled under the university's academic integrity policy.
