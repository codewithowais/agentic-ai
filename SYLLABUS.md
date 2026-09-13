# Agentic AI — From Foundations to Advanced Systems

**A 16-week, hands-on course on designing, building, and operating AI agents**

---

## Assumptions I made (adjust freely)

Before the syllabus itself, here are the calls I made where the brief left room. None are hard to change:

1. **Delivery format:** cohort-based, synchronous (in-person or live online), one 3-hour session per week, with all code in a shared repo. Sessions are recorded for catch-up.
2. **Primary model provider:** examples use a current frontier model via API (Claude by default; OpenAI/others work identically). Every pattern is written to be **provider- and framework-agnostic** — the model is a swappable dependency, never the lesson.
3. **Primary framework:** **LangGraph** (with LangChain primitives) is the spine from Week 7 onward, because its explicit graph model makes orchestration concepts teachable. **CrewAI** and **AutoGen** appear for comparison, not as parallel tracks.
4. **Class size:** 12–30 learners, working solo on labs and in pairs/trios for the capstone.
5. **Cost:** each learner needs ~$20–40 of API credit across the term (labs are designed to be cheap; we cache and use small models where we can).

If any of these are wrong — especially delivery format or provider — tell me and I'll re-cut the affected sections.

---

## 1. Course description

Agents are what you get when you stop treating a language model as a text box and start treating it as the reasoning core of a system that can *decide, act, observe, and try again*. This course is about building those systems well.

We assume you already understand machine learning basics, how large language models work, prompting, and embeddings — so we spend **zero** time re-explaining them. Instead, from day one we focus on the thing that is genuinely new and genuinely hard: **giving a model a loop, tools, memory, and autonomy, and keeping the result reliable, safe, and affordable.**

Over 16 weeks you move from building a single tool-using agent by hand, to multi-agent systems with supervisors and human-in-the-loop checkpoints, to production concerns — evaluation, tracing, reliability, security, and deployment. You'll write a lot of Python. By the end you'll have a portfolio of labs and a deployed capstone agent that a stranger could actually use.

## 2. Rationale — why this course, why now

Most "AI agent" material is either a marketing demo (works once, on stage) or a framework tutorial (teaches the API, not the ideas). Both age badly, because the tools change every few months. This course is built around the opposite bet: **the durable skill is understanding agent *patterns* — the loop, tool design, memory, orchestration, evaluation, guardrails — well enough that you can pick up whatever framework is current next year in an afternoon.**

