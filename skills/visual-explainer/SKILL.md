---
name: visual-explainer
description: Build a custom diagram or interactive HTML explainer that makes one idea click. Use when the user says visualize, explain visually, diagram this concept, interactive explainer, explorable, or make it in HTML. For animated videos use explainer-video. Not for plain data charts or UI design.
---

# Visual explainer

Code is now cheap, so a custom artifact that exists only to make one idea land for one person is worth building and then throwing away. The goal is understanding, not a portfolio piece. Every choice below serves one test: after this, can the viewer explain the idea to someone else?

## Step 1: Find the one idea (before any format or code)

Decide these first, in your reasoning:

- **Viewer**: who, and what they already know. Default: smart, new to this topic.
- **Takeaway**: one sentence the viewer should be able to say afterward.
- **The aha**: the visual move that makes it click. Usually one of: watch what changes while something else stays fixed; see the same thing two ways; push a parameter past a limit and watch it break.
- **Misconception**: the most common wrong belief, so the explainer can show why it is wrong.
- **Encoding**: which quantity maps to position, color, size, or motion. Each color carries one meaning and keeps it in every view.

If the user gives source material (paper, code, notes, an earlier answer), read all of it first. Use its real equations, numbers, and names. Do not invent data to make a prettier picture. If you use toy numbers, label them as toy numbers.

Correctness comes first, because a beautiful wrong explainer teaches the wrong thing fast. Check every equation and claim. For factual or current claims, verify with search and list the sources in a footer.

## Step 2: Pick the format

| Format | Best for | Cost to build and fix |
|---|---|---|
| Diagram | Structure, flow, architecture, how parts connect | Minutes |
| Interactive HTML page | "What happens if I change X?", simulations, multi-part explainers to explore | Tens of minutes |
| Animated video | Derivations and transformations that unfold in time; something to share | Hours. Use the explainer-video skill if it is available |

- If the user names a format, use it.
- If not, pick the cheapest format that can carry the aha, then offer the next format up in one line at the end. An interactive page often teaches more per hour of build time than a video, because the viewer can poke at it.
- A page can embed diagrams.

## Diagram

- If the environment has an inline visual tool, use it for a quick diagram in the chat. Otherwise write SVG (hand-placed, full control) or Mermaid (flowcharts, sequence diagrams) inside a small HTML page.
- The title states the claim, not the topic: "Attention lets each token read every other token", not "Attention".
- About 12 boxes maximum. If you need more, make one overview diagram and one zoomed-in detail diagram.
- Flow direction follows cause and time (left to right, or top to bottom). Label arrows with verbs.
- One highlight color for the path that matters; everything else neutral grey. Add a legend if color encodes anything.
- Verify: render to PNG and look at it (headless browser screenshot, or `rsvg-convert` / `cairosvg` for SVG). Fix overlapping labels, cut-off text, and crossing arrows.

## Interactive HTML page

- One self-contained `.html` file with inline CSS and JS. CDN libraries are fine: KaTeX for math, D3 or Plotly for plots, Three.js for 3D. If the environment has an artifact publishing tool, publish there and follow its design guidance. Otherwise deliver the file.
- Page shape:
  1. **Hook**: the question the page answers, in one or two sentences.
  2. **Core interactive**: one main visual with 1 to 3 controls. The defaults must show the aha on load, before the viewer touches anything.
  3. **Live annotation**: text and numbers that update with the controls and say what just changed and why.
  4. **Guided experiments**: 2 to 4 "Try this" prompts that walk the viewer to the aha and into the misconception ("Set the learning rate above 1.0. Why does the loss explode?").
  5. **Check yourself**: one question with a hidden answer the viewer can reveal.
  6. **Recap**: three sentences.
- Compute the real thing in JS (real gradient descent, a real convolution, a real order book), not a faked curve. Faked visuals break when the viewer pushes a slider to an edge.
- Sliders show their current value. Add a reset button. Animate transitions briefly (about 200 to 400 ms) so the eye can follow the change.
- Keep prose short. STE-lite style (the ste-writer skill) works well for the text blocks.
- Make it work on a phone: text width about 70 characters maximum, no horizontal scroll, controls large enough to touch.
- Keep state in JS variables. Do not depend on localStorage, because some hosts block it.
- Verify: load the page in a headless browser if one is available (for example Playwright), take a screenshot, and read the console for errors. Move each control to its minimum and maximum and look again. If no browser is available, at least run `node --check` on the extracted script.

## Delivery
- Give the artifact with one sentence on what it shows and what to try first.
- Offer the next format up only if it would add real understanding.
- Include the source files so the user can change one part and rebuild only that part.
