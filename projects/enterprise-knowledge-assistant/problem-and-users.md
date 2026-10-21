# Enterprise Knowledge Assistant: Problem & Users

> The hardest decisions in an enterprise AI assistant — what it can see, who it serves, what it's allowed to do — get made here, before any model or retrieval choice exists.

*Scenario: Ridgeline Software, a mid-market B2B SaaS company selling workflow and operations-automation tools to enterprise manufacturing and logistics teams. Illustrative, not based on any employer's data.*

This is the first artifact in a multi-week capstone: an internal knowledge assistant for Ridgeline's customer-facing teams. Later artifacts (RAG design, agent/tool design, evaluation, guardrails) build directly on the scope defined here — this doc isn't preamble, it's the constraint set for everything after it.

## 1. Problem

Ridgeline's Support and Customer Success teams answer a steady stream of questions that all require the same thing: pulling together scattered internal information fast enough to respond before a ticket escalates or a renewal conversation goes cold.

- **Support reps** (L1/L2) resolve tickets about product behavior, configuration, and policy (SLAs, refund/credit rules, data-retention terms) by searching a help center, a separate internal wiki, and old resolved tickets — three places, no single source of truth, and no guarantee any of them is current.
- **Customer Success Managers (CSMs)** need a fast read on an account's health before a renewal call or a risk flag — but that picture is scattered across the ticket history, release notes that affected the account, and CSM-only notes, with no single place that assembles it.

The cost isn't abstract: inconsistent answers across reps (two reps give two different refund policies to two customers), slower resolution (hunting across tools takes longer than solving the actual problem), and avoidable escalations when a rep can't find the authoritative answer in time.

## 2. Product decision

Scope V1 narrowly, by role and by job, rather than building a general-purpose "ask anything" assistant:

| In scope for V1 | Deliberately out of scope for V1 |
|---|---|
| Support reps, CSMs | Sales (AEs/SEs), Legal, Finance |
| "Give me a cited, trustworthy answer to a product/policy question" | Pricing, contract terms, deal-specific edge cases |
| "Summarize this account's recent history and recommend a next step" | Taking any action automatically (sending, updating, approving) |
| Product docs, help-center/policy docs, resolved-ticket history | Contracts, billing/finance systems, internal eng chat |

**Why this wedge and not Sales:** Support/CS questions are high-frequency and comparatively low-risk — mostly factual, product/policy questions with a right answer. Sales questions (can we discount this, what did we promise this account contractually) are lower-frequency but much higher-risk if the assistant is confidently wrong, and touch data (contracts, pricing) that needs real permissioning work before any AI system should see it. Proving the assistant works on the safer, faster-feedback-loop wedge first is the point — Sales is a V2 candidate once trust and the permissioning model exist, not a V1 feature left out for convenience.

**Why these knowledge sources and not others:** product docs and policy docs are structured, owned, and relatively stable — a reasonable starting point for groundedness. Ticket history is messier but valuable as precedent ("how was this handled last time"). Contracts and finance data are excluded not because they're low-value, but because they require per-account permissioning the V1 system doesn't have yet — including them now would mean building access control as an afterthought, which is how enterprise AI assistants leak data they shouldn't.

## 3. How the system works

At this stage, "how it works" is a scope map, not an architecture — retrieval, ranking, and tool design come in later capstone days. The map:

```
Support rep ──┐
              ├─▶ Job: "cited answer to a product/policy question"
CSM ──────────┤
              └─▶ Job: "account summary + recommended next step"
                        │
                        ▼
              Knowledge sources (V1 scope):
              product docs · policy docs · resolved tickets
                        │
                        ▼
              Assistant responds with: answer + citation,
              or summary + one recommended action
                        │
                        ▼
              Human decides and acts (no autonomous actions in V1)
```

Two things this map is deliberately silent on, on purpose: *how* retrieval finds the right document (Week 2) and *whether* the assistant should ever be allowed to take an action itself rather than just recommend one (Week 3). Locking those in now would be designing ahead of the evidence.

## 4. Risks and trade-offs

- **Narrow scope caps early visible value.** Sales is the team most vocal about wanting this, and they're excluded from V1. The trade-off is deliberate: a smaller, trustworthy wedge beats a broad launch that mishandles one high-risk case and poisons trust in the whole assistant.
- **Excluding ticket-adjacent sources (eng chat, internal notes) loses some context.** Those sources are high-value but low-structure and hard to verify as current — including them raises the hallucination/staleness risk more than it raises answer quality, at least for V1.
- **"Recommend, don't act" limits ROI.** A CSM still has to read the recommendation and act on it manually. That's intentional — it keeps V1 fully reversible (nothing happens without a human) — but it also means the time savings are real but partial until agentic/action capability is deliberately added later, with its own approval design.
- **Role-based scoping can ossify.** If Support/CS prove it out and Sales is added later, the knowledge sources and permission model need to be extended, not just reused — a "one-size assistant" temptation is exactly how scope creep reintroduces the risks this doc exists to avoid.

## 5. How I would measure success

This is a scoping artifact, not a shipped feature, so success here isn't an adoption metric — it's whether the scope holds up as later capstone days build on it. Concretely:

- **For this doc:** can Week 2 (RAG design) and Week 3 (agent/tool design) be built directly against this problem/user/JTBD definition without re-scoping it? If a later day needs to redefine who the user is or what data is in scope, this artifact under-specified something.
- **For the eventual V1 (forward-looking, to be refined once evaluation design lands):** ticket resolution time for in-scope question types; % of assistant answers accepted/used without escalation; no increase in incorrect-answer-driven escalations (guardrail); CSM-reported trust in the account summary (qualitative, early).

---
**Related:** [`01-ai-product-foundations/ai-product-decision-checklist.md`](../../01-ai-product-foundations/ai-product-decision-checklist.md) — the value/feasibility/data-readiness/risk/reversibility checklist this scoping decision implicitly applies.
