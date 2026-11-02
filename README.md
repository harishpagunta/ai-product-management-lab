# AI Product Management Lab

A collection of product thinking, system design, evaluation frameworks, and hands-on experiments focused on building useful, measurable, and responsible AI products.

## Focus Areas

- AI Product Strategy
- LLM-Based Product Experiences
- RAG and Knowledge Retrieval
- Agentic Workflows
- AI Evaluation
- Human-in-the-Loop Systems
- Responsible AI and Guardrails
- AI System Design
- Product Metrics and Experimentation
- AI Product Economics

## Product Principles

The work in this repository is guided by a few principles:

- Start with the user problem, not the model
- Use AI only where it creates meaningful product value
- Design for uncertainty, failure, and human oversight
- Evaluate outputs systematically
- Balance quality, latency, cost, and trust
- Make product decisions explainable and measurable

## Work

Every artifact answers five questions: **Problem → Product decision → How the system works → Risks and trade-offs → How I would measure success.** Scenarios use fictional companies and are illustrative, not based on any employer's data.

| Area | Focus |
| --- | --- |
| `01-ai-product-foundations/` | Choosing the right intelligence layer, and when not to use GenAI |
| `02-prompt-context-design/` | Prompt vs context, contextual AI experiences |
| `03-rag-knowledge-retrieval/` | RAG design, failure modes, retrieval quality |
| `04-agentic-workflows/` | Workflow vs agent, tool use, recovery, human approval — learning area, not claimed as production work |
| `05-ai-evaluation/` | Output evaluation frameworks and scorecards |
| `06-responsible-ai/` | Enterprise guardrails and human oversight |
| `07-ai-product-metrics/` | Metrics trees, quality vs cost vs latency |
| `projects/enterprise-knowledge-assistant/` | End-to-end case: grounded answers, approved actions, human approval |

### Published

Artifacts are listed here as they are published.

- [`01-ai-product-foundations/choosing-the-right-intelligence-layer.md`](01-ai-product-foundations/choosing-the-right-intelligence-layer.md) — a decision framework for picking rules engines, classic ML, or GenAI for a given job, illustrated through a support-ticket routing scenario, grounded in the token/context-window/temperature mechanics that explain why GenAI behaves the way it does.
- [`01-ai-product-foundations/when-not-to-use-genai.md`](01-ai-product-foundations/when-not-to-use-genai.md) — a checklist for when GenAI is technically possible but still the wrong choice, illustrated through a policy-lookup scenario contrasting search/retrieval with a GenAI chat layer.
- [`01-ai-product-foundations/ai-product-decision-checklist.md`](01-ai-product-foundations/ai-product-decision-checklist.md) — a five-part checklist (value, feasibility, data readiness, risk, reversibility) for deciding whether to build an AI feature at all, illustrated through a churn-risk outreach drafter scenario.
- [`projects/enterprise-knowledge-assistant/problem-and-users.md`](projects/enterprise-knowledge-assistant/problem-and-users.md) — problem, users, and jobs-to-be-done for a multi-week enterprise knowledge assistant capstone: scoping V1 to a support/CS wedge and explicitly excluding contracts and finance data until permissioning catches up.
- [`projects/enterprise-knowledge-assistant/rag-strategy.md`](projects/enterprise-knowledge-assistant/rag-strategy.md) — applying the RAG design, permission, and quality-monitoring lessons from earlier artifacts to the capstone's two V1 jobs, drawing a role-based permission line around CSM-only notes that the scoping doc had left open.
- [`projects/enterprise-knowledge-assistant/agent-tool-design.md`](projects/enterprise-knowledge-assistant/agent-tool-design.md) — applying the workflow-vs-agent test to the capstone's two V1 jobs, finding both are workflows (not agents) once "no autonomous actions in V1" is taken seriously, with the simpler failure-recovery and approval design that follows from read-only tools.
- [`02-prompt-context-design/prompt-vs-context.md`](02-prompt-context-design/prompt-vs-context.md) — a diagnostic for separating prompt problems, context problems, and genuine model-capability limits, illustrated through a support-ticket-drafting assistant that needed structured output instead of free text.
- [`02-prompt-context-design/contextual-ai-experience-design.md`](02-prompt-context-design/contextual-ai-experience-design.md) — why surface placement, citations, and calibrated trust cues decide whether an AI feature is actually trusted and used, illustrated by adding inline placement, sourcing, and a confidence-gated UI to the support-ticket-drafting assistant.
- [`03-rag-knowledge-retrieval/rag-for-product-managers.md`](03-rag-knowledge-retrieval/rag-for-product-managers.md) — why most "bad AI answer" bugs are retrieval bugs, illustrated by replacing manual excerpt paste-in with hybrid search and reranking in the support-ticket-drafting assistant's policy lookups.
- [`03-rag-knowledge-retrieval/rag-system-design.md`](03-rag-knowledge-retrieval/rag-system-design.md) — why ingestion, freshness, and permissions are separate failure modes from retrieval quality, illustrated by adding permission-aware retrieval and incremental re-indexing as the support-ticket-drafting assistant's policy library expands into HR- and Legal-adjacent documents.
- [`03-rag-knowledge-retrieval/rag-failure-modes.md`](03-rag-knowledge-retrieval/rag-failure-modes.md) — a six-category failure-mode taxonomy for diagnosing "bad AI answer" reports (retrieval miss, ranking failure, freshness failure, permission failure, grounded-but-wrong generation, context overload), with a triage order for the support-ticket-drafting assistant.
- [`03-rag-knowledge-retrieval/retrieval-quality-scorecard.md`](03-rag-knowledge-retrieval/retrieval-quality-scorecard.md) — a lightweight weekly scorecard (precision, proxied recall, groundedness) for catching RAG regressions before rep complaints surface them, routed into the failure-mode taxonomy when a metric dips.
- [`04-agentic-workflows/workflow-vs-agent.md`](04-agentic-workflows/workflow-vs-agent.md) — a decision framework for choosing between normal software, a fixed workflow, and a true agent, illustrated by a routine-credit-eligibility feature that looks agent-like but is better served by a fixed workflow with LLM-powered steps.
- [`04-agentic-workflows/agent-tool-use-system-design.md`](04-agentic-workflows/agent-tool-use-system-design.md) — a system design for agent tool calling, memory, state, and bounded planning, illustrated by a ticket-triage agent with a narrow tool surface, session-scoped memory, an explicit audit log, and a capped tool-call budget that defaults to escalation.
- [`04-agentic-workflows/agent-failure-and-recovery.md`](04-agentic-workflows/agent-failure-and-recovery.md) — a per-tool retry policy, per-call credential scoping, and a compensation-or-flag rule for partial failures, illustrated by the ticket-triage agent handling a timed-out tool call and a partially-completed multi-step action.
- [`04-agentic-workflows/human-approval-patterns.md`](04-agentic-workflows/human-approval-patterns.md) — why "human in the loop" is really four different patterns (approve, edit, escalate, audit) with different costs and risks, mapped to the ticket-triage agent's action types by cost, reversibility, and frequency.
