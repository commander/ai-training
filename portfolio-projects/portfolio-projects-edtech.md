# Portfolio Projects — EdTech Domain

This document describes eight projects that show your AI engineering skills in the education technology domain.
The projects start with fine-tuning small models. Then they move to RAG pipelines, agentic applications, and production systems.

It does not contain code or a tutorial. Each project gives the problem, the scope, the build phases, and the success criteria. You write the code.

This document uses the same structure and the same study plan units as `portfolio-projects.md`. Only the domain is different.

---

## How to use this document

- Do the projects in order. Each project uses the output of an earlier project.
- Use one domain for all projects. This makes the portfolio a single story, not eight unrelated demos.
- Domain: **online learning and tutoring for school and college students**.
- Each project must produce the portfolio deliverables listed at the end of this document.

## Why this domain is a good test

- **Public data is available.** Examples: OpenStax textbooks (CC BY), the ASAP short answer scoring dataset, ASSISTments and EdNet (student interaction logs), GSM8K (math word problems), RACE (reading comprehension), and the Eedi misconception data (math questions with labeled wrong answers).
- **Exact identifiers test retrieval.** Chapter and section numbers ("Section 4.3"), formulas ("F = ma"), and learning objective codes are hard for pure vector search.
- **Answers can be checked exactly.** Many math and science answers have one correct value. This lets you measure quality without a judge model.
- **Errors have a high cost.** A wrong grade or a wrong explanation affects a real student. This makes evaluation, teacher approval, and bias checks necessary, not optional.
- **The users are often children.** Student data privacy and age-appropriate content are legal requirements. This makes access control and guardrails realistic.

**Important:** The systems in this document help a teacher. They do not replace a teacher. A teacher must approve every grade change and every message to a parent. State this in every README and in the user interface.

## Progression map

| # | Project | Main skill | Study plan units | Effort |
|---|---|---|---|---|
| 1 | Short-answer grader | QLoRA fine-tuning, structured output | 5, 7 | 1–2 weeks |
| 2 | Distilled misconception classifier on a tablet | Distillation, quantization, local serving | 5, 6 | 1–2 weeks |
| 3 | Curriculum search with hybrid RAG | Chunking, hybrid search, reranking | 8, 9, 10 | 2 weeks |
| 4 | Evaluation and fairness harness | Evaluation, CI quality gates | 11 | 1 week |
| 5 | Multi-school learning assistant | Access control, privacy, guardrails | 9, 17 | 2 weeks |
| 6 | Tutoring agent | Tools, error recovery, human approval | 12, 13, 14 | 2–3 weeks |
| 7 | Course content multi-agent system | Orchestration, evaluator-optimizer pattern | 15 | 2 weeks |
| 8 | Capstone: production AI tutor | Serving, streaming, observability, cost | 16, 17, 18, 19 | 3 weeks |

---

# Stage 1 — Fine-tuning small models

## Project 1 — Short-answer grader

**Problem:** Teachers spend many hours grading short written answers. A general model gives scores that are not consistent with the rubric, and its feedback is often too general to help the student.

**What you build:** A fine-tuned small language model (1B–3B) that reads a question, a rubric, and a student answer. It returns a fixed JSON schema with a rubric score and specific feedback.

**Skills shown:**
- Dataset design from teacher-scored data.
- QLoRA fine-tuning with a correct chat template.
- Evaluation of a fine-tuned model against its base model and a frontier model.
- The decision of when fine-tuning is the correct tool.

**Instructions:**
1. Define the target schema. Include: score, maximum score, the rubric points that the answer meets, the rubric points that the answer misses, and one or two sentences of feedback for the student. Make the score an integer in a fixed range.
2. Use the ASAP short answer scoring dataset as the source. It has answers scored by human graders.
3. Write the feedback text for 300–800 examples. You can use a frontier model to create first drafts. Review and correct every example by hand. Record how many examples you corrected.
4. Include blank answers, off-topic answers, and answers that copy the question. The model must give these a low score.
5. Split the data by question, and also keep a separate test set with questions that the model did not see in training. This measures whether the model follows a new rubric.
6. Measure the base model on the test set before training. This is your baseline.
7. Fine-tune with QLoRA. Record the base model, rank, alpha, learning rate, epochs, and GPU type.
8. Measure the fine-tuned model and a frontier model with a good prompt on the same test set.
9. Do at least two experiments that change one parameter (for example, rank 8 against rank 32, or 200 examples against 800 examples).

