# The AI Product Decision Checklist

> A good AI product decision isn't "can we build it" — it's five dimensions evaluated in the right order, where a single weak one can override a strong score everywhere else.

*Scenario: Northbeam, a fictional mid-market B2B SaaS company. Illustrative, not based on any employer's data.*

## 1. Problem

Northbeam's Customer Success leadership wants a GenAI tool that drafts outreach emails to at-risk enterprise accounts, using account history and support tickets. The pitch is strong on one dimension — CS reps spend hours a week manually drafting these — and that's where the conversation usually stops. No one has checked whether the underlying data actually supports it, or what happens the first time it gets something wrong.

## 2. Product decision

A five-part checklist, applied in sequence, each with a concrete gate — not a vague rating:

| Dimension | What it checks | Applied to the churn-risk drafter |
|---|---|---|
| Value | Is the pain real, frequent, and strategically worth solving — regardless of how it's solved | CS reps spend ~5+ hrs/week on this; real, recurring pain |
| Feasibility | Can the model do this reliably, with the team and timeline available | An LLM can draft a coherent, on-brand email from structured inputs — low technical risk |
| Data readiness | Is the data clean, current, and permissioned *today* | Account history is solid; support-ticket tagging is inconsistent — a real gap |
| Risk | How bad is a wrong output, and who's exposed | A bad draft reaching an enterprise customer can damage a renewal relationship |
| Reversibility | How easily can a bad output be caught before harm | Fully reversible, if the draft is never auto-sent — a human must review and send it |

**The rule that matters:** a failing data-readiness check, or a high-risk/low-reversibility combination, overrides a strong value score. It doesn't just average it down. For Northbeam, that means the ticket-data gap caps the launch scope even though value and feasibility both look good — and the human-review gate is what makes the risk acceptable at all.

## 3. How the system works

Walking the churn-risk email drafter through all five, in order:

1. **Value** — confirmed first, cheaply (a few interviews with CS reps), before any build work starts.
2. **Feasibility** — a quick technical spike: can the model produce a usable draft from the account + ticket data available. Yes.
3. **Data readiness** — this is where it gets real. Account history (renewal dates, contract terms) is structured and reliable. Support-ticket data is not: half-filled fields, inconsistent tagging, no clean signal for "this ticket indicates churn risk." That gap caps what the feature can responsibly cover at launch.
4. **Risk** — a wrong or tone-deaf email to an enterprise account carries real relationship cost, not just an awkward sentence.
5. **Reversibility** — the design choice that makes this shippable: the model drafts, a human reviews and sends. Nothing reaches the customer unreviewed. This is what turns an otherwise medium-risk feature into a low-risk one, without touching the model at all.

The checklist's output isn't a single score — it's a scoped decision: launch on the subset of accounts where ticket data is clean enough to trust, with mandatory human review, and treat broader ticket-data cleanup as a separate, justified investment.

## 4. Risks and trade-offs

- **Scoring one dimension while ignoring another** — a strong value case can carry a weak data-readiness problem past review if the checklist isn't applied as a gate, not an average.
- **The review gate that makes this safe also caps the ROI** — every draft still costs CS time to review, so this ships as "assisted drafting," not full automation. That's a real, deliberate trade-off between a lower-risk, lower-ceiling design and a higher-risk, higher-upside one — not a limitation to quietly work around.
- **A checklist can become theater** — if "data readiness: weak" never actually blocks a launch in practice, the framework is decorative. The gate has to be real, including the times it says no.

## 5. How I would measure success

- **Time saved per CS rep per week** — the core value hypothesis, measured directly.
- **% of drafts sent with no or minor edits** — a proxy for whether reps actually trust the output, not just tolerate it.
- **No increase in at-risk-account churn post-launch** — guardrail metric; the tool should never make outcomes worse even while saving time.
- **Qualitative CS feedback on trust** — whether reps would flag a wrong-feeling draft before sending, which tells you if the human-review gate is actually doing its job or just a rubber stamp.

---
**Related:** `01-ai-product-foundations/when-not-to-use-genai.md`
