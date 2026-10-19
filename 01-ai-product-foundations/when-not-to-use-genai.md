# When Not to Use GenAI

> Most "should we use GenAI" conversations skip the more useful question — when GenAI technically works but is still the wrong choice. That judgment call is where a PM earns their seat at the table.

*Scenario: Northbeam, a fictional mid-market B2B SaaS company. Illustrative, not based on any employer's data.*

## 1. Problem

Northbeam leadership issues an "add AI everywhere" mandate. One target: an internal policy-lookup tool — support agents asking things like "what's our refund policy for enterprise customers past the 30-day window?" The proposal on the table is a GenAI chatbot. But the actual need is surfacing the exact, current policy text, not a generated paraphrase of it. A chatbot would technically work here — it just isn't the right layer for a question that already has one correct, literal answer sitting in a document.

## 2. Product decision

A "don't default to GenAI when" checklist, applied before any build decision:

| Signal | Don't default to GenAI — use instead |
|---|---|
| An exact, reproducible answer already exists in a document | Search / retrieval — return the source passage, not a rewritten one |
| Being wrong is high-stakes and hard to reverse | Deterministic rules, or a mandatory human sign-off |
| The decision must be auditable after the fact | Rules or classic ML — both can be inspected; GenAI's reasoning can't be, reliably |
| The problem is closed-set with labeled history | Classic ML |
| Cost/latency at scale outweighs the value generation adds | Whatever near-zero-marginal-cost layer does the job |

For the policy-lookup case: none of the "GenAI earns its place" conditions apply — there's no open-ended synthesis needed, no judgment call, just an exact lookup. That alone settles it.

## 3. How the system works

Two designs over the same policy documents, contrasted:

- **(a) Search / retrieval:** agent query → index lookup → return the exact source passage, verbatim. No generation step. The answer today is byte-identical to the answer tomorrow, as long as the underlying policy hasn't changed.
- **(b) GenAI chat layer:** agent query → retrieve relevant passage(s) → an LLM generates a new answer *in its own words*. Even when retrieval was correct, the generation step introduces paraphrase drift — the model can restate a number, a date, or a condition slightly wrong while sounding completely confident.

The failure mode isn't retrieval — it's that generation gets added on top of a correct retrieval, for a task that never needed generation in the first place.

## 4. Risks and trade-offs

- **Generated policy language can drift from the literal, current policy** — and for refund/compliance-adjacent answers, that drift creates real liability, not just an awkward sentence.
- **The deterministic option can feel like more friction** ("why can't it just answer me in plain language") — a genuine product cost, not only a technical one. Search returns a passage; it doesn't have a conversational tone. That trade-off needs to be owned, not hidden.
- **Search/indexing has its own failure modes** — stale documents, poor query matching, policies that are genuinely ambiguous and would benefit from a human (not a model) interpreting them. "Don't use GenAI" isn't "don't invest in the alternative."

## 5. How I would measure success

- **% of policy questions resolved by the exact source passage, without escalation to a human** — the core signal that search-as-answer is sufficient.
- **Zero tolerance for liability-relevant drift**, if GenAI is ever layered on top as a summarization convenience later.
- **Cost per resolved query**, compared across the deterministic and GenAI paths — making the economics of "we didn't need GenAI here" visible, not just a hunch.

---
**Related:** `01-ai-product-foundations/choosing-the-right-intelligence-layer.md`