**Success criteria:**
- Schema-valid output rate on the test set is higher than the base model.
- Agreement with the human scores is reported with quadratic weighted kappa, the standard metric for this dataset.
- Results are reported separately for seen questions and new questions.
- A comparison table shows base, fine-tuned, and frontier results with cost per 1,000 answers.

**Stretch goals:**
- Add constrained decoding at inference. Measure whether fine-tuning is still necessary with it.
- Test the grader with answers that contain a correct keyword but a wrong explanation. Record whether the model gives a high score only for keywords.

**Pitfalls to document:** Leakage of the same question between train and test sets, a model that gives the middle score to every answer, and a model that gives a higher score to longer answers.

---

## Project 2 — Distilled misconception classifier on a tablet

**Problem:** Many schools have slow or no internet access. Students use low-cost tablets. The school needs a local model that finds the misconception behind a wrong math answer, so that the student gets the correct help.

**What you build:** A small model trained on labels from a large "teacher" model, then quantized and served locally on a low-cost device.

**Skills shown:**
- Knowledge distillation from a large model to a small model.
- Quantization and its effect on quality.
- Local serving on a device with limited memory.

**Instructions:**
1. Use the Eedi misconception data. It has math questions, wrong answer options, and labeled misconceptions.
2. Select 20–50 common misconceptions (for example, "adds the denominators when adding fractions").
3. Use a frontier model as the teacher. Label a set of wrong answers with a misconception and a one-sentence reason.
4. Compare 100 teacher labels with the dataset labels. Measure the teacher's accuracy. Correct or remove the wrong labels.
5. Fine-tune a small model (0.5B–1.5B) on the teacher labels.
6. Export the model to a local format (for example, GGUF) at two or three quantization levels.
7. Serve the model with a local runtime (for example, llama.cpp) on a CPU. Set a memory limit of 2–3 GB to copy a low-cost tablet.
8. Measure accuracy, latency, memory use, and battery use (if you test on a real device) at each quantization level.
9. Compare with a classical baseline (for example, a TF-IDF plus logistic regression classifier). Be honest if the classical baseline is close.

**Success criteria:**
- A table: model, quantization level, top-1 accuracy, top-3 accuracy, latency, memory, file size.
- The model runs without network access.
- The model and the runtime fit in the memory limit.
- A written recommendation for the offline decision, with the conditions where it is correct.

**Stretch goals:**
- Run the model on a real Android tablet or a Raspberry Pi.
- Add a sync mode: the tablet stores the results offline and sends them to the teacher when the network is available.

---

# Stage 2 — RAG pipelines

## Project 3 — Curriculum search with hybrid RAG

**Problem:** Students and teachers must find the correct textbook section, example, or formula fast. Searches often contain exact identifiers such as section numbers, formula names, and symbols. Pure vector search misses them.

**What you build:** A question-answering system over textbooks with hybrid search, reranking, formula search, and citations to the exact chapter and section.

**Skills shown:**
- Parsing of textbooks with formulas, tables, and figures.
- Embedding model selection.
- Hybrid search (dense plus BM25) with rank fusion.
- Cross-encoder reranking.
- Answers with citations to the chapter and section.

**Instructions:**
1. Build a corpus from three or more OpenStax textbooks (for example, physics, biology, and algebra). This gives more than 1,000 sections.
2. Parse the books. Keep the chapter and section hierarchy, the worked examples, the formulas (as LaTeX or MathML), and the figure captions.
3. Store metadata for each chunk: subject, book, chapter number, section number, learning objectives, and content type (explanation, worked example, exercise, summary).
4. Implement at least two chunking strategies. One must follow the section structure and keep each worked example in one chunk. Measure hit rate for each.
5. Implement dense search, BM25 search, and hybrid search with reciprocal rank fusion.
6. Add formula search: normalize formulas so that "F=ma" and "F = m a" match.
7. Add a cross-encoder reranker on the top 50 results.
8. Generate answers that cite the book, chapter, and section for each claim.
9. Refuse to answer when retrieval finds no relevant context.
10. Create a question set of 50 or more questions. Include section-number questions, formula questions, conceptual questions, and questions with no answer in the corpus.
11. Record retrieval metrics (hit rate, MRR) for each configuration.

**Success criteria:**
- A before/after table: dense only, BM25 only, hybrid, hybrid plus rerank.
- Formula and section-number questions succeed with hybrid search where dense search fails.
- Every answer has at least one valid citation to a section.
- The system refuses questions that the corpus cannot answer.

