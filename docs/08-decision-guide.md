# Decision Guide

Practical tools for scoping AI features.

These questions help you scope AI features and avoid building something brittle.  
This isn't an exhaustive list, these are some of the most common signals worth watching for.

### Start Here

- **What problem are they actually trying to solve?** Get past "we want AI to do X" and understand the underlying pain point. Sometimes the best solution isn't AI at all.
- **What blocks does this system need?** Use the building blocks framework to figure out what you're actually building. Someone may describe a chatbot, but the requirements imply retrieval, tools, and a chained workflow underneath.

### Context

- **What data does this feature need access to in order to give a good answer?** If the answer is "everything in our CRM," that's a retrieval design problem, not a prompting problem.
- **How much context does the AI actually need vs. how much would we dump in?** More context isn't always better. Irrelevant information creates noise and degrades output quality.

### Oversight

- **Where does the human belong in this workflow?** The requester may assume full automation — "just automate it" — but the right answer might be "AI drafts, human approves."
- **What does the review step look like?** Not just "someone checks it" but concretely: who reviews, how do they review, and what do they do when the AI is wrong?

### Data

- **What data will flow through this system?** Trace the path before you build anything. If customer data, employee data, or anything sensitive is involved, think about trust boundaries first.

### Sustainability

- **How will we know this is still working in 3 months?** If there's no plan for monitoring or periodic review, that's worth raising early.
- **What will this cost to run?** Get a rough sense of token volume, model costs, and infrastructure needs before committing.

## What blocks am I building with?

Walk through these questions.

| Question | If yes, you're using | Pay extra attention to |
|----------|---------------------|----------------------|
| Is this a single AI call with no conversation or memory? | 🧱 Basic | Context Management |
| Does it involve back-and-forth conversation or a system prompt? | 💬 Session | Context Management, Cost |
| Does it pull in external data (docs, database, API) before prompting? | 🧠 Memory | Context Management, Data Flow, Evaluation |
| Can the AI take actions (call APIs, create records, send messages)? | 🔧 Action | Human Oversight, Data Flow, Failure Modes |
| Does it chain multiple AI calls where one output feeds the next? | ⛓️ Chain | Evaluation, Failure Modes, Cost |
| Does the AI decide what to do next — choosing tools, planning steps, looping? | 🤖 Autonomy | All concepts at highest stakes |

## When AI isn't the answer

Sometimes the best response to "can we use AI for this?" is "we shouldn't."

AI adds value when the problem involves generating, summarizing, or interpreting unstructured information — when the task requires judgment, language, or pattern recognition that's hard to express as rules. But not every problem fits that description.

**Consider not using AI when:**

**The problem has clear, deterministic rules.** If you can write an if/else statement or a database query that solves it reliably, AI adds complexity without value. A rules engine is cheaper, faster, and predictable.

**The cost of being wrong is high and the output can't be reviewed.** If the AI's output goes directly to a customer, a regulator, or a financial system with no human checkpoint, and the consequences of a mistake are severe, the risk may not be worth it.

**The data doesn't exist or isn't accessible.** AI needs context to work well. If the feature requires data that's locked in someone's head, scattered across undocumented systems, or simply doesn't exist yet, the first step is getting the data, not adding AI.

**A simpler tool would solve it.** Sometimes the request is really "help me see this data differently." That's a reporting problem, not an AI problem.

---

[← Cost Management](07-cost.md) · [README](README.md)
