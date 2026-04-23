---
name: start-v3-council
description: Launch an LLM-Council-V3 run — voice-first, batch-oriented council. User supplies an MP3 braindump; pipeline transcribes, parses into shared Context + Q1..Qn, fans each question out to a council of models via OpenRouter, and emits a typeset Typst PDF. Use when the user says "run the V3 council", "process this braindump", "/start-v3-council", or provides an audio recording or transcript in the LLM-Council-V3 repo.
---

# Start a V3 Council (Voice-First, Batch)

You are operating inside the `LLM-Council-V3` repo. This council variant differs from the others: the input is a **voice braindump**, not a typed question, and a single run answers **multiple questions** extracted from the braindump.

## Status

The pipeline is a scaffolded template. If the repo does not yet contain `backend/` (check first), this skill's job is to **tell the user clearly that V3 is still at whiteboard / README stage**, and point them at the Homehunter or Decide skills if they want an immediately-runnable council.

If `backend/` is present, proceed below.

## Preconditions

1. `OPENROUTER_API_KEY` set.
2. A transcription provider key — either Gemini (`GEMINI_API_KEY`) or an OpenAI-compatible Whisper endpoint, depending on repo configuration.
3. `uv sync` if the venv is missing.
4. `typst` CLI installed if the user wants the final PDF in the same run.

## Step 1 — Capture the braindump

Accept any of:

- A path to an MP3/WAV/M4A file.
- An already-transcribed markdown file (skip straight to the parse step).
- A directly pasted transcript.

If the user offers to record now, suggest a free 3–10 minute recording — the pipeline is designed for unstructured rambling.

## Step 2 — Run the pipeline

When implemented, the expected invocation is:

```bash
uv run python -m backend.main --audio path/to/braindump.mp3
# or
uv run python -m backend.main --transcript path/to/notes.md
```

Explain each stage to the user as it runs:

1. **STT** — audio → raw transcript.
2. **Cleanup** — filler removal, paragraphing.
3. **Parse** — agent extracts shared Context + Q1..Qn.
4. **Actuator/Runner** — for each question, fan out to the council of models.
5. **LLM Council** — per-question: per-model answer → peer review → Chairman synthesis.
6. **Aggregator** — gathers all Chairman outputs.
7. **Typst** — renders the aggregate to a typeset PDF report.

## Step 3 — Surface the output

When complete:

1. Show the **parsed structure** first — the user should confirm the agent extracted the questions they intended. If not, offer to re-parse with a hand-edited context/question split.
2. Summarise each question's Chairman synthesis in one line; offer to expand on request.
3. Report the PDF path.

## Step 4 — Iterate

Offer to:

- Re-parse the transcript if the question extraction was off.
- Re-run a single question (not all of them) with a different council roster.
- Edit `prompts/` for the parser, council, or Chairman.

## Failure modes

- Backend not implemented yet → say so clearly; do not fake it.
- Transcription fails → surface the provider error; suggest a smaller chunk or a different provider.
- A single question's council fails → report which, proceed with the rest, flag in the aggregator output.

## Out of scope

- Does not clean or produce audio outputs — this is input-only for voice.
- Not for single-question use — if the user has a single question, point them at `LLM-Council-Template`, `LLM-Council-Grounded`, or `LLM-Council-Decide`.