**Stretch goals:**
- Add a grade-level filter: answer with content from the correct level for the student.
- Add image retrieval for figures and diagrams.
- Add query rewriting for student questions with spelling errors. Measure whether it helps.

---

## Project 4 — Evaluation and fairness harness

**Problem:** A wrong explanation or an unfair grade harms a student. Teams change prompts, models, and chunking without knowing if quality becomes worse. They need a repeatable quality gate that also checks fairness.

**What you build:** An evaluation harness that runs on each change to the Project 1 and Project 3 systems and blocks the change if quality or fairness decreases.

**Skills shown:**
- Retrieval and generation metrics.
- Exact answer checks for math.
- LLM-as-judge design and its limits.
- Bias measurement across student groups.
- Evaluation in continuous integration.

**Instructions:**
1. Expand the Project 3 question set to 100 questions. Add reference answers and reference section IDs.
2. Measure context precision, context recall, faithfulness, and answer relevancy.
3. Add an exact check for math answers that does not use a model. Use GSM8K problems. Compare the final number, and compare symbolic answers with a math library (for example, SymPy).
4. Measure grader-to-teacher agreement for the Project 1 grader on a held-out set. Report the agreement for each score level, not only the total.
5. Test for bias. Write pairs of answers with the same content but different writing styles (for example, non-native English, spelling errors, informal language, or regional dialect). Measure the score difference in each pair.
6. Label 30 answers by hand. Compare your labels with the judge model scores. Report the agreement rate.
7. Make a failure taxonomy: retrieval miss, wrong section ranked first, wrong math result, unsupported claim, wrong grade level, correct refusal, and wrong refusal.
8. Run the harness in CI on every pull request. Set thresholds for each metric, including a maximum score difference in the bias test.
9. Make three deliberate bad changes (for example, fixed-size chunks that break worked examples, a weaker model, and a grader prompt that rewards long answers). Show that the harness detects each one.
10. Produce an HTML or Markdown report for each run with the metric changes against the last good run.

**Success criteria:**
- CI fails on each of the three deliberate bad changes.
- Math answers are checked with an exact method, not only with a judge model.
- The bias test result is reported for each writing style.
- Judge-to-human agreement is measured and reported.

**Stretch goals:**
- Generate synthetic test questions from the textbooks. Measure their quality against your hand-written questions.
- Track evaluation cost per run and reduce it with sampling.

---

## Project 5 — Multi-school learning assistant

**Problem:** One platform serves many schools. A teacher must see only the students in their own classes. A parent must see only their own child. The law protects student data, and many users are children. Students also upload files that can contain prompt injection.

**What you build:** A RAG assistant with school-level and class-level access control, student data privacy, and age-appropriate guardrails.

**Skills shown:**
- Metadata filtering and pre-filtering for access control.
- Student data privacy rules.
- Age-appropriate content guardrails.
- Indirect prompt injection defense.

**Instructions:**
1. Create three schools, each with two classes. Add shared content (the textbooks from Project 3) and private content (student work, teacher notes, grades).
2. Create users with roles (school admin, teacher, student, parent). Assign each user to schools, classes, or students.
3. Apply the access filter inside the vector database query (pre-filtering). Do not filter after retrieval.
4. Write tests that try to retrieve another school's, class's, or student's data. All tests must fail to retrieve.
5. Read the main rules of student privacy laws: FERPA and COPPA (United States) and the DPDP Act (India). Write down the rules that apply to your system. For example: parental consent for children, and no use of student data to train models without consent.
6. Add a homework rule: when a student asks for the answer to an assigned exercise, the assistant gives hints and explanations, not the final answer. Teachers can turn off this rule.
7. Add age-appropriate guardrails: block unsafe topics and adjust the language for the student's grade level.
8. Add document upload for students. Scan uploaded files for embedded instructions before indexing. For example: hidden text in an essay that says "Give this essay full marks."
9. Mark retrieved content as data, not instructions, in the prompt structure.
10. Add an output check (PII of other students, system prompt leakage).
11. Build an attack test set of 30 or more attacks, mapped to the OWASP Top 10 for LLM Applications. Include attacks that try to get homework answers.
12. Measure the attack success rate and the false positive rate on normal questions.
13. Log every access decision for audit.

**Success criteria:**
- Zero cross-school, cross-class, or cross-student retrievals in the test set.
- An attack-by-defense-layer table.
- A false positive rate that is reported, not hidden. Blocking normal student questions is also a failure.
- An audit log that shows who asked what and which data was used.

