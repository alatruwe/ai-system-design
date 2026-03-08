# AI System Design Principles
### A practical framework for building AI-powered features

**Author:** Adeline Latruwe

---

This is a thinking framework for anyone building AI into products and systems, not just using AI, but designing AI features. It's tool-agnostic, and it's framed as questions to think about rather than rules to follow.

**Who this is for:**
- Developers building AI features
- Product managers scoping AI features
- Technical leads making architecture decisions
- Anyone evaluating whether AI is the right fit for a problem

**What this is not:**
- A technical implementation guide — it won't teach you how to set up RAG, build an agent, or configure a vector database
- A tutorial on any specific tool, provider, or framework
- A coding resource — there's no code in this guide

## How this guide works

The guide is built around a simple idea: AI systems are made of **building blocks**, and each block introduces **concepts and risks** you need to think about. The more blocks your system uses, the more there is to consider.

1. **Start with the blocks.** [Anatomy of an AI System](docs/00-anatomy.md) introduces six building blocks — basic single prompt, session, memory, action, chain, and autonomy — and the common patterns they combine into.
2. **Check what matters.** [Concepts](00b-concepts-and-risks.md) maps each block to the concepts you need to think about: context management, oversight, evaluation, data flow, failure modes, and cost.
3. **Go deeper where needed.** Each concept has a deep-dive section. Read the ones relevant to your blocks, skip the rest.

## Example: using this guide

Say your team wants to build an automated workflow that receives support emails, searches a knowledge base for relevant articles, and drafts a response.

**Step 1 — Identify your blocks.** You're looking at 🧱 Basic (single AI call), 🧠 Memory (knowledge base retrieval), and possibly 🔧 Action (if the system sends the response or creates a ticket).

**Step 2 — Check what matters.** [Concepts](00b-concepts-and-risks.md) tells you that with 🧠 Memory, you need to think about context management, data flow, and evaluation. With 🔧 Action, add human oversight and failure modes.

**Step 3 — Read the relevant deep dives.** Context Management helps you think about what gets retrieved and whether it's the right information. Human Oversight helps you decide whether the drafted response goes out automatically or waits for approval. Failure Modes reminds you to consider what happens when retrieval returns the wrong articles.

---

## Guide Structure

| File | Topic |
|------|-------|
| [00-anatomy.md](00-anatomy.md) | Anatomy of an AI System — The Building Blocks |
| [01-concepts-and-risks.md](01-concepts-and-risks.md) | What Each Block Needs & Key Concepts |
| [02-context.md](02-context.md) | Context Management |
| [03-oversight.md](03-oversight.md) | Human Oversight & Guardrails |
| [04-evaluation.md](04-evaluation.md) | Evaluation & Observability |
| [05-data-flow.md](05-data-flow.md) | Data Flow & Trust Boundaries |
| [06-failure-modes.md](06-failure-modes.md) | Failure Modes |
| [07-cost.md](07-cost.md) | Cost Management |
| [08-decision-guide.md](08-decision-guide.md) | Decision Guide |

---

## License

Apache 2.0
