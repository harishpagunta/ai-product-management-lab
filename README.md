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
| `04-agentic-workflows/` | Workflow vs agent, tool use, recovery, human approval |
| `05-ai-evaluation/` | Output evaluation frameworks and scorecards |
| `06-responsible-ai/` | Enterprise guardrails and human oversight |
| `07-ai-product-metrics/` | Metrics trees, quality vs cost vs latency |
| `projects/enterprise-knowledge-assistant/` | End-to-end case: grounded answers, approved actions, human approval |

### Published

Artifacts are listed here as they are published.

- [`01-ai-product-foundations/choosing-the-right-intelligence-layer.md`](01-ai-product-foundations/choosing-the-right-intelligence-layer.md) — a decision framework for picking rules engines, classic ML, or GenAI for a given job, illustrated through a support-ticket routing scenario, grounded in the token/context-window/temperature mechanics that explain why GenAI behaves the way it does.
- [`01-ai-product-foundations/when-not-to-use-genai.md`](01-ai-product-foundations/when-not-to-use-genai.md) — a checklist for when GenAI is technically possible but still the wrong choice, illustrated through a policy-lookup scenario contrasting search/retrieval with a GenAI chat layer.