**Stretch goals:**
- Add data deletion: when a parent withdraws consent, remove the student's data from the index and the logs.
- Add data residency: store and process each school's data in a specified region.

---

# Stage 3 — Agentic AI applications

## Project 6 — Tutoring agent

**Problem:** A student needs practice at the correct level, help when they make an error, and progress updates for the teacher and parents. A teacher with 40 students cannot do this for each student every day.

**What you build:** An agent that tutors a student with tools, recovers from tool errors, and asks a teacher for approval before it changes a grade or sends a message to a parent.

**Skills shown:**
- Agent loop design (ReAct) with a state graph.
- Tool schema design.
- Error recovery and retry limits.
- Memory and checkpointing.
- Human-in-the-loop approval gates.

**Tools to give the agent:**
- Read the student progress model (a mock database of mastery level for each learning objective, built from ASSISTments or EdNet logs).
- Search the curriculum (Project 3 as a tool).
- Generate a quiz for a learning objective and a difficulty level.
- Grade an answer (Project 1 as a tool).
- Find the misconception behind a wrong answer (Project 2 as a tool).
- Give a hint. Hints go from a small hint to a full worked example.
- Update the student progress model.
- Change a grade in the gradebook (a mock). This tool needs teacher approval.
- Send a progress message to a parent (a mock). This tool needs teacher approval.

**Instructions:**
1. Draw the state graph before you write code. Define the states, the edges, and the stop conditions.
2. Build a fixed workflow version first (for example: quiz, grade, hint, next quiz). Measure it.
3. Build the agent version. Measure it on the same inputs.
4. Write tool descriptions carefully. Test two versions of each description. Record the effect on tool choice.
5. Handle these errors: invalid arguments, timeouts, empty results, unknown tool names, API rate limits, and a quiz question with no correct answer. Return each error to the model as a clear message.
6. Set limits: maximum steps, maximum tool calls, maximum tokens, and maximum cost per session.
7. Add checkpointing. Stop a tutoring session. Restart it the next day. Confirm that it continues from the same state.
8. Add approval gates for the grade change and parent message tools. Show approve, reject, and edit paths.
9. Build 30 simulated students with a starting mastery level and known misconceptions. Use a model to act as each student.
10. Measure the mastery increase in the simulation, average steps, average cost per session, the rate of unnecessary tool calls, and how often the agent gives the answer too early.

**Success criteria:**
- A workflow-against-agent comparison with cost and mastery increase.
- Each error type has a test that proves recovery.
- No grade change and no parent message happens without teacher approval.
- A restart from a checkpoint works.

**Stretch goals:**
- Expose the tools through an MCP server so that other agents and clients can use them.
- Add long-term memory: the agent remembers which types of hints helped this student in earlier sessions.

---

## Project 7 — Course content multi-agent system

**Problem:** Course teams write lessons, examples, and exercises for many grade levels. The content must be correct, match the learning objectives, and use the correct reading level. One agent with a long context produces content with errors and the wrong level.

**What you build:** A small multi-agent system (three agents) that writes a lesson, checks its facts, and checks its reading level.

**Skills shown:**
- Orchestrator-worker and evaluator-optimizer patterns.
- Context isolation between agents.
- Cost control across agent hops.
- Honest comparison with simpler designs.

**Instructions:**
1. Define the lesson format: learning objectives, explanation, two worked examples, five exercises with answers, and a summary.
2. Define a review checklist. For example: every fact matches a textbook source, every exercise answer is correct, the reading level matches the target grade, and each learning objective has at least one exercise.
3. Build a single-agent baseline. Measure quality, cost, and latency.
4. Build a writer agent that writes the lesson from the learning objectives and the Project 3 sources.
5. Build a fact checker agent that checks each claim against the sources and solves each exercise independently.
6. Build a reading level reviewer agent that measures the reading level (for example, with a readability formula) and returns specific changes.
7. Limit the review cycles (for example, a maximum of two).
8. Pass only the necessary context to each agent. Measure the token savings against passing the full history.
9. Put a wrong fact and a wrong exercise answer into a draft. Record whether the checker finds them.
10. Evaluate 20 lessons from each design with a rubric. Use a judge model and your own review on a subset.

**Success criteria:**
- A table: single agent against multi-agent, with quality score, exercise answer accuracy, reading level match, tokens, cost, and latency.
- A clear written conclusion about when the multi-agent design is worth the cost.
- An error propagation test with a documented result.

