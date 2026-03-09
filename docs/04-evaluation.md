# Evaluation & Observability

**How do you know the system is working, and how do you know when it stops?**


Most teams ship an AI feature, confirm it works in a demo, and never look at the outputs again until someone complains. The problem is that by the time someone complains, the feature may have been degrading for weeks.

AI systems can fail like traditional software, they crash, they timeout, they throw errors. But they also fail in a way traditional software doesn't: they degrade. The output gets a little worse, a little less relevant, a little less accurate. And because the output still looks polished, nobody notices.

Two concepts help you stay ahead of this:

**Evaluation** is about quality — is the output good? Does it meet the criteria you defined? Evaluation answers the question: is this system doing what it's supposed to do?

**Observability** is about visibility — can you see what's happening inside the system? What went in, what came out, what happened in between? Observability answers the question: when something goes wrong, can you figure out why?

You need both. Evaluation without observability tells you *something broke* but not *where*. Observability without evaluation gives you data but no way to know if what you're seeing is a problem.

## Define what success looks like

When you're scoping an AI feature, define what you'd need to see in the output to call it working, the same way you'd define acceptance criteria for any feature. What does a good response look like for this use case? What would a bad one look like? Where's the line between "useful" and "not worth shipping"?

These criteria will look different for every system. A summarization tool might need to preserve key facts without inventing new ones. A classification system might need to hit a certain accuracy rate. A drafting tool might need to match a specific tone. The specifics matter less than the habit: **if you can't describe what good looks like, you won't be able to tell when it stops being good.**

## Common pitfall: drift

AI output quality can change over time, sometimes without anyone touching the system, sometimes as a side effect of changes that seemed unrelated. This is drift, and it has several sources:

**Model updates.** Providers deprecate model versions and ship updates. Even when you pin to a specific version, that version can be sunset with a migration deadline. The replacement won't produce identical outputs.

**Data changes.** The documents your retrieval layer pulls from get updated, reorganized, or deleted. The model keeps answering confidently, but the underlying data has shifted.

**Prompt edits.** Someone tweaks the system prompt to fix one case and accidentally breaks three others. Without a way to compare before and after, the regression is invisible.

**User behavior shifts.** Users start using the feature in ways you didn't anticipate: longer inputs, different domains, unexpected edge cases. The system wasn't designed for these patterns and the output quality drops.

The practical takeaway isn't that AI systems are unstable. It's that **you need to be able to tell if your outputs changed, regardless of the reason.**

## What to Think About

**Visibility into what's happening.** If something goes wrong, can you retrace what the system did? Knowing what went in, what came out, and what happened in between is the foundation, without it, debugging is guesswork.

**A baseline for quality.** If the output starts to degrade, how would you know? Having something to compare against, even a small set of representative examples, is what turns a vague sense of "this feels off" into a concrete signal.

**Change detection.** If the same request starts producing different results, different formatting, or taking noticeably longer, something shifted. These changes aren't always problems, but they're always worth understanding.

**Retrieval relevance.** When the system pulls in external data, that data can go stale or shift without warning. The model will keep answering confidently regardless, the question is whether anyone is checking what it's working with.

## How evaluation and observability change based on your system

**🧱 Basic and 💬 Session** — Evaluation and observability needs are minimal. Having a record of inputs and outputs is still valuable if the system runs at any volume.

**🧠 Memory** — Retrieval quality degrades silently. Knowing what was retrieved alongside each response is what lets you tell whether a bad output came from bad retrieval or bad generation.

**⛓️ Chain** — A failure in step 1 can look like a success by step 3. Visibility into each step — not just the final output — is what makes problems traceable.

**🤖 Autonomy** — The path isn't predetermined, so not every execution can be tested upfront. Understanding what the agent did and why — which tools it called, what decisions it made — is what makes the system auditable.

---

[← Human oversight & Guardrails](03-oversight.md) · [Data flow & Trust boundaries →](05-data-flow.md)
