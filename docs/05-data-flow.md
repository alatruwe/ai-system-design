# Data flow & Trust boundaries

**Where does data go when it enters your system, and who can see it?**


Every time data enters an AI system, it crosses a boundary. It leaves your environment and enters someone else's: a model provider's infrastructure, a third-party API, a cloud service. That boundary matters because the rules change on the other side.

What happens to the data after you send it? Is it stored? Is it logged? Is it used to improve future models?  
These are the same questions you'd ask about any third-party service that handles your data. AI is no different, but the boundaries are easier to overlook because the interaction may feel like a conversation, not a data transfer.

## Trust boundaries

A trust boundary is the point where data moves from a system you control to one you don't. In traditional software, these boundaries are well-understood (your database, your API, your network...). In AI systems, new boundaries appear:

**Your system → the model provider.** Every prompt you send leaves your infrastructure. The data is now subject to the provider's policies, not yours.

**The model → downstream systems.** If the model's output feeds into other tools, databases, or user-facing products, the data continues to travel. A hallucinated value or a leaked piece of context doesn't stop at the model's response, it flows wherever that response goes.

Thinking about where these boundaries are in your system is what helps you decide what data is safe to include, what needs extra care, and what shouldn't be there at all.

## Provider policies

Different providers handle data differently, and the same provider may have different policies depending on the tier you're on.

**API vs. consumer product.** Many providers treat API calls differently from their consumer chat products. API calls often have stronger data retention and training opt-out guarantees. Consumer products may use conversations for training unless you opt out.

**Enterprise agreements.** Enterprise tiers typically offer zero-data-retention agreements, stronger privacy guarantees, and contractual commitments about data handling. These matter when processing sensitive data.

The key takeaway is that provider policies vary, they change over time, and the defaults aren't always what you'd expect. It's worth understanding what applies to your specific setup.

## Sensitive data

Any data that can identify a specific individual (names, email addresses, financial information, medical records...) deserves extra thought before it enters an AI workflow.

The questions worth asking: does the task actually require this data, or could it work with anonymized or redacted input? Do your existing data handling policies account for data flowing through an AI provider? If sensitive data enters a prompt, could it surface in logs, model outputs, or the provider's systems?

These aren't new questions, they're the same ones that apply to any system that handles personal data. The difference is that AI workflows make it easy to include data without thinking about where it ends up, especially when retrieval or tool use pulls in data automatically.

## Common pitfall: Treating the provider like an internal system

It's easy to think of an AI API call the same way you'd think of calling your own database: send data, get a response. But every call crosses a trust boundary. The provider is a third party, and the data you send is subject to their policies, not yours. This distinction gets lost quickly, especially when the integration is seamless and the API feels like part of your own system.

## How data flow and Trust boundaries change based on your system

**🧱 Basic and 💬 Session** — The data in your prompt goes to the provider. That's one trust boundary: what you're sending and what their policies say about it.

**🧠 Memory** — The system pulls in external data and sends it to the provider. The trust boundary question expands: not just "what am I sending?" but "what is the system pulling in on my behalf, and should that data be crossing this boundary?"

**🔧 Action** — The model can send data to external tools or trigger actions in other systems. Sometimes data flows both ways, a tool returns results that enter the next prompt. Sometimes the action is one-directional, like posting a message. Either way, each tool interaction is a potential trust boundary to think about.

**⛓️ Chain** — Data flows through multiple steps, potentially crossing multiple boundaries. A piece of sensitive data that enters at step 1 can propagate through every step that follows, crossing a new trust boundary each time.

**🤖 Autonomy** — The agent decides what data to gather and where to send it. Trust boundaries become harder to map upfront because the path isn't fixed. The agent's decisions determine what data crosses which boundaries.

---

[← Evaluation & Observability](04-evaluation.md) · [Failure Modes →](06-failure-modes.md)
