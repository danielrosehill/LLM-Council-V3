# LLM Council V3 — "Remix" 2

![Whiteboard sketch](whiteboard/IMG20260423113826_wb.jpg)

![LLM Council Remix 2 diagram](whiteboard/llm-council-remix-diagram.png)

A remix of Andrej Karpathy's [`karpathy/llm-council`](https://github.com/karpathy/llm-council) with a voice-first, batch-oriented twist.

## What I'm trying to build

The original LLM Council is a chat-style UI: one question in, multiple models answer, they rank each other, a Chairman model synthesises a final response.

This remix keeps the council pattern but changes the front end and the batching model. Instead of typing one question at a time, the idea is:

1. **Braindump** — I record an MP3 of myself rambling: "I have this big idea but don't know how to do a, b, c…"
2. **STT + cleanup** — audio → transcript → light cleanup pass.
3. **Parse** — an agent reads the cleaned transcript and splits it into:
   - **Context** (shared background for every question)
   - **Q1, Q2, Q3 …** (the individual questions buried in the braindump)
4. **Actuator / Runner** — for each question, fan out Context + Q to the council via [OpenRouter](https://openrouter.ai/).
5. **LLM Council** — multiple models (A1, A2, A3 …) each answer, then review each other's answers, then a Chairman model produces a final synthesised response (same pattern as Karpathy's).
6. **Aggregator** — collects all final answers across Q1…Qn.
7. **Typst** — render the aggregated Q&A into a typeset **PDF report**.

So the loop is: *ramble in, typeset PDF out — with a council debate in the middle.*

## Stack (planned)

- **API**: OpenRouter (same as upstream — one key, many models).
- **STT**: Gemini or Whisper, depending on latency/quality preference.
- **Council & Chairman**: configurable roster via OpenRouter model IDs.
- **Typesetting**: Typst for the final PDF.

## Relationship to upstream

- Upstream (`karpathy/llm-council`): interactive chat UI, one question at a time.
- This repo: voice-in / PDF-out batch pipeline, same council/chairman scoring underneath.

Not a fork — a reimplementation around a different input and output surface. Credit to Karpathy for the council pattern that sits at the heart of it.

## Diagram generation

The cleaned-up diagram above was produced by **Nano Banana 2** via the [Fal](https://fal.ai/) API, driven by my [`nano-tech-diagrams-mcp`](https://github.com/danielrosehill/nano-tech-diagrams-mcp) server (the `whiteboard_cleanup` tool, `clean_polished` style).

For reference, the prompt assembled and sent to Nano Banana 2 was:

> Take this whiteboard photograph and transform it into a polished diagram. Remove the physical whiteboard, markers, frame, and any background elements. Correct any perspective distortion so the output appears as a perfectly straight-on, top-down view regardless of the angle the original photo was taken from. Preserve all the original content, text, and diagrams. Where handwriting is ambiguous, infer the correct spelling from context rather than reproducing the raw strokes literally — for example, a word that looks like 'proxknox' should be rendered as 'Proxmox' if that is the obvious intended meaning. Render as a beautiful and polished graphic featuring clear labels and icons on a clean white background. Content should be legible and well-organized with a clear visual hierarchy. Use clean shapes, consistent line weights, and a professional color palette. The result should be visually attractive and easy to understand.
>
> The following specific terms appear in this whiteboard and should be spelled exactly as listed: LLM Council, Remix, MP3, STT, Cleanup, Text, Context, Aggregator, Typst, PDF Report, Actuator, Runner, Daniel Rosehill

Credit: **Nano Banana 2** (Google), served via **[Fal](https://fal.ai/)**.

---

Daniel Rosehill (not a bot) · [github.com/danielrosehill](https://github.com/danielrosehill)
