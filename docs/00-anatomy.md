# Anatomy of an AI System

Before we talk about principles, we need a shared picture of what an AI system actually looks like. Not every AI feature is the same, they range from simple to complex, and have different components, capabilities, and risk.

Think of AI systems as built from building blocks.  
Each block adds a capability, and complexity comes from how many blocks you're combining and how much freedom the system has.  
This framing helps you scope what you're actually building, which tells you what you need to design for.

## The Building Blocks

Every AI system is assembled from some combination of these:

- 🧱 **Basic** — A single prompt in, single response out. The atomic unit.
- 💬 **Session** — Conversation history and a system prompt. The model has context from earlier in the conversation, but that context is ephemeral, once the session ends, it's gone. Present when the system is conversational; optional in any pattern.
- 🧠 **Memory** — Retrieval. The system pulls in persistent external context (documents, databases) at query time. Unlike session, this data exists independently of any conversation.
- 🔧 **Action** — Tools. The system can do things in the world (call APIs, edit files, send messages).
- ⛓️ **Chain** — Multiple AI calls in sequence, where one output feeds the next. The workflow is fixed, with defined steps.
- 🤖 **Autonomy** — The AI decides what to do next. It plans, selects tools, takes actions, observes results, and loops. The system is goal-oriented.


These are the archetypes you'll encounter in the wild.  
Most real-world systems are combinations of blocks, but these patterns give you a shared vocabulary for talking about what you're building.

The complexity jumps come from three thresholds: how much can the system do in a **single call**, what happens when you **chain multiple calls** together, and what changes when the AI gains **autonomy** over its own path.

---
### Simple Interaction
**Blocks:** 🧱

A single input, a single output, no conversation.  
Think of a step in an automated workflow that takes a block of text and summarizes it: one prompt, one response, done.

```
Input → Prompt → Model → Response
```

There's no memory, no session, no back-and-forth. The quality depends entirely on the prompt and the input.  
This is the most basic building block, and it's often a single step inside a larger system.

---
### Chatbot
**Blocks:** 🧱 + 💬

Add a system prompt and conversation history, and the model starts behaving more consistently. It has instructions and it remembers what you said earlier in the conversation. This is what you're doing when you open ChatGPT or Claude in your browser and have a plain back-and-forth conversation, no file uploads, no project context, just chat.

```
User → System Prompt + Conversation History → Model → Response
```

The system prompt is where you define the AI's role, tone, constraints, and goals. This is the simplest version of "designing" an AI experience rather than just using one. Every time you start a conversation and say "you are a helpful assistant that speaks like a pirate", that's a system prompt. The platform is doing the same thing behind the scenes.

**The boundary with retrieval:** As soon as you attach files, add documents to a project, or enable features that pull in external context, you've added the 🧠 Memory block. Claude's Projects feature, ChatGPT's file uploads, or any setup where the system searches through provided materials to inform its response uses retrieval. The distinction is whether the model is working with just the conversation, or with external data injected alongside it.

---
### Retrieval, Tools, or Both
**Blocks:** 🧱 + 🧠 and/or 🔧 (+ 💬 if conversational)

This is where the model gains capabilities beyond just talking.  
There are three variants, all at a similar complexity because each adds one kind of capability to a single AI call:

**Retrieval (🧱 + 🧠):** The model pulls in relevant information at query time. Consider a workflow step that receives a support email, searches the knowledge base for relevant articles, and drafts a response. The AI never has a conversation, it gets the email, retrieves context, and produces a draft in a single call.  

**Tools (🧱 + 🔧):** The model can take actions: call an API, query a database, send a message. It's still a single interaction cycle, but the model can now *do things* beyond generating text. A bot that receives an alert, calls an API to gather system status, and posts a formatted summary to Slack is operating here, no conversation, just input, action, output.

**Retrieval + Tools (🧱 + 🧠 + 🔧):** The model can both pull in external context and take actions within a single call. Consider a support chatbot that searches your knowledge base for relevant articles (retrieval) and can also create a ticket or issue a refund (tools). The system is more capable but still operates in a single prompt-response cycle, the developer defines what tools and data sources are available, and the model uses them within one interaction.

The key distinction from chained workflows: everything here happens within **a single AI call**. The model might use tools and retrieval, but there's one prompt, one response cycle.

---
### Chained Workflow
**Blocks:** 🧱 + 🧠 + 🔧 + ⛓️ (+ 💬 if conversational)

Multiple AI calls connected in sequence, where the output of one becomes the input of the next. Each step transforms, validates, or enriches the data. The workflow is fixed, with defined steps: step 1 feeds step 2 feeds step 3.

```
Input → AI Call 1 → Transform → AI Call 2 → Validate → AI Call 3 → Output
```

Chained workflows multiply both capability and fragility. If one step fails silently (a rate limit, a bad parse, a hallucinated intermediate value...) the final output can look polished but be wrong. This is where the failure modes principle becomes critical.

Consider a content pipeline that takes a research paper, extracts key findings (call 1), generates a summary for a non-technical audience (call 2), and produces social media posts (call 3). Each step is a separate AI call, and the developer wired them together. The system is powerful, but a hallucinated finding in step 1 flows through every step after it.

---
### Agentic System
**Blocks:** 🧱 + 🧠 + 🔧 + ⛓️ + 🤖 (+ 💬 if conversational)

The same blocks as a chained workflow, plus one critical addition: **autonomy**. The AI doesn't just follow a fixed sequence: it decides what to do next. It plans, selects tools, takes actions, observes results, and loops until the goal is met. The human sets a goal; the agent figures out the steps.

```
User Goal → Planner → Tool Selection → Action → Observation → Loop → Final Output
```

For example, this is where AI coding agents live. You say "refactor this module to use async" and it reads your codebase, plans an approach, edits files, runs tests, sees failures, fixes them, and loops until the tests pass. It can load skills, run background agents, and use tools like bash and file editing. You set the goal, it decides the path.

Agentic systems are powerful but hard to predict. The AI is making decisions about *what to do next*, not just *what to say*. Every principle in this guide applies here with the highest stakes.

---
## Summary

| Pattern | Blocks | Example | Key Risk |
|---------|--------|---------|----------|
| Simple interaction | 🧱 | A summarization step in a workflow | Prompt quality |
| Chatbot | 🧱 💬 | ChatGPT / Claude conversation | System prompt design |
| Retrieval | 🧱 🧠 | Workflow step: email → knowledge base search → draft response | Retrieval quality, data freshness |
| Tools | 🧱 🔧 | Bot: alert → API status check → Slack summary | Tool reliability, error handling |
| Retrieval + tools | 🧱 🧠 🔧 | Support bot with knowledge base + ticket creation | Combined retrieval and action risks |
| Chained workflow | 🧱 🧠 🔧 ⛓️ | Multi-step content pipeline | Silent failures, compounding errors |
| Agentic system | 🧱 🧠 🔧 ⛓️ 🤖 | AI coding agents | Unpredictable decisions, trust boundaries |


Most real-world systems are combinations, not clean single-pattern implementations. A chained workflow might contain several simple interaction steps and a retrieval layer inside it. An agentic system might orchestrate a mix of single calls and chained workflows as part of its plan. The complexity of your system is determined by the *most complex pattern present*, which determines the design rigor you need.

---

← [README](README.md) · [Next: What Each Block Needs & Key Concepts →](01-concepts-and-risks.md)
