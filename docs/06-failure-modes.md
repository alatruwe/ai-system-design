# Failure modes

**What happens when the AI breaks?**


AI systems will fail. The same input can produce different outputs, or return something that looks right but isn't. If you're building AI into a system, you need to think about failure the same way you think about failure for any external dependency: what happens when it breaks?

The difference with AI is that some failures don't look like failures. A hallucinated answer is still well-written. A response based on bad retrieval is still confident. The system doesn't crash, it just quietly gets it wrong.

## Common failure types

Not every failure type applies to every system, but knowing the catalog helps you ask the right questions during design.

**Hallucination.** The model confidently makes things up, invented statistics, fabricated references, details that aren't in the source data. This is especially dangerous when the output looks polished and authoritative.

**Rate limits.** AI providers limit how many requests you can make in a given time window. This becomes especially relevant with chains and agents. An agent that decides to make 50 calls to complete a task can burn through your quota in ways you didn't anticipate.

**Timeouts.** The model takes too long to respond. This is especially common with larger prompts or complex reasoning tasks, and it can break workflows that expect a response within a certain window.

**Model updates.** Providers may update models over time and the outputs won't be identical. What worked before may behave differently.

**Context overflow.** The input exceeds the model's context window. The call either fails outright or the model silently truncates the input, working with incomplete information.

**Malformed output.** You expected JSON, you got a paragraph. You expected a number, you got a sentence. The model didn't follow the output format, and downstream systems either break or silently process something unexpected.

## How failures compound

For a basic AI system, a failure is usually visible, you see the bad output and deal with it. As you add blocks, failures get harder to spot and harder to trace.

With retrieval (🧠), the system can pull in the wrong documents, and the model produces a confident answer based on bad information. Nothing looks broken.

With chains (⛓️), a hallucinated value in step 1 becomes the input for step 2, which transforms it into something that looks even more legitimate by step 3. The final output is polished but built on a broken foundation.

With autonomy (🤖), the agent can get stuck in loops, choose wrong tools, or take irreversible actions based on bad reasoning. The failure isn't just bad text — it's bad actions.

## How failure modes change based on your system

**🧱 Basic and 💬 Session** — Failures are visible. You see the output and can judge whether something went wrong. The main risk is hallucination (output that looks right but isn't).

**🧠 Memory** — Retrieval adds a silent failure layer. The model's output might be fine given the context it received, but the context itself might be wrong, outdated, or irrelevant.

**🔧 Action** — The model is triggering actions, not just generating text. A failure here might mean posting a message with wrong information, creating a record with bad data, or calling a tool with the wrong parameters. The consequences go beyond a bad response, something happened in the real world.

**⛓️ Chain** — Failures compound through steps. Each step trusts the output of the previous one, and by the time the final result appears, the original error may be unrecognizable.

**🤖 Autonomy** — The agent chooses its own path, which means it can fail in ways you didn't anticipate. Loops, wrong tool selection, and irreversible actions based on bad reasoning are all possibilities that can't be fully tested upfront.

---

[← Data flow & Trust boundaries](05-data-flow.md) · [Cost Management →](07-cost.md)