So we build things by hand before we reach for a framework (you'll implement a ReAct loop from scratch in Week 3 before touching LangGraph in Week 7), and we treat evaluation, reliability, and security as first-class engineering — the parts that separate a demo from a product.

## 3. Prerequisites

**Required**
- Comfortable writing Python: functions, classes, type hints, virtual environments, `pip`/`uv`, reading a traceback, using `async`/`await` at a basic level.
- Working knowledge of AI/ML and LLM fundamentals: what a model, token, embedding, and context window are; what prompting and fine-tuning are; roughly how RAG works conceptually.
- Basic command line and Git (clone, branch, commit, push).
- Able to read JSON and reason about API request/response shapes.

**Helpful but not required**
- Prior REST API experience, basic Docker familiarity, exposure to a web framework (FastAPI/Flask).

**Explicitly NOT required** — we do not re-teach these: neural network internals, transformer architecture math, prompt-engineering basics, or "what is an LLM."

**A quick self-check:** if you can write a Python function that calls an LLM API, parses the JSON response, and handles an error, you're ready.

## 4. Course-level learning outcomes

By the end of the course, you will be able to:

1. **Explain and diagram** the anatomy of an agent (model, tools, memory, control loop) and justify when an agentic approach is — and isn't — the right choice.
2. **Implement** a tool-using reasoning loop (ReAct and reflection variants) from scratch in Python, without a framework.
3. **Design** robust agent tools with typed schemas, validation, and error handling against real external APIs.
4. **Build** grounded agents that combine working memory, long-term memory, and retrieval (RAG).
5. **Construct** multi-agent systems using a modern framework, with explicit orchestration (graphs, supervisor/worker) and human-in-the-loop checkpoints.
6. **Integrate** agents with the outside world through context engineering and the Model Context Protocol (MCP).
7. **Evaluate, trace, and debug** agent behavior using systematic methods and instrumentation, not vibes.
8. **Engineer** agents for reliability and cost: retries, caching, fallbacks, and latency/cost budgeting.
9. **Assess and mitigate** agent safety and security risks, including prompt injection, with guardrails and least-privilege permissions.
10. **Deploy and operate** an agent as a monitored service and articulate the basics of LLMOps.
11. **Deliver** an end-to-end capstone agent — designed, built, evaluated, and deployed — and defend your design decisions.

Each outcome maps to graded work; the mapping is in §8.

## 5. Required tools & tech stack

| Category | What we use | Notes |
|---|---|---|
| Language | **Python 3.11+** | `uv` for env/deps (or `venv`+`pip`) |
| LLM access | A frontier model API | Claude by default; any provider works. Everyone needs an API key + credit |
| Agent frameworks | **LangGraph + LangChain** (primary), **CrewAI**, **AutoGen** (comparison) | Introduced Week 7; hand-rolled before that |
| Retrieval | An embedding model + a vector store (Chroma or FAISS locally; a hosted option optional) | Week 6 |
| Tools/integration | `httpx`/`requests`, `pydantic` for schemas, **MCP** SDK | MCP in Week 11 |
| Evaluation & tracing | LangSmith **or** an open alternative (Langfuse/Phoenix); plus a lightweight custom eval harness | Week 12 |
| Serving | **FastAPI** + Uvicorn; Docker for packaging | Week 15 |
| Version control | Git + GitHub (course org) | All labs submitted as PRs |
| Editor | Any; VS Code / Cursor recommended | — |

### Setup notes (do before Week 1)
- Install Python 3.11+ and `uv` (`pip install uv` or the standalone installer).
- Create an account with the chosen LLM provider and generate an API key. **Add ~$10 credit to start.**
- Clone the course repo and run the provided `hello_agent.py` smoke test.
- **Store keys in a `.env` file — never commit keys.** A `.gitignore` and `.env.example` are provided. Week 1's lab checks this.
- A one-page "Setup & Troubleshooting" guide lives in the repo; Week 1's buffer time is reserved for fixing broken setups.

## 6. The 3-hour session structure

Every week follows the same rhythm so you always know where you are. Total: **180 minutes.** "Buffer" (recap, break, Q&A) is baked into every session — about 40 minutes of breathing room.

| Segment | Time | Purpose |
|---|---|---|
| **Recap & warm-up** *(buffer)* | 0:00–0:15 (15 min) | Review last week, connect to today, quick questions from homework |
| **Concept block I** | 0:15–0:55 (40 min) | New ideas, with live-coded demos |
| **Guided lab, part 1** | 0:55–1:35 (40 min) | Hands-on, instructor circulating |
| **Break** *(buffer)* | 1:35–1:50 (15 min) | — |
| **Concept block II** | 1:50–2:20 (30 min) | Deeper/second topic, patterns, pitfalls |
| **Guided lab, part 2 + stretch** | 2:20–3:00 mainly (40 min) | Finish the lab; stretch goals for the fast-moving |
| **Wrap, Q&A & homework brief** *(buffer)* | last 10 min (10 min) | Debrief, assign homework, open Q&A |

**Where the buffer lives:** ~40 min per session (recap + break + wrap/Q&A) absorbs the normal friction of hands-on work. On top of that, **Weeks 4, 8, and 12 are lighter "checkpoint" weeks** with less new material, deliberately leaving room to catch up, consolidate, and (in later ones) review capstone progress.

## 7. Assessment breakdown

| Component | Weight | What it covers |
|---|---|---|
| **Weekly labs** (12 graded, best 10 count) | 25% | Completion + correctness of in-class/finished labs, submitted as PRs |
| **Homework assignments** (weekly) | 15% | Extending labs; short applied problems |
| **Mini-project** (Week 4) | 10% | A small self-contained agent demonstrating the loop + reasoning patterns |
| **Checkpoint deliverables** (Weeks 8 & 12) | 10% | Capstone proposal (W8) and mid-capstone review (W12) |
| **Capstone project** (Weeks 13–16) | 30% | Design, build, evaluate, deploy, and present an end-to-end agent |
| **Participation & peer engagement** | 10% | Attendance, in-class contribution, peer code review, demo-day feedback |

**Grade weighting maps to outcomes:** labs/homework → outcomes 1–8; mini-project → 2, 4; checkpoints + capstone → 5–11.

> Labs are graded lightly (done-and-works) so you can experiment without fear. The capstone is where depth is rewarded.

## 8. Capstone project

**The brief:** design, build, evaluate, and deploy an agent (or small multi-agent system) that does something genuinely useful for a real user or workflow. It must use tools, some form of memory or retrieval, and demonstrate reliability and safety thinking. "A chatbot with no tools" does not qualify; "a research assistant that searches, reads, cross-checks, and drafts a cited summary" does.

**Examples of good scope:** a personal research/summarization agent with citations; a codebase Q&A agent over a real repo; a customer-support triage agent with human handoff; a data-analysis agent that queries a database and charts results; a multi-agent "content pipeline" (researcher → writer → editor).

**Requirements:**
- At least 2 well-designed tools with schemas + error handling.
- Memory and/or retrieval that meaningfully changes behavior.
- An **evaluation suite** (a small labeled test set + metrics) — not just a demo.
- **Reliability** features (retries/fallbacks/caching) and a **cost/latency** note.
- A **safety pass**: at least one guardrail and a least-privilege permission story.
- **Deployed** and reachable (an API endpoint or simple UI), with basic monitoring/tracing.
- A short written design doc + a 6–8 minute demo.

**Milestones (tied to weeks):**

| Milestone | Due | Deliverable |
|---|---|---|
| **Proposal** | **Week 8** | 1-page: problem, users, tools, memory/retrieval plan, success criteria |
| **Architecture sketch** | **Week 10** | Diagram of agents/graph, tools, data flow, handoffs, human-in-the-loop points |
| **Mid-capstone review** | **Week 12** | Working core loop + one real tool + first eval cases; 5-min status + feedback |
| **Reliability + safety pass** | **Week 14** | Retries/fallbacks/caching in place; guardrails + permissions documented |
| **Deploy + monitor** | **Week 15** | Live endpoint/UI with tracing and basic monitoring |
| **Final demo + design doc** | **Week 16** | Demo day, peer feedback, submitted repo + doc |

## 9. Grading scale & policies

**Grading scale**

| Grade | Range |
|---|---|
| A | 93–100 |
| A− | 90–92 |
| B+ | 87–89 |
| B | 83–86 |
| B− | 80–82 |
| C+ | 77–79 |
| C | 70–76 |
| D | 60–69 |
| F | < 60 |

**Attendance.** This is a hands-on, cohort course — showing up matters. Up to **2 absences** without penalty. Beyond that, each absence costs 2 points off the participation grade unless arranged in advance. Sessions are recorded, but recordings don't replace in-class lab help.

**Late work.** Labs and homework: **−10% per day, up to 3 days**, then no credit — except the "best 10 of 12 labs" rule gives everyone slack for two bad weeks. Capstone milestones are firm dates (they gate feedback), but talk to me *before* a deadline if life happens; I'd much rather adjust than penalize.

**Academic integrity.** This is an AI course — **using AI tools (including coding assistants and agents) is encouraged**, and often the point. The rule is simple: **you must understand and be able to explain everything you submit.** Cite substantial code you adapt from others. Collaboration on labs is fine (discuss, pair, review); copying another person's homework or capstone is not. Capstone work must be your team's own. If you use a heavy assist for something, note it — transparency is never penalized; passing off unexplained work as your own is.

**Accessibility & inclusion.** Tell me early about accommodations you need and I'll make them. Questions are welcome from anyone at any level; there are no dumb questions in a field this young.

## 10. Suggested resources

> **The field moves fast — treat every specific tool as temporary and every *pattern* as durable.** Anything named here may be renamed or superseded by the time you read it; the concept underneath won't be.

**Primary (always current):**
- Official docs for your model provider, LangChain/LangGraph, CrewAI, AutoGen, and MCP. Read release notes — this is where the field actually lives.
- Provider "building agents" / "agent design" guides and cookbooks.

**Foundational reading (patterns over tools):**
- The original **ReAct** paper (reasoning + acting) and **Reflexion** (self-reflection) — read these; they're the backbone of Weeks 3–4.
- **Toolformer** and function-calling write-ups for tool use.
- Writing on **RAG** and retrieval for grounding.
- The **MCP** specification and introductory posts.
- Vendor and community write-ups on **agent evaluation**, **tracing**, and **prompt injection / agent security** (e.g., OWASP's LLM risk lists).

**Habits:**
- Follow 2–3 practitioner blogs/newsletters rather than trying to read everything.
- Keep a personal "patterns notebook" — every time a lab teaches a reusable trick, write it down. That notebook is more valuable than any single framework.

---

# Weekly schedule

Difficulty rises steadily: **Weeks 1–4 intermediate foundations → 5–8 capable agents → 9–12 advanced architectures → 13–16 production & capstone.**

---

## Month 1 — Agent foundations

### Week 1 — What makes something an agent? Anatomy & setup

**Learning objectives**
- **Define** what distinguishes an agent from a plain LLM call, and **identify** the four components (model, tools, memory, control loop) in an example system.
- **Configure** a working Python + API environment and **run** a minimal model call safely (keys in `.env`).
- **Evaluate** whether a given problem warrants an agent versus a simpler solution.

**Topics**
- Agent vs. workflow vs. single prompt; the autonomy spectrum. The anatomy: model, tools, memory, loop. When *not* to build an agent. Course logistics and repo tour.

**Hands-on lab**
- Stand up the environment; run the smoke test. Then build a "proto-agent": a script that calls the model in a manual loop — model proposes an action in text, you (the human) execute it and paste the result back. Feel the loop before automating it.

**Homework**
- Write a one-page teardown of an existing agent product (real or from a demo): identify its model/tools/memory/loop, and argue whether an agent was the right call. Verify your `.env`/secrets setup with the provided checker.

**Buffer use this week**
- Extra time in the recap slot and wrap Q&A is reserved for **environment triage** — the classic Week 1 tax of keys, versions, and installs. No one leaves without a working setup.

---

### Week 2 — Tool / function calling: how agents act

**Learning objectives**
- **Implement** function/tool calling: define a tool, expose it to the model, parse the tool call, execute it, return the result.
- **Explain** the request/response cycle of tool use and **debug** a malformed tool call.
- **Build** a 2-tool agent that chooses between tools based on the user's request.

**Topics**
- The function-calling protocol; tool schemas (name, description, JSON parameters); how the model decides to call a tool; parsing arguments; feeding results back. Common failure modes (hallucinated args, wrong tool).

**Hands-on lab**
- Build a small agent with two tools (e.g., a calculator and a current-time/weather stub). Wire the full loop: user → model → tool call → execution → model → answer. Add logging so every tool call is visible.

**Homework**
- Add a third tool of your choice and a test showing the agent picks the right tool for three different prompts. Write down one case where it picked wrong and why.

**Buffer use this week**
- Break and wrap slots used for a group "gallery walk" of everyone's tool logs — comparing how different prompts changed tool selection.

---

### Week 3 — The agent loop: ReAct, built by hand

**Learning objectives**
- **Implement** a ReAct loop (reason → act → observe → repeat) from scratch, with no framework.
- **Design** a stopping condition and a max-iteration guard to prevent runaway loops.
- **Trace** a multi-step run and explain each reason/act/observe step.

**Topics**
- The ReAct pattern in depth; the scratchpad/thought trace; termination and iteration limits; state carried across steps; why "loop + tools + stop condition" is the heart of every agent framework you'll meet later.

**Hands-on lab**
- Hand-roll a ReAct agent that answers multi-step questions using your Week 2 tools plus a search/lookup tool. It must show its reasoning trace, loop until it has an answer, and stop safely at a max step count.

**Homework**
- Give your ReAct agent a task that requires **at least 3 tool calls** to solve. Capture the full trace and annotate where it reasoned well and where it wasted a step.

**Buffer use this week**
- Wrap Q&A dedicated to debugging infinite loops and runaway costs — a rite of passage. We set a hard iteration cap together.

---

### Week 4 — Reasoning patterns + mini-project *(checkpoint week)*

**Learning objectives**
- **Implement** reflection (self-critique-then-revise) and **self-consistency** (sample-and-vote) on top of a base agent.
- **Evaluate** whether each pattern actually improves results on a small task, with before/after evidence.
- **Deliver** a self-contained mini-project agent that combines the loop with a reasoning pattern.

**Topics**
- Reflection / self-refinement; self-consistency and majority voting; when extra reasoning helps vs. just costs more. Lighter new content — this is a consolidation + build week.

**Hands-on lab**
- Take your Week 3 ReAct agent and add a reflection step; measure the difference on 3–5 test cases. Start the mini-project.

**Mini-project (graded, 10%)**
- Build a small agent that uses the loop **plus** at least one reasoning pattern to solve a task you choose (e.g., a math word-problem solver, a fact-checker, a small research helper). Include a short note on whether the reasoning pattern helped and your evidence. **Due end of Week 5.**

**Buffer use this week**
- This is a **checkpoint week**: reduced new material and protected in-class time to catch up on Weeks 1–3, get unblocked, and start the mini-project with the instructor in the room.

---

## Month 2 — Building capable agents

### Week 5 — Designing agent tools: schemas, error handling, real APIs

**Learning objectives**
- **Design** production-quality tool schemas with clear descriptions, typed parameters (Pydantic), and validation.
- **Implement** robust error handling so a failing tool returns a useful message the agent can recover from — not a crash.
- **Integrate** a real third-party API as a tool, including auth and rate-limit handling.

**Topics**
- What makes a tool "legible" to a model (naming, descriptions, param design); input validation; structured error results; idempotency and side effects; wrapping real APIs; secrets and rate limits.

**Hands-on lab**
- Replace a stub tool with a **real API** (e.g., a public weather, search, or GitHub API). Add Pydantic validation and graceful error returns. Deliberately break it (bad input, network error) and make the agent recover.

**Homework**
- Add a second real-API tool and write 3 tests: happy path, invalid input, and API failure. The agent must degrade gracefully in the last two.

**Buffer use this week**
- Break used for a short clinic on reading API docs and handling auth — the part that trips everyone up.

---

### Week 6 — Agent memory & retrieval (working vs. long-term, RAG grounding)

**Learning objectives**
- **Distinguish** working memory (context window) from long-term memory (persisted) and **implement** both.
- **Build** a retrieval-augmented tool that grounds the agent in an external knowledge source.
- **Evaluate** how memory/retrieval changes agent behavior on repeated and knowledge-dependent tasks.

**Topics**
- Conversation/working memory and summarization; long-term memory stores; embeddings + vector search as a *tool* the agent calls; RAG for grounding and reducing hallucination; when memory helps vs. bloats context.

**Hands-on lab**
- Add long-term memory (persisted across runs) and a **retrieval tool** over a small document set to one of your agents. Show it correctly answering a question that requires the retrieved docs — and admitting when the answer isn't there.

**Homework**
- Point the retrieval tool at a corpus you care about (docs, notes, a wiki). Produce 3 grounded Q&A examples with citations to the source chunks.

**Buffer use this week**
- Wrap Q&A covers embeddings gotchas (chunking, stale indexes) and a preview of how this feeds the capstone.

---

### Week 7 — Agent frameworks hands-on (LangGraph; CrewAI & AutoGen)

**Learning objectives**
- **Rebuild** a previous hand-rolled agent in **LangGraph**, mapping your by-hand concepts onto the framework's abstractions.
- **Compare** LangGraph, CrewAI, and AutoGen and **justify** which fits a given problem.
- **Implement** a simple graph with nodes, edges, and state.

**Topics**
- Why frameworks exist (state, persistence, streaming, orchestration); LangGraph's graph/state model; CrewAI's role/crew model; AutoGen's conversational-agents model; trade-offs and lock-in; "you already understand the internals, so the API is easy."

**Hands-on lab**
- Port your Week 3–5 agent to LangGraph. Then re-express the *same* task minimally in CrewAI **or** AutoGen and write a short comparison: what each made easy vs. awkward.

**Homework**
- Finish the port; extend the LangGraph version with one conditional edge (branching behavior). Note two things the framework gave you for free that you'd hand-coded before.

**Buffer use this week**
- Recap slot maps "hand-rolled concept → framework term" as a class, so the frameworks feel like relabeling, not new magic.

---

### Week 8 — Agent planning & task decomposition + capstone proposal *(checkpoint week)*

**Learning objectives**
- **Implement** a planner that decomposes a complex goal into ordered subtasks the agent executes.
- **Compare** plan-then-execute vs. interleaved (ReAct-style) planning and choose appropriately.
- **Deliver** a capstone proposal with a clear problem, users, and success criteria.

**Topics**
- Task decomposition; plan-and-execute vs. ReAct; replanning when a step fails; subgoal tracking. Lighter new content — a planning + proposal week.

**Hands-on lab**
- Add a planning step to your LangGraph agent: it drafts a plan, executes step by step, and can replan on failure. Test it on a task with 4+ subtasks.

**Capstone proposal (graded checkpoint)**
- Submit the 1-page proposal: problem, target users, planned tools, memory/retrieval approach, and measurable success criteria. **Due end of Week 8.**

**Buffer use this week**
- **Checkpoint week:** protected time for 1:1 proposal feedback and to catch up on the framework transition. Deliberately light on new material.

---

## Month 3 — Advanced agent architectures

### Week 9 — Multi-agent systems: roles, collaboration, handoffs

**Learning objectives**
- **Build** a multi-agent system where specialized agents collaborate on a task.
- **Implement** a clean handoff protocol (how one agent passes control and context to another).
- **Evaluate** when multiple agents beat a single well-designed agent — and when they just add cost and failure modes.

**Topics**
- Role specialization (researcher/writer/critic); communication patterns; shared vs. private state; handoffs and message passing; the real costs of multi-agent (latency, error propagation, coordination bugs).

**Hands-on lab**
- Build a 2–3 agent "content pipeline" (e.g., researcher → writer → editor) in LangGraph or CrewAI, with explicit handoffs. Compare its output and cost against a single-agent baseline.

**Homework**
- Add a critic agent that can send work back for revision. Show one run where the critic caught and fixed a real problem.

**Buffer use this week**
- Wrap Q&A on multi-agent debugging — tracing *which* agent went wrong is a new skill; we practice reading multi-agent logs.

---

### Week 10 — Orchestration: graphs, supervisor/worker, human-in-the-loop

**Learning objectives**
- **Implement** a supervisor/worker orchestration where a supervisor routes tasks to workers.
- **Build** a human-in-the-loop checkpoint that pauses the graph for approval before a consequential action.
- **Design** an orchestration graph diagram for your capstone.

**Topics**
- Supervisor/router patterns; conditional routing; parallel workers; interrupts, checkpoints, and resuming; human approval gates for irreversible/sensitive actions; state persistence across a pause.

**Hands-on lab**
- Build a supervisor agent that dispatches to 2+ workers and add a **human-in-the-loop interrupt**: the graph pauses and waits for your approval before executing a "sensitive" tool, then resumes.

**Homework / capstone milestone**
- Submit your **capstone architecture sketch** (agents/graph, tools, data flow, handoffs, human gates). Wire at least one human-approval checkpoint into a prototype.

**Buffer use this week**
- Break used to workshop architecture diagrams in pairs before they become the capstone milestone.

---

### Week 11 — Agents in the real world: context engineering & MCP

**Learning objectives**
- **Apply** context-engineering techniques (what to put in context, when, and how to compress it) to improve agent reliability and cost.
- **Integrate** an external system via the **Model Context Protocol (MCP)**.
- **Evaluate** the impact of context choices on a real task.

**Topics**
- Context engineering: selection, ordering, compression/summarization, and hygiene; context windows as a scarce resource; MCP — what it is, why standardized tool/context integration matters, connecting an MCP server; the shift from bespoke tools to reusable connectors.

**Hands-on lab**
- Connect your agent to an MCP server (a provided or public one) and use its tools/resources. Then run a context-engineering pass on one agent: measure tokens/cost before and after compression and summarization.

**Homework**
- Integrate one more MCP capability *or* apply context engineering to your capstone, and report the before/after cost and quality difference.

**Buffer use this week**
- Recap slot connects context engineering back to Week 6 memory — they're two sides of "managing what the model sees."

---

### Week 12 — Agent evaluation, tracing & debugging + mid-capstone review *(checkpoint week)*

**Learning objectives**
- **Build** an evaluation harness: a labeled test set plus metrics for an agent task.
- **Instrument** an agent with tracing and **debug** a failure by reading its trace.
- **Evaluate** your own capstone against its stated success criteria so far.

**Topics**
- Why "it worked in the demo" isn't evaluation; offline eval sets; metrics (task success, tool-call correctness, groundedness, cost); LLM-as-judge and its pitfalls; tracing/observability tools; systematic debugging of agent failures. Lighter new content — consolidation + review.

**Hands-on lab**
- Add tracing to an agent and build a small eval suite (5–10 labeled cases) that scores it automatically. Use a trace to diagnose one real failure and fix it.

**Mid-capstone review (graded checkpoint)**
- Present a 5-minute status: working core loop, one real tool, and your first eval cases. Give and receive peer feedback. **Due Week 12.**

**Buffer use this week**
- **Checkpoint week:** the bulk of class is structured mid-capstone reviews and unblocking. Evaluation is taught precisely now because you'll apply it to your own project immediately.

---

## Month 4 — Production agents & capstone

### Week 13 — Reliability: retries, caching, fallbacks, cost & latency

**Learning objectives**
- **Implement** retries with backoff, response caching, and model/tool fallbacks in an agent.
- **Measure and budget** cost and latency for an agent run, and **optimize** one bottleneck.
- **Evaluate** reliability improvements with before/after numbers.

**Topics**
- Transient vs. permanent failures; retry/backoff; idempotency; caching (prompt/response/tool-result); fallback chains (cheaper/smaller model, degraded mode); measuring and controlling cost and latency; the reliability/cost/quality triangle.

**Hands-on lab**
- Harden an agent: add retries with backoff, cache an expensive tool or model call, and add a fallback model. Produce a table of cost and p50/p95 latency before and after.

**Homework / capstone milestone**
- Apply reliability features to your capstone (**reliability pass**, part of the Week 14 milestone) and record the cost/latency impact.

**Buffer use this week**
- Wrap Q&A on cost surprises — everyone shares their biggest bill and how they'd cut it.

---

### Week 14 — Safety & security: prompt injection, guardrails, permissions

**Learning objectives**
- **Demonstrate** a prompt-injection attack against a tool-using agent and **implement** a mitigation.
- **Build** input/output guardrails and a **least-privilege** permission model for agent tools.
- **Assess** an agent for common security risks and document the residual risk.

**Topics**
- Prompt injection (direct and indirect, e.g., via retrieved/tool content); the confused-deputy problem; guardrails (input filtering, output validation, allow-lists); least-privilege tool permissions and human approval for dangerous actions; data exfiltration risks; the OWASP LLM risk categories as a checklist.

**Hands-on lab**
- Attack your own agent: craft an indirect prompt injection hidden in a document/tool result that makes it misbehave. Then defend it — add a guardrail and tighten tool permissions so the attack fails. Show both the successful attack and the successful defense.

**Homework / capstone milestone**
- Complete the **safety pass** on your capstone: at least one guardrail and a documented least-privilege permission story. **Reliability + safety milestone due Week 14.**

**Buffer use this week**
- Break used for a live "red team the instructor's agent" exercise — attacks are more fun (and stickier) as a group sport.

---

### Week 15 — Deploying & operating agents (serving, monitoring, LLMOps basics)

**Learning objectives**
- **Deploy** an agent as a service (FastAPI endpoint, containerized) reachable over HTTP.
- **Implement** production monitoring: logging, tracing, and basic metrics/alerts for an agent.
- **Explain** the core LLMOps loop (observe → evaluate → improve) for a live agent.

**Topics**
- Wrapping an agent in an API; statelessness vs. session state; streaming responses; containerizing with Docker; environment/secrets in production; monitoring cost, latency, errors, and quality in the wild; feedback capture and the LLMOps improvement loop.

**Hands-on lab**
- Wrap your agent in a FastAPI endpoint, containerize it, and deploy it somewhere reachable (local container or a simple host). Add request logging + tracing and a minimal metrics view.

**Homework / capstone milestone**
- **Deploy your capstone** with tracing and basic monitoring. It must be reachable and instrumented for demo day. **Deploy + monitor milestone due Week 15.**

**Buffer use this week**
- Wrap Q&A is a deployment troubleshooting clinic — because something always breaks in prod. We also confirm every capstone is reachable before demo day.

---

### Week 16 — Capstone demos, feedback & what's next *(checkpoint / wrap week)*

**Learning objectives**
- **Present** and **defend** your capstone: architecture, evaluation results, reliability/safety choices, and cost.
- **Evaluate** peers' agents with structured, constructive feedback.
- **Plan** your continued learning path in a fast-moving field.

**Topics**
- Demo day; how to present an agent (show the eval numbers, not just a happy-path demo); reflecting on trade-offs made; where the field is heading and how to keep up (patterns over tools); building a portfolio.

**Hands-on "lab" (demo day)**
- Each learner/team gives a 6–8 minute live demo + defense. The rest of the class runs structured peer review against a shared rubric (does it work, is it evaluated, is it reliable, is it safe, is it deployed).

**Final deliverable**
- **Capstone submission:** repo + design doc + live endpoint/UI + eval results + demo. Submitted end of Week 16.

**Buffer use this week**
- **Wrap week:** deliberately light on new content. Time is protected for demos, peer feedback, a course retrospective, and a "what to learn next" roadmap so momentum outlasts the course.

---

## Appendix: outcome → assessment map

| Course outcome | Primarily assessed by |
|---|---|
| 1. Explain agent anatomy & when to use | Week 1 lab/HW, participation |
| 2. Hand-implement ReAct/reflection | Weeks 3–4 labs, mini-project |
| 3. Robust tool design | Week 5 lab/HW, capstone |
| 4. Memory + retrieval grounding | Week 6 lab/HW, capstone |
| 5. Multi-agent + orchestration + HITL | Weeks 9–10 labs, capstone |
| 6. Context engineering + MCP | Week 11 lab/HW, capstone |
| 7. Evaluate, trace, debug | Week 12 lab, capstone eval suite |
| 8. Reliability & cost | Week 13 lab, capstone milestone |
| 9. Safety & security | Week 14 lab, capstone milestone |
| 10. Deploy & operate | Week 15 lab, capstone deployment |
| 11. Deliver & defend end-to-end | Capstone + Week 16 demo |

*This syllabus is a living document; specific tools may be swapped as the field evolves, but the learning outcomes and pattern-first approach will not.*
