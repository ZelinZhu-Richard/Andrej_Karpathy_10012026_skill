---
name: explainer-video
description: Make a 3Blue1Brown-style animated explainer video with Manim, delivered as a silent video plus a timed narration script. Use when the user asks for an explainer video, animated explanation, 3b1b-style video, or a Manim animation of a concept, derivation, or algorithm.
---

# Explainer video

Output: a silent animated video and a narration script timed to it. Do not generate audio or use text-to-speech. The user reads or records the narration, so the script is a main deliverable, not a by-product.

"3b1b style" means the visual grammar: dark canvas, color-coded math, objects that build and transform smoothly, narration that leads the eye. It does not mean copying Grant Sanderson's animations or his pi creature characters. Make original visuals.

## 1. Find the one idea

- **Viewer**: who, and what they already know. Default: smart, new to this topic.
- **Takeaway**: one sentence the viewer should be able to say afterward.
- **The aha**: the visual move that makes it click. Usually one of: watch what changes while something else stays fixed; see the same thing two ways; push a parameter past a limit and watch it break.
- **Misconception**: the most common wrong belief, so the video can show why it is wrong.
- **Encoding**: which quantity maps to position, color, size, or motion. Each color carries one meaning for the whole video.

If the user gives source material, read all of it first and use its real equations, numbers, and names. Label toy numbers as toy numbers. Check every equation and claim, because a beautiful wrong video teaches the wrong thing fast.

## 2. Write the script (the source of truth)

- Default length: 60 to 180 seconds. Default pace: 150 words per minute (2.5 words per second), so 150 to 450 words. If the user gives their own reading pace, use it.
- 4 to 8 scenes. For each scene, write the narration with beat markers, then list what each beat shows.
- A beat marker (`[B1]`, `[B2]`, ...) goes in the narration exactly where its visual should happen:

  > Gradient descent starts at a random point [B1]. The gradient points uphill [B2], so we step the other way [B3].
  >
  > B1: a dot appears on the loss curve. B2: an arrow from the dot points uphill. B3: the dot moves a small step downhill.

- Write for the ear: short sentences, no parentheses, symbols spelled out ("x squared", "the sum from i equals one to n"), acronyms written as they are spoken.
- Show the user the scene list (one line per scene), then continue. The low-quality preview render is the checkpoint for changes.

## 3. Compute the timing from the script

The video and the script must agree, so derive both from the same numbers. Write a small Python helper that:

- counts the words in each scene's narration (markers excluded),
- sets each beat time to (words before the marker) / words per second,
- sets each scene duration to (total words) / words per second, plus a 1-second pause at the end,
- writes `timing.json` with each scene's duration and beat times. Scene start times are the running sum of durations.

The Manim code and the script export both read `timing.json`. Never time one by hand and the other by formula.

## 4. Animate with Manim

- Use Manim Community Edition (`pip install manim`). It needs ffmpeg, and a LaTeX install for `MathTex`. Grant Sanderson's own `manimgl` has a different API. Do not mix the two; examples recalled from memory often do, so check imports and method names.
- If LaTeX is missing and cannot be installed, use `Text` for labels and keep the math simple.
- One `Scene` class per storyboard scene, all in `scenes.py`. Each scene loads its beat times from `timing.json`. Before each beat, `self.wait()` until that beat's time. Give each beat's animation a `run_time` no longer than the gap to the next beat. End with a `self.wait()` that brings the scene to its full duration.
- Style:
  - Dark background, light text, one accent color per concept, kept the same through the whole video.
  - Build objects step by step (`Create`, `Write`, `FadeIn`). Change them with `Transform` or `TransformMatchingTex`, so the viewer sees what became what.
  - Point at what the narration discusses at that moment with `Indicate`, `Circumscribe`, or a color change.
  - At most about 3 things moving at once. Hold still for a beat after the aha.
  - The narration carries the sentences. The screen shows equations, shapes, and short labels for key terms, never paragraphs.
  - `FadeOut` objects you no longer need, so scenes do not fill with leftovers.
  - Keep everything inside the frame (about 14.2 by 8 units). Use `.scale_to_fit_width()` and `.to_edge()` for long equations.
- Preview: `manim -ql scenes.py Scene1`. Final: `manim -qh scenes.py Scene1` (1080p). On slow machines, use `-qm` (720p). Output goes to `media/videos/scenes/<quality>/`.

## 5. Assemble and export

- Join the scene videos with a list file that has one line per scene, such as `file 'Scene1.mp4'`:
  ```
  ffmpeg -f concat -safe 0 -i list.txt -c copy explainer.mp4
  ```
- Export `script.md` from the same timing data. For each scene: a heading with its start and end time (mm:ss), then the narration with each marker replaced by a cue that gives its time and visual, for example `[0:07 arrow appears]`. State the assumed pace at the top, so the reader knows how fast to read.
- Offer captions (`.srt`) in one line. Do not make them by default.

## 6. Verify

- Pull a frame just after each beat and look at it: `ffmpeg -ss <seconds> -i explainer.mp4 -frames:v 1 beat1.png`. Look for overlapping text, objects cut off at the edge, text too small to read, and leftovers from earlier beats.
- Check that the video length (`ffprobe -v error -show_entries format=duration -of csv=p=0 explainer.mp4`) matches the total in `timing.json`.
- Read the script once more. Split any sentence that is hard to say in one breath.

## 7. Deliver

- `explainer.mp4`, `script.md`, `scenes.py`, and `timing.json`, with one sentence on what the video shows, its length, and the pace assumed.
- If the user later says their reading pace is different, or gives a recording, re-time: update `timing.json` (from the new pace, or from each scene clip measured with `ffprobe`), re-render only the scenes that changed, and re-export the script.
