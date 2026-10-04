# Claude skills for understanding LLM output

Three [Claude skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that turn model output into formats that are easier to understand. They follow the ladder from Andrej Karpathy's post on reading LLM output: clean writing, then diagrams, then interactive web pages, then explainer videos.

| Skill | What it does | Example prompt |
|---|---|---|
| [`ste-writer`](skills/ste-writer/SKILL.md) | Writes or rewrites text in ASD-STE100 Simplified Technical English, strict or a softer "80%" STE-lite | "Explain conformal prediction in STE-lite" |
| [`visual-explainer`](skills/visual-explainer/SKILL.md) | Builds a diagram or an interactive HTML explainer around one "aha" | "Make an interactive explainer of why attention is quadratic in sequence length" |
| [`explainer-video`](skills/explainer-video/SKILL.md) | Makes a 3Blue1Brown-style Manim video plus a narration script timed to it (no audio) | "Make a 2-minute 3b1b-style video on gradient descent" |

## Design notes

- **STE levels.** Strict applies the full spec. Lite keeps every structure rule (sentence length, active voice, one meaning per word) and relaxes only the vocabulary. Technical terms stay precise in both.
- **Cheapest format first.** `visual-explainer` picks the cheapest format that carries the idea and offers the next step up. An interactive page is often a better teacher than a video, because the viewer can change things.
- **Script is the source of truth.** `explainer-video` puts beat markers in the narration, computes all timing from word count, and drives both the Manim scenes and the exported script from one `timing.json`. The video and the script cannot drift apart.

## Install

**Claude Code**: copy the skill folders into your personal skills directory.

```bash
git clone https://github.com/ZelinZhu-Richard/Andrej_Karpathy_10012026_skill.git
mkdir -p ~/.claude/skills
cp -r Andrej_Karpathy_10012026_skill/skills/* ~/.claude/skills/
```

**Claude.ai**: zip one skill folder (for example `cd skills && zip -r ste-writer.zip ste-writer`) and upload it under Settings → Capabilities → Skills.

## Requirements

- `ste-writer`: none.
- `visual-explainer`: none required. A headless browser (for example Playwright) lets Claude screenshot and check its own pages.
- `explainer-video`: Python, [Manim Community Edition](https://www.manim.community/), ffmpeg, and LaTeX for math typesetting.

## Limits

- `ste-writer` does not include the official ~900-word STE dictionary, so its vocabulary is approximate. The output is "STE style", not certified ASD-STE100. For real compliance, use the specification at [asd-ste100.org](https://www.asd-ste100.org/).

## License

MIT. See [LICENSE](LICENSE).
