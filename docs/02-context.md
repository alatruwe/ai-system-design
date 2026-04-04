# <img src="assets/icons/context.png" width="25"/> Context Management

**What information does the AI have access to, and is it the right information?**


AI doesn't know what you didn't tell it. It can't read your mind, it can't check your database on its own, and it can't guess what you left out.  
Every AI interaction, from a one-line prompt to a complex retrieval pipeline, comes down to the same question: **what context is the model working with, and is it the right context?**

This is true at every level of complexity. When you type a question into into any AI chat interface, you're choosing what context to provide. When you build a system that assembles a prompt from a database query, a user's message, and a set of instructions, you're making that same choice, just programmatically.

## The context window

Every model has a context window: the total amount of text (input and output combined) it can work with in a single interaction.  
Think of it as working memory. Anything outside that window doesn't exist for the model.

This matters because it's finite.  
A model with a 128k token context window sounds like a lot, until you're stuffing in a system prompt, conversation history, retrieved documents, and the user's actual question.  
Context is a budget, and every piece of information you include is a trade-off against something else you could have included.

The practical question isn't "how big is the context window", it's **"what deserves to be in there, and what doesn't?"**

## Common context strategies

When you're scoping an AI feature, one of the first design questions is: **where does the context come from?** There are a few common patterns:

**Direct input** — The context is provided directly in the prompt, whether the user types it, pastes content, or a document is loaded in. "Here's the spec, now answer questions about it." This is the simplest pattern and the one most people start with.

**Retrieval (RAG)** — The system searches a knowledge base, database, or set of documents and pulls in the most relevant pieces based on the user's query. This is the pattern behind most "chat with your docs" tools.

**API lookup** — The system calls an external API to get structured data (a customer record, order status, system metrics) and includes it in the prompt.

**Tool use / function calling** — The model itself requests specific data mid-interaction by calling defined functions, and the system provides the results back.

The choice of strategy shapes everything downstream: what infrastructure you need, what can go wrong, and how much the context can be trusted.

## Common mistakes and risks

Most context problems fall into six categories:

**Too much context.** Dumping everything in and hoping the model figures it out. The signal gets lost in noise. Just because you *can* include the entire document doesn't mean you *should*. The model may lose focus on what actually matters.

**Too little context.** The model doesn't have enough information to give a useful answer, so it guesses or hallucinates. This is especially common when builders assume the model "just knows" something that was never included in the prompt.

**Wrong context.** The retrieval pulls in documents that look relevant but aren't (wrong department, similar keywords but different meaning...). The model has no way to know the context is wrong. It will use whatever you give it with full confidence.

**Stale context.** The data was accurate last month but has since changed, and nobody updated the source. The model answers based on what it was given, not what's currently true.

**Conflicting context.** Multiple sources feed into the same prompt and they disagree. The documentation says one thing, the database says another, the user's message implies a third. The model has to pick one, and you won't know which it chose or why.

**Poisoned context (prompt injection).** When context comes from sources you don't control (user input, retrieved documents, external APIs...) there's a risk that the context contains instructions the model might follow. The more sources feeding into your prompt, the more entry points exist for unintended instructions.

A good rule of thumb: **if you can't explain what context the AI is using and why, it's worth pausing to map it out before going further.**

## How context changes based on your system

Context management looks different depending on what you're building:

**🧱 Basic** — The prompt is the entire design. The quality of the output depends entirely on the quality of what you put in. At this level, context management is prompt craft.

**💬 Session** — Conversation history becomes part of the context. As conversations grow, you're spending more of your context budget on history. The question shifts from "what do I include?" to "what do I keep, and what can I let go?"

**🧠 Memory** — External data enters the prompt through retrieval. Now the question is: what gets retrieved, how relevant is it, and is it current? You're no longer hand-crafting context: you're designing the system that assembles it.

**⛓️ Chain** — Each step produces output that becomes context for the next step. Context management is no longer about a single prompt, it's about what flows between steps and whether the right information survives the journey.

**🤖 Autonomy** — The agent decides what context to gather. It chooses which tools to call, which data to retrieve, and how to use the results. You're not managing context directly, you're setting the boundaries for how the agent manages it.

---

[← What each block needs & key concepts](01-concepts-and-risks.md) · [Human oversight & guardrails →](03-oversight.md)
