# Panel

Panel runs the multi-model setup you already use (Claude writes and Codex reviews, a council of models, best-of-n runs) and keeps score of which model's contributions actually survive on your own work: findings that get fixed, patches that pass tests, code that gets merged. Over time it uses that history to decide which models get a seat for which kind of task, instead of guessing from public leaderboards.

> **Status: early idea, looking for feedback. There is no code here yet.** If you run more than one model on the same work, I'd love to hear how you decide which ones are worth it.

## The problem

More and more developers already run models from different labs against each other, mostly for code review: one model writes, another from a different lab reviews, sometimes a third judges. The reasoning is sound. A model tends to miss its own mistakes, so a reviewer from a different family catches things the author can't see.

What almost nobody has is evidence about **which** models are earning their place on **their** code. People pick the panel from vibes and benchmarks, and they say so:

- "I don't know if Grok earns its place next to Codex." ([paddo.dev](https://paddo.dev/blog/claude-reviewing-claude/))
- A four-lab review setup reports no labeled bug set, no precision/recall, and a judge from the same family as one of the reviewers. ([Comfy](https://blog.comfy.org/p/comfy-internals-how-we-got-four-rival))

The public evidence disagrees with itself, which suggests the answer depends on the workload:

- Greptile found Opus and GPT both catch more serious bugs in each other's code than in their own. ([Greptile](https://www.greptile.com/blog/model-inversion))
- A July 2026 study found Claude reviewing Codex helped a lot, while Codex reviewing Claude made results *worse* than Claude alone. ([arXiv 2607.21656](https://arxiv.org/abs/2607.21656))
- Mixing models only beats one strong model when the step that picks the winner is good. With weak selection or plain synthesis, the diverse panel loses. ([arXiv 2603.20324](https://arxiv.org/abs/2603.20324))

So the people who'd get the most from a multi-model setup are flying blind on the one question that decides whether it's worth the cost: which seats are pulling their weight here?

## Who has it

Developers and small teams who already run cross-vendor review or model councils on real code, often with no human reviewer in the loop, and who pay 2x to 8x the model cost for it.

## How it might work (rough)

1. **Wrap what you already run.** Panel sits around your existing review or council step (Claude Code hooks, CI, a CLI) instead of replacing it.
2. **Record every contribution.** Which model said what, in which role, on which kind of change.
3. **Grade it with outcomes, not opinions.** Did the finding get fixed? Did the test that would catch it fail first? Was it a false positive the host model threw out? Did the merged code get reverted?
4. **Show a per-repo scorecard.** Per model and per task type: useful findings, false-positive rate, unique catches nobody else made, cost per useful finding.
5. **Use it.** Suggest dropping a seat that never catches anything unique, or adding one for the kind of work where you keep getting burned. Later, route automatically.

The bigger direction is a full multi-model harness that learns what each model is good at from your own results. Panel starts with the measurement, because without it the rest is guesswork.

## How existing options fall short

| Option | What it does well | What it doesn't do |
|---|---|---|
| Perplexity Model Council, OpenRouter Fusion, Karpathy's llm-council | Several models answer and a synthesizer merges them | No memory of which model was right on your work; built for research questions, not code |
| Cursor best-of-n with multi-agent judging, GitHub Agent HQ | Run several models on one task and pick or compare | The judge picks a winner per task; nothing tracks who wins over time on your repo |
| OpenAI's Codex plugin for Claude Code, PAL MCP, lots of review skills | Easy cross-vendor second opinion | No scoring of whether the second opinion was worth it |
| Not Diamond, Factory Router, LangChain's Open SWE router | Route between models, some learn from outcomes | Tuned mainly to cut cost at equal quality; pick one model per turn rather than measuring a panel |

## How to help

- **Open an issue** and tell me how you pick which models run in your setup, or why you don't think this matters.
- **Or just reply to my email** if that's how you found this.

A "no, I just run all of them and don't care who caught it" is useful too.

## License

MIT
