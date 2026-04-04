# <img src="assets/icons/cost.png" width="25"/> Cost Management

**What does it cost to build and run, and what drives those costs?**


AI costs behave differently from traditional software costs. An AI call costs based on how much text goes in and comes out — and that can vary wildly depending on the input, the conversation length, or how many steps the system takes. If you're not thinking about cost as part of the design, you'll find out about it in the invoice.

## What drives cost

**Tokens.** Most AI providers charge by token (the unit of text the model reads and generates). Every piece of context you include in the prompt costs money: the system prompt, conversation history, retrieved documents, and the user's input. The response costs money too. Longer inputs and longer outputs mean higher costs per call.

**Model choice.** Not every task needs the most powerful model. A simple classification or formatting task might perform just as well with a smaller, cheaper model. Choosing the right model for the job is one of the most direct cost levers available, and one that's easy to overlook when you default to the most capable option.

**Volume.** A single AI call is cheap. A thousand calls a day adds up. A system that makes multiple calls per user interaction (retrieval, then generation, then validation) multiplies the cost per interaction. Volume is where costs go from negligible to significant.

**Conversation length.** In conversational systems, every message in the history is sent with every new request. A 50-message conversation means you're paying for all 50 messages plus the new one, every time. Long conversations get expensive fast.

## Hidden Costs

The cost you see from your AI provider is the visible cost, but it's not the full picture.

**Retrieval infrastructure.** Vector databases, embedding generation, indexing pipelines, these have their own hosting and compute costs that are independent of the model provider.

**Human review.** If your system includes human-in-the-loop checkpoints, that's someone's time. The more volume the system handles, the more review time it requires, unless you design the escalation threshold thoughtfully.

**Iteration and experimentation.** Prompt engineering, eval runs, A/B testing different models, the development process itself consumes tokens. This is expected and necessary, but worth being aware of, especially during the build phase.

**Failure and retries.** When calls fail and get retried, you're paying twice. When an agent gets stuck in a loop, you're paying for every iteration. Failures aren't just a reliability problem, they're a cost problem.

## How cost changes based on your system

**🧱 Basic** — Cost per call is straightforward and predictable. The main lever is model choice — using the right model for the complexity of the task.

**💬 Session** — Cost grows with conversation length. Every new message carries the full history. This is often the first place teams are surprised by costs.

**🧠 Memory** — Retrieval adds cost in two places: the infrastructure that supports it, and the extra tokens from injecting retrieved content into the prompt.

**🔧 Action** — Each tool call may involve additional API costs beyond the model itself. If tools return data that gets included in the next prompt, that's more tokens too.

**⛓️ Chain** — Multiple calls per interaction multiply the base cost. A 3-step chain costs at least 3x a single call, more if steps include retrieval or retries.

**🤖 Autonomy** — Cost becomes unpredictable. The agent decides how many calls to make, and a task you expect to take 5 calls might take 50. Setting budgets and limits is essential at this level.

---

[← Failure modes](06-failure-modes.md) · [Decision guide →](08-decision-guide.md)
