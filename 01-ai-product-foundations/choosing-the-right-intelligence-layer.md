# Choosing the Right Intelligence Layer

> Most enterprise AI features fail not because the model was wrong, but because the team picked the wrong intelligence layer — rules, classic ML, or GenAI — for the job.

*Scenario: Northbeam, a fictional mid-market B2B SaaS company. Illustrative, not based on any employer's data.*

## 1. Problem

Northbeam's support team is drowning in inbound tickets, and "let's use AI" has become the default pitch for almost any feature involving text or judgment — routing tickets, drafting replies, flagging churn risk. The problem is that "AI" is being used as a single word for three very different kinds of systems, each with different cost, risk, and reliability profiles. Teams skip the step of asking *which* layer actually fits the job, and default to the most generative (and most expensive, least predictable) option because it's the most visible one right now.

This matters because the wrong layer shows up later as a production problem: a GenAI feature deployed where a deterministic rule would have been cheaper and more auditable, or a rules engine stretched past its breaking point where a classifier was needed. Both cost the company credibility with users and with leadership.

The pattern is familiar to anyone who's sat in these rooms: a stakeholder asks for a "smart chatbot" when the actual user problem would be better served by a simple classifier or recommendation step. The request is really "make this feel smarter and more automated" — and untangling what *kind* of smart is the PM's job, not the engineer's afterthought.

## 2. Product decision

The decision isn't "should we use AI" — it's "which layer of intelligence does this specific job need." A simple framework, scored against the factors that actually change engineering cost and user-facing risk:

| Factor | Rules engine | Classic ML | GenAI / LLM |
|---|---|---|---|
| Needs exact, auditable determinism (e.g., billing, compliance routing) | Best fit | Partial — needs guardrails | Poor fit without heavy constraints |
| Labeled historical data available | Not required | Required, and must be representative | Not required to start |
| Tolerance for occasional wrong output | Low | Low–medium | Medium–high, if reviewed |
| Task is open-ended / generative (drafting, summarizing, explaining) | Can't do it | Can't do it | Best fit |
| Cost and latency sensitivity | Near-zero marginal cost | Low, after training | Per-call cost and latency, compounding at scale |
| Maintainability as rules/edge-cases grow | Degrades fast past a few dozen rules | Stable if retrained | Stable, but needs evaluation infrastructure |

For Northbeam's ticket routing specifically: a rules engine handles the top 5 "obvious" categories cheaply and transparently; a classifier is the right fit once there's a year of labeled ticket data and the categories are closed-set; GenAI earns its place only at the edges — drafting a reply, summarizing a long thread, or handling genuinely open-ended requests a closed taxonomy can't capture.

**Rejected approach:** routing every ticket through an LLM classifier from day one. It would have worked, but at higher cost and lower auditability than a rules + classic ML combination for a problem that's 80% closed-set.

One more input belongs in this decision: context-window size. A GenAI feature that needs to read a long document or thread end-to-end (a policy doc, a multi-day escalation) can't just "read it all" — it needs a chunking or retrieval strategy once the material exceeds what fits in one call. That's a scoping decision a PM makes up front, not an engineering detail discovered during implementation.

## 3. How the system works

At the level a PM needs to track — not the level an ML engineer needs:

- **Rules engine:** engineer-maintained if/then logic. Same input → same output, always. No learning, no data requirement, brittle at the edges (every new edge case is a new line of code).
- **Classic ML (classifier/regressor):** a model trained on historical labeled examples (e.g., past tickets + their correct category) that learns a pattern and applies it to new, unseen input. Deterministic in the sense that the same input gives the same output *for a fixed model version*, but the model itself can drift as real-world data shifts away from training data.
- **GenAI / LLM:** a prompt plus context sent to a foundation model API, which samples from a learned probability distribution over possible outputs. The same input can produce different outputs across calls (even at low temperature, rarely exactly identical), because the system is generating, not looking up or classifying from a fixed set of categories.

The three sit on a spectrum of *how predictable the output is* and *how that behavior was produced* — not a ladder where GenAI is simply "more advanced." Each is the right tool for a different shape of problem.

**Why GenAI behaves this way, mechanically:** an LLM doesn't look anything up — it reads the prompt as a sequence of *tokens* (sub-word chunks, not words) and generates the next token by sampling from a learned probability distribution, one token at a time. Two settings shape that behavior directly:
- **Context window** — the total tokens (prompt + any injected context + the reply) a single call can hold. Exceed it and the call truncates or fails; this is why "just read the whole document" isn't always an option.
- **Temperature** — how much randomness the sampling step allows. Near zero, the model almost always picks the most likely next token (consistent, "safe"); higher, it samples more broadly (varied, occasionally surprising). This is a product-facing setting, not just a model parameter — it's worth specifying deliberately rather than leaving at a default.

**Worked example — a billing dispute ticket.** A customer emails: "I was charged twice, please refund the extra charge."
- *Deterministic path:* a classifier flags the ticket as a billing dispute → fixed logic checks the billing system for an actual duplicate charge → if confirmed, a defined refund workflow fires. Same input always produces the same output; a QA engineer can write "given a confirmed duplicate charge, assert refund = $X."
- *Probabilistic path:* an LLM reads the email and generates a reply — it might apologize and promise a refund, ask a clarifying question, or (if context is thin) state a refund amount that doesn't match the real duplicate charge. Run twice, the same email can get two different replies, and there's no single "assert X" test for that — only a measured rate of correct replies across a review sample.
- *The decision this forces:* the refund determination itself stays deterministic and auditable; GenAI's legitimate role is narrower — drafting the empathetic reply *around* a decision the deterministic system already made. Letting the model both decide the refund and communicate it is the "wrong layer" failure this framework exists to catch.

## 4. Risks and trade-offs

- **Rules engine:** brittle outside the cases it was written for; every edge case not covered either breaks silently or needs a human to add a new rule, so maintenance cost rises non-linearly with scope.
- **Classic ML:** needs continuously representative labeled data; silently degrades ("drifts") as real-world patterns shift away from training data, and that drift is invisible without monitoring.
- **GenAI:** adds non-determinism, hallucination risk, per-call cost, and latency that compounds at scale — quality can't be nailed down to a single "pass/fail" test the way traditional QA expects. Two mechanical risks sit underneath this: context-window truncation (a reply silently cut off mid-thought because the call hit its token cap, distinct from hallucination) and an unset or default temperature (nobody deliberately decided how consistent vs. varied the output should be).
- **Cross-cutting risk:** choosing GenAI by default when a cheaper, more auditable layer would do raises both cost and governance risk, especially for anything touching billing, compliance, or anything where a wrong answer has real consequences.

Mitigation at Northbeam: keep the rules engine and classifier doing the deterministic, high-volume, closed-set work; reserve GenAI for genuinely open-ended tasks, and always behind human review until its error rate is measured and acceptable for the specific context.

## 5. How I would measure success

This is a decision framework, not a model — so success isn't an accuracy number, it's **fewer wrong-layer choices and faster time-to-decision**:

- **North-star metric:** % of new AI feature proposals that go through the layer-selection framework before a build decision is made (vs. defaulting straight to GenAI).
- **Supporting metrics:** average time from "feature idea" to "layer decision"; rework rate (features rebuilt on a different layer within 6 months of launch, a proxy for having picked wrong the first time).
- **Guardrail metrics:** for anything touching compliance/billing, zero tolerance for a GenAI-only solution without a deterministic fallback or human checkpoint.

---
**Related:** links to other artifacts in this repo.