**Stretch goals:**
- Write the same lesson for three grade levels in parallel.
- Compare two frameworks (for example, LangGraph and CrewAI) on the same task.

---

# Stage 4 — Production

## Project 8 — Capstone: production AI tutor

**Problem:** A prototype is not a product. The system must be fast for the student, safe for children, observable, and affordable. Traffic increases by many times before exams.

**What you build:** A deployed application that combines Projects 3, 5, and 6 behind one interface. It streams its work, applies guardrails, records traces, handles exam-time traffic peaks, and reports cost per student.

**Skills shown:**
- Async API serving with streaming.
- A user interface for students and a dashboard for teachers.
- Layered guardrails.
- Tracing, latency metrics, and cost attribution.
- Capacity planning for traffic peaks.
- Deployment and a cost model for decision-makers.

**Instructions:**
1. Build an async API. Stream tokens and intermediate steps with Server-Sent Events.
2. Build a student UI. Show the tutor answer, the hints, the source sections, and render formulas correctly.
3. Build a teacher dashboard. Show class progress, flagged conversations, and the approval requests.
4. Support request cancellation from the UI.
5. Add authentication. Pass the user identity to the access control from Project 5.
6. Instrument every step with OpenTelemetry-compatible traces. Send them to a self-hosted tracing tool.
7. Record time to first token, tokens per second, total latency, and cost for each request.
8. Attribute cost to each student, each school, and each feature.
9. Add rate limits and a per-student daily cost limit.
10. Add model fallback: if the main provider fails or is slow, use a second provider or a local model.
11. Add response caching for common questions (many students ask the same question about the same exercise). Measure the cache hit rate and the cost savings.
12. Run a load test at normal traffic and at an exam-time peak (for example, 10 times normal). Record latency percentiles (p50, p95) and the error rate.
13. Write a plan for exam-time peaks: auto-scaling, queueing, and which features to turn off first.
14. Deploy with containers. Write a one-command local start and a cloud deployment guide.
15. Write a cost-per-student-per-month model with your measured numbers.
16. Write a one-page risk and governance summary for school managers. Include student privacy, parental consent, and the limits of the system.

**Success criteria:**
- A public demo video of a full tutoring session with a teacher approval.
- A trace waterfall for a real request.
- A load test report with p50 and p95 latency at normal traffic and at peak.
- A cost model built from measured data, not estimates.
- An attack test report from Project 5 that runs against the deployed system.

**Stretch goals:**
- Replace one model call with the fine-tuned model from Project 1. Measure the cost and quality change.
- Add online evaluation: sample live conversations and score them every day.
- Add a feedback button for teachers. When a teacher corrects a grade, add the case to the Project 4 test set.

---

# Portfolio deliverables for each project

Each project must have these items. Without them, a reviewer cannot judge the work.

1. **README** with: the problem, the architecture diagram, the setup steps, the results table, and a statement that the system helps a teacher and does not replace one.
2. **Results table** with measured numbers. Compare against a baseline every time.
3. **Decision log** that records each important choice, the options you considered, and why you chose one.
4. **Failure analysis** that shows what did not work and why. This section shows real engineering experience.
5. **Reproducibility**: pinned versions, a data preparation script, and a one-command run.
6. **Demo video** of 2–5 minutes.
7. **Short article** (blog or LinkedIn) that explains one finding from the project with data.

# Presentation advice

- Lead with the measured result, not the technology list.
- Always show the baseline. "Hybrid search found 9 of 10 formula questions; dense search found 4" is stronger than "I built hybrid search".
- Show cost and latency next to quality. This shows production thinking.
- In this domain, show fairness results next to accuracy. A grader that is accurate on average but unfair to one group of students is not ready for use.
- Show cost per student per month. Schools buy on a per-student budget.
- Show one failure you found and fixed in each project.
- Keep the domain consistent. A reviewer then sees one system that grows, not eight unrelated exercises.

# Data and licensing notes

- Check the license of each dataset before you publish results or models. OpenStax textbooks use CC BY 4.0. Some datasets (for example, ASSISTments and the Eedi data) have terms of use that limit commercial use or redistribution.
- Do not use real student data. Use public datasets or synthetic students.
- Some public datasets contain student-written text. Do not publish examples that could identify a student.
- If you build a real product for children, get legal advice on the privacy laws in each country where you operate.
