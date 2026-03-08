# Human oversight & guardrails

**How much autonomy does the AI have, and who catches mistakes?**


Every AI system makes a decision about how much autonomy the AI has. If you don't design that decision intentionally, the default is usually more autonomy than you meant to give, followed by more risks than you meant to support.

## Two tools for oversight

There are two ways to keep an AI system in check, and most systems need some combination of both.

### Guardrails

Guardrails are automated checks — does the output match the expected format? Does it contain prohibited content? Is the confidence score above the threshold? Guardrails catch the obvious, predictable failures without requiring a human to review every response. They're fast, consistent, and scale with volume.

### Human in the loop

Human-in-the-loop is about the human as a gate before actions happen — approving, correcting, or escalating before the output goes anywhere. In practice, it takes three forms:

**Approval gate** — AI produces output, human approves before it's released. This is the most common pattern. The AI drafts, the human decides whether it goes out. Example: AI drafts a customer email, human reads it and clicks send.

**Escalation** — AI handles routine cases automatically, flags uncertain or high-risk ones for human decision. Not everything needs a human, just the cases the system isn't confident about. Example: AI auto-categorizes support tickets, but routes anything below a confidence threshold to a person for triage.

**Correction** — Human reviews AI output, edits it, and those corrections feed back into improving the system. The human isn't just a gatekeeper, they're a teacher. Example: AI classifies incoming leads as hot, warm, or cold. Sales team reclassifies the ones it got wrong, and those corrections retrain the scoring tool.

Neither guardrails nor human review replaces the other. Guardrails can't exercise judgment. Humans can't review every output at scale. The question for your system is: **what mix of automated checks and human judgment keeps this safe?**

## Reversibility

One useful lens for deciding how much autonomy to give: **is the action reversible?**

If the AI saves a draft that someone can edit later, you can afford more autonomy. If the AI sends an email, deletes a record, or posts publicly, the stakes are higher and the human checkpoint becomes more important. Irreversible actions need more oversight.

## Common pitfall: review steps that don't work

Putting a human in the loop only works if the human actually reviews. If the design makes it too easy to skip — approve button right next to the output, no friction, no diff view — the human checkpoint exists on paper but not in practice.

The design of the review step matters as much as its existence. A good review step shows the human what changed, highlights what's uncertain, and makes it easy to catch problems. A bad review step puts a green "Approve" button next to a wall of text and hopes someone reads it.

## How oversight changes based on your system

**🧱 Basic and 💬 Session** — Guardrails (format checks, content filters) can apply, but human-in-the-loop oversight doesn't come into play here.

**🔧 Action** — The AI is doing things, not just talking. Guardrails can validate parameters before a tool is called and check results before they're used. Human-in-the-loop decides which actions the AI can take on its own and which require approval.

**⛓️ Chain** — The AI's output flows through steps you might never see. Guardrails between steps can validate format, check for hallucinated values, and catch errors before they propagate. Human checkpoints at key stages catch the subtler problems that automated checks miss.

**🤖 Autonomy** — The AI is making decisions about what to do next. Guardrails set the boundaries — what tools are available, what actions are off-limits, what budgets apply. Human checkpoints define what the agent can do on its own, what requires approval, and what the kill switch looks like.

---

[← Context Management](02-context.md) · [Evaluation & Observability →](04-evaluation.md)
