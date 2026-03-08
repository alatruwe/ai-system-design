# What each block needs & key concepts

Every building block in an AI system introduces concepts you need to think about.  
This section gives you the full landscape at a glance, then maps each block to what matters most. The deep-dive sections that follow cover each concept in detail.  
Use this page to figure out which ones are relevant to what you're building.

## The Concepts

**Context Management** — What information does the AI have access to, and is it the right information? Covers how to think about what goes into the model, common mistakes, prompt injection as a risk, and how context needs change based on which blocks your system uses. [Deep dive →](02-context.md)

**Human Oversight & Guardrails** — How much autonomy does the AI have, and who catches mistakes? Covers the autonomy spectrum, automated guardrails, and how to decide where humans belong in a workflow. [Deep dive →](03-oversight.md)

**Evaluation & Observability** — How do you know the system is working, and how do you know when it stops? Covers what "good output" means, why AI systems degrade silently, and what signals to watch for. [Deep dive →](04-evaluation.md)

**Data Flow & Trust Boundaries** — Where does data go when it enters your system, and who can see it? Covers trust boundaries, provider policies, and questions to ask before data leaves your system. [Deep dive →](05-data-flow.md)

**Failure Modes** — What happens when the AI breaks? Covers the ways AI systems fail  (hallucination, rate limits, timeouts, malformed output...) and why you need to plan for them before you ship. [Deep dive →](06-failure-modes.md)

**Cost Management** — What does it cost to build and run, and what drives those costs? Covers token economics, model selection as a cost lever, enterprise licensing considerations, and the hidden costs that aren't on the invoice. [Deep dive →](07-cost.md)

## What each block needs

Not every concept applies equally to every block. Here's what to pay attention to based on what you're building with.

### 🧱 Basic — Single Interaction

The simplest case. Your main concern is the quality of what goes in and what comes out.

- **Context Management** — The prompt is the entire design. A clear, well-structured prompt is the difference between useful output and garbage.
- **Failure Modes** — What happens if the model returns something unexpected? Even a single call can hallucinate or return malformed output.
- **Cost Management** — A single call is cheap, but if it runs at volume (e.g., a step in a workflow processing thousands of items), token costs add up fast.

### 💬 Session — Conversational Context

You're maintaining state across a conversation. Everything from 🧱 applies, plus:

- **Context Management** — Conversation history is context. Most platforms handle this for you and the session management is built in. But if you're building your own conversational system, as conversations grow, older messages may be truncated or lost. What gets kept and what gets dropped or compacted is a design decision.
- **Cost Management** — Every message in the conversation history is sent with every new request, whether the platform manages it or you do. Long conversations mean growing token costs per call.

### 🧠 Memory — Retrieval

The system pulls in external data. Everything from 🧱 applies, plus:

- **Context Management** — Now the critical question is: what data gets retrieved, how relevant is it, and what happens when retrieval returns the wrong thing?
- **Data Flow & Trust Boundaries** — External data enters the prompt. Where does it come from? Is it trusted? Who can see it?
- **Evaluation & Observability** — Retrieval quality degrades silently. Documents get outdated, indexes go stale, and the model keeps answering confidently with bad information.
- **Cost Management** — Retrieval infrastructure (vector databases, embeddings, indexing) has its own costs independent of the model.

### 🔧 Action — Tools

The model can do things in the world. Everything from 🧱 applies, plus:

- **Human Oversight** — The AI is taking actions, not just generating text. Who approves those actions? What happens if the AI calls the wrong tool or passes bad parameters?
- **Data Flow & Trust Boundaries** — Tools often interact with external services. Data flows out of your system. What's being sent, and to where?
- **Failure Modes** — Tool calls can fail, timeout, or return errors. The model needs to handle these gracefully, and so does your system.

### ⛓️ Chain — Fixed Workflow

Multiple AI calls in sequence. Everything from the individual blocks applies, compounded:

- **Evaluation & Observability** — A failure in step 1 looks like a success by step 3. Visibility into each step, not just the final output, is what makes problems traceable.
- **Failure Modes** — This is where failures compound. A hallucinated value in one step flows through every step after it. Rate limits on one call can stall the whole pipeline. Design for partial failure.
- **Cost Management** — Multiple calls multiply costs. A 3-step chain costs 3x a single call at minimum, more if steps include retrieval or retries.

### 🤖 Autonomy — Agentic System

The AI decides its own path. Everything above applies, at the highest stakes:

- **Human Oversight** — The AI is making decisions about what to do next. Where are the checkpoints? What actions require approval? What's the kill switch?
- **Evaluation & Observability** — The path isn't predetermined, so you can't test every possible execution. You need robust logging, monitoring, and the ability to audit what the agent did and why.
- **Failure Modes** — The agent can get stuck in loops, choose wrong tools, or take irreversible actions based on bad reasoning. These are worth designing for upfront.
- **Cost Management** — Agentic systems can make an unpredictable number of calls. A task you expect to take 5 calls might take 50. Set budgets and limits.

## Quick reference

| Block | Context | Oversight | Evaluation | Data Flow | Failure Modes | Cost |
|-------|:-------:|:---------:|:----------:|:---------:|:-------------:|:----:|
| 🧱 Basic | ● | | | | ○ | ○ |
| 💬 Session | ● | | | | ○ | ○ |
| 🧠 Memory | ● | | ● | ● | ○ | ● |
| 🔧 Action | ○ | ● | | ● | ● | |
| ⛓️ Chain | ○ | | ● | ○  | ● | ● |
| 🤖 Autonomy | ○ | ● | ● | ● | ● | ● |

● = critical · ○ = relevant

---

← [Anatomy of an AI System](00-anatomy.md) · [Next: Context Management →](02-context.md)
