# University courses on AI-assisted SE and evidence-based assessment design

Research date: October 8, 2026. All facts below carry a source URL. "Verified" means read on the primary page during this session. Where a fetched page did not show something, it is listed under Gaps rather than guessed.

## Q1. Does a study of "23 syllabi" exist?

### Takeaway
Yes. Geng et al., "Mapping the Emerging Curriculum for AI-Assisted Software Engineering via Syllabus Analysis" (arXiv:2608.05898, submitted August 6, 2026) analyzed 23 syllabi of US upper-division AI-assisted SE courses. It reports **descriptive** assessment weights (medians/ranges across 14 percentage-graded courses) but **does not recommend weights**. The draft syllabus claim is accurate only if reworded: "informed by/benchmarked against" observed weights. It is not "recommended weights synthesized from empirical data" in the paper's sense.

### Cited findings
- Citation: Francis Geng, Anshul Shah, Mia Chen, Paul Denny, Juho Leinonen, Bill Griswold, Gerald Soosai Raj, Leo Porter. "Mapping the Emerging Curriculum for AI-Assisted Software Engineering via Syllabus Analysis." arXiv:2608.05898 [cs.SE], submitted August 6, 2026. Venue: arXiv preprint only (no peer-reviewed venue shown on the abstract page). — [arXiv abs](https://arxiv.org/abs/2608.05898)
- Abstract: "We analyzed 23 publicly available syllabi and course materials of upper-division, credit-bearing courses that meet specific criteria, including explicitly addressing Generative AI in software engineering. Through iterative qualitative coding, we characterized courses' learning objectives, assessments, topics, and documented AI tools." — [arXiv abs](https://arxiv.org/abs/2608.05898)
- Method: Google searches of .edu and github sites, March 10–30, 2026. Institutional screening (US, credit-bearing, upper-division) left 32 candidates. Content screening left 23. The content criteria were: explicit GenAI framing, coverage of at least 2 SDLC phases, and required GenAI use in graded SE work. — [arXiv HTML](https://arxiv.org/html/2608.05898)
- Assessment weights across **14** conventional percentage-graded courses (the other 9 were not percentage-based or not public). Median and range by category: — [arXiv HTML](https://arxiv.org/html/2608.05898)
  - Participation/engagement: in 14 courses, median 17.5%, range 5–46%
  - Project/capstone: in 12 courses, median 50%, range 30–85%
  - Coding assignments: in 9 courses, median 20%, range 10–60%
  - Review/reflection: in 6 courses, median 19%, range 10–40%
  - Paper presentation: in 4 courses, median 20%, range 10–20%
  - Quiz/exam: in 4 courses, median 15%, range 10–30%
- Assessment features (14 courses): AI-required work median 70% (range 20–100%), programming work median 70%, in-class work median 31%, group work median 55%. Proctored assessment appeared in only 3 courses (median 20%, range 10–30%). — [arXiv HTML](https://arxiv.org/html/2608.05898)
- Tools named (9 courses named tools): Claude Code 6, Cursor 6, GitHub Copilot 3. All others appeared in a single course. — [arXiv HTML](https://arxiv.org/html/2608.05898)
- Common topics (18 courses): AI/ML fundamentals 12, testing 10, agentic development 10, prompting 9. — [arXiv HTML](https://arxiv.org/html/2608.05898)
- The paper gives no weight prescriptions. Its only design suggestion is that "process logs, code reviews, demonstrations or oral defenses, individual reflections, and in-class checkpoints may help preserve project-based work while strengthening assessment of individual understanding." — [arXiv HTML](https://arxiv.org/html/2608.05898)
- Stated limitation: "course-material analysis also cannot show how instructors enacted these courses, how students used AI tools in practice, or how effective the courses were for learning." — [arXiv HTML](https://arxiv.org/html/2608.05898)
- Related but different study: Ali et al. analyzed **98** computing syllabi from 54 R1 institutions for GenAI *policies*, not SE assessment weights (SIGCSE TS 2025). — [arXiv 2410.22281](https://arxiv.org/pdf/2410.22281); [SIGCSE 2025 listing](https://sigcse2025.sigcse.org/details/sigcse-ts-2025-Papers/23/Analysis-of-Generative-AI-Policies-in-Computing-Course-Syllabi)

### Inferences
- The draft wording "recommended weights synthesized from empirical data" overstates the source in two ways:
  - The weights are descriptive medians from 14 courses, not 23.
  - The paper has no evidence on learning outcomes.
- Safer wording: "Weights are benchmarked against the medians reported in Geng et al. (2026), arXiv:2608.05898, across 14 percentage-graded courses out of 23 analyzed. For example, the project/capstone median is 50%."
- A median is not a recommendation. Courses near the 50% project median still differ a lot in how much proctored or individual verification they include.

### Gaps
- I did not confirm peer-reviewed publication of 2608.05898, for example acceptance at SIGCSE 2027 or ICSE-SEET. It should be cited as a preprint.
- The paper's HTML did not say which 14 courses fed the weight table.

## Q2. Real courses teaching SE with AI coding agents, and how they assess

### Takeaway
At least 23 US courses exist (listed in Geng et al.). Of those I checked directly:
- **CMU 15-113/114** is the clearest model of oral verification. It uses TA mock-interview evaluations after each project, and "Oral Evaluations + Quizzes" count for 20%.
- **Michigan EECS 498** has no exams and puts about 50% on the final "Create" phase.
- **Northeastern CS 7180** puts 50% on projects and uses quizzes as the AI-independent check.

I could not verify Stanford CS146S's grading.

### Course list from Geng et al.
Courses as listed in the paper, with term and URL. I have not individually verified these unless marked in the table further below. — [arXiv HTML](https://arxiv.org/html/2608.05898)

1. Kalamazoo College, AI-Assisted Software Development, Fall 2023 — cs.kzoo.edu/cs488/syllabus.html
2. Purdue, AI-Assisted Software Engineering (CS 59200), Spring 2024 — tianyi-zhang.github.io/files/CS59200_AI_Assisted_Software_Engineering_Syllabus.pdf
3. Virginia Tech, AI Tools for Software Delivery (5914), Spring 2024 — hosting.cs.vt.edu/specialtopics/graduate/spring24/5914-atkinson.html
4. UIUC, Software Quality Assurance with Generative AI (CS 598 LMZ), Spring 2025 — lingming.cs.illinois.edu/courses/cs598lmz-s25.html
5. Northwestern, Applied AI for Software Development (397), Spring 2025
6. CMU, AI Tools for Software Development, Fall 2025 — ai-developer-tools.github.io
7. Harvard, Software Engineering with Generative AI, Fall 2025 — materials not public
8. NC State, Generative AI for Software Engineering, Fall 2025 — github.com/gai4se/GAI4SE-Course
9. Stanford, The Modern Software Developer (CS146S), Fall 2025 — themodernsoftware.dev
10. UMD, Effective Use of AI Coding Assistants and Agents (CMSC 398Z), Fall 2025
11. CU Boulder, Generative AI-Powered Software Engineering (CSCI 7000), Fall 2025
12. UW, AI-Assisted Software Development (CSE 490A2), Fall 2025
13. UVA, Software Engineering and LLMs, Fall 2025
14. CMU, Effective Coding with AI (15-113), Spring 2026 — cs.cmu.edu/~113
15. NJIT, AI-Assisted Software Engineering (CS 485), Spring 2026
16. UChicago, Design, Build, Ship (MPCS 51238), Spring 2026
17. UIUC, Software Engineering with LLM Agents, Spring 2026
18. Univ. of Memphis, AI Tools for Software Development, Spring 2026
19. Univ. of Utah, Vibe Coding (CS 3960), Spring 2026
20. UCSD, Generative AI and Programming (CSE 115/215), Spring 2026
21. Florida Atlantic Univ., Generative AI SDLCs (COT 6930), Spring 2026
22. Northeastern, AI-Assisted Software Engineering (CS 7180), Spring 2026 — johnguerra.co/classes/aiCoding_spring_2026
23. Univ. of Michigan, Applied Agentic Software Engineering (EECS 498), Fall 2026 — eecs498-aase.github.io

### Courses verified directly on their own pages

| Course | Instructor | Term | Grading | Oral/AI verification | Source |
|---|---|---|---|---|---|
| CMU 15-113/15-114 Effective Coding with AI | Mike Taylor | Fall 2026 (minis) | Homework 20%, participation 20%, big projects 30%, TA meetings 10%, oral evaluations + quizzes 20% | A TA interview after each project "in the style of a mock job interview," graded on process, transparency and ownership. Project 3 has a final oral exam or presentation. AI use must be documented. Submitting AI code you cannot explain is not allowed. | [cs.cmu.edu/~113](https://www.cs.cmu.edu/~113/) |
| Michigan EECS 498-016 Applied Agentic SE | Marcus Darden (lead) | Fall 2026 | Administrative 10%, Apply 18%, Analyze 22.5%, Create 49.5%. No exams. | Graded on projects, labs and demos, with a final showcase. Uses Aider, then a custom agent loop, then a production assistant. | [eecs498-aase.github.io](https://eecs498-aase.github.io/index.html) |
| Northeastern CS 7180 AI-Assisted SE ("Vibe Coding") | John Alexis Guerra Gómez | Spring 2026 (Oakland campus) | Participation 15%, weekly quizzes 10%, 5 homeworks 25%, 3 projects 50% | No oral exam. Quizzes test understanding "independent of AI assistance." AI use is required, must be documented, and students must be able to explain their code. The final pair project includes a live demo. | [johnguerra.co](https://johnguerra.co/classes/aiCoding_spring_2026/) |
| Stanford CS146S The Modern Software Developer | Mihail Eric | Fall 2025; Fall 2026 listed | **Not found** on the landing page | Final project "showcasing modern development practices" | [themodernsoftware.dev](https://themodernsoftware.dev); [Stanford bulletin](https://bulletin.stanford.edu/courses/2274401) |
| UW CSE 490A2 AI-Assisted Software Development | Michael Ernst | Autumn 2025 | **Not found** (grading is on Canvas) | Tools named: Cursor, Copilot, Claude Code | [UW course page](https://courses.cs.washington.edu/courses/cse490a2/25au/) |

### AI policy reference point: Harvard CS50 (CS50x)
- CS50x allows "CS50's own AI-based software, including the CS50 Duck (ddb)" and bans other AI "(e.g., ChatGPT, Claude, Copilot, Gemini, et al.) that suggests or completes answers to questions or lines of code." — [CS50x academic honesty](https://cs50.harvard.edu/x/honesty/)
- This is the opposite of the AI-native courses above, which require agent use.

### Industry courses
- DeepLearning.AI with JetBrains, "Spec-Driven Development with Coding Agents," taught by Paul Everitt. Beginner level, about 1 h 16 min, 15 video lessons and 1 graded assignment (PRO). Covers a project "constitution," feature specs, and plan-implement-verify loops. — [DeepLearning.AI](https://www.deeplearning.ai/short-courses/spec-driven-development-with-coding-agents/)

### Inferences
- The project weights I verified (30%, about 50% for the Create phase, 50%) match the Geng median of 50%.
- CMU is the only verified course that pairs that weight with recurring individual oral checks.

### Gaps
- Stanford CS146S grading and AI policy: the landing page had no breakdown, and cs146s.stanford.edu did not resolve.
- UW grading is on Canvas, which is not public.
- I did not fetch the remaining 18 courses' pages.
- No MIT, UC Berkeley or University of Toronto course turned up in this search or in Geng et al. That is not proof none exist, and Geng et al. covered US institutions only, so Toronto was out of its scope.
- Anthropic Academy / Claude Code courses: not verified this session.

## Q3. Evidence for oral defenses, process/artifact grading and capstone weighting

### Takeaway
There is peer-reviewed evidence that oral and code-interview formats can work and scale. The evidence is mostly design-and-experience reports with small samples, not controlled outcome studies. I found no study establishing an optimal capstone weight.

### Cited findings
- **Code Interviews.** Kannam, Yang, Dharm, Lin. "Code Interviews: Design and Evaluation of a More Authentic Assessment for Introductory Programming Assignments," SIGCSE TS 2025.
  - Replaced invigilated-exam-style checks with discussions of students' own take-home work.
  - Findings: interviews pushed students to discuss their work in more nuanced (sometimes repetitive) ways. Peer formats cut some stress but added other kinds. Scaling was hard: TA sections were converted for 5 weekly assignments. The focus on assessment limited TA feedback.
  - [arXiv 2410.01010](https://arxiv.org/abs/2410.01010)
- **Conversational Exam.** Barba and Stegner, "The Conversational Exam: A Scalable Assessment Design for the AI Era," *Computer* 59(6):132–139, June 2026.
  - Live-coding oral exams while explaining reasoning. Supervised AI access is allowed.
  - Ran 58 students in small groups over 2 days.
  - [arXiv 2601.10691](https://arxiv.org/abs/2601.10691)
- **AI-run oral exams.** SIGCSE TS 2026 lightning talk, "AI-Driven Oral Examinations for Code Assessment." Uses automated conversational checks of students' own submissions, built into the Classmoji LMS. This is a lightning talk, not a full paper. — [SIGCSE 2026 program](https://sigcse2026.sigcse.org/details/sigcse-ts-2026-lightning-talks/17/AI-Driven-Oral-Examinations-for-Code-Assessment-Evaluating-Understanding-Beyond-the-)
- **Geng et al. (2026)** suggest process logs, code reviews, demos or oral defenses, reflections and in-class checkpoints to strengthen assessment of individual understanding. This is a suggestion, not a tested result. — [arXiv HTML](https://arxiv.org/html/2608.05898)
- **ITiCSE 2023 working group.** Prather et al., "The Robots are Here: Navigating the Generative AI Revolution in Computing Education," ITiCSE Working Group Reports 2023 (Turku). Covers opportunities, curricular changes and the rethinking of assessment. — [arXiv 2310.00658](https://arxiv.org/pdf/2310.00658)
- **Related policy studies:**
  - "To Police or to Guide," on how CS instructors design GenAI policies. — [arXiv 2607.16475](https://arxiv.org/pdf/2607.16475)
  - Institutional vs. course GenAI policy comparison, which found course-level uptake "still guarded." — [arXiv 2607.12296](https://arxiv.org/abs/2607.12296)
- **Practice example:** CMU 15-113 grades process, transparency and ownership through TA mock-interviews after each project. — [cs.cmu.edu/~113](https://www.cs.cmu.edu/~113/)

### Inferences
- A defensible design based on these sources:
  - Project/capstone around 40–50%, in line with the Geng median.
  - 15–25% for individually verified components: oral defense or code interview, and/or proctored quiz. CMU uses 20%, and the Geng proctored median is 20%.
  - Explicit AI-use documentation (process logs).
- This is my synthesis, not a finding from any one study.

### Gaps
- I found no controlled study comparing learning outcomes across assessment weightings in AI-assisted SE courses.
- I did not read the full papers for 2410.01010 or 2601.10691. The details above come from the abstract pages.
- ICSE-SEET 2025/2026 papers were not searched specifically.
