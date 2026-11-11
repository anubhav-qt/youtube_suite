# 0008: Narration is a harness feature, with Kokoro first and more engines later

Date: 2026-11-12

Status: Accepted by the creator.

## Context

The first test video's narration is made with Kokoro-82M ([first workflow](../first-workflow.md#inputs)).
The creator then asked for narration as a feature of the app: someone who wants a narrated video can do everything through the harness.
More text-to-speech engines will come later. Kokoro is the first.
The tools and gateways are Rust, and the harness reaches them through one tool server ([ADR 0004](0004-harness-on-a-typescript-pi-fork.md)).

## Decision

- A narration gateway in the Rust tool server is the only code that makes speech.
- The harness and the editor UI use one `narrate` tool: script text, engine, voice, and speed in; an audio file and each sentence's start and end out.
- Engines sit behind one interface. Kokoro-82M is the first. Each new engine is chosen with the creator, one at a time.
- An engine that sends the script to an outside service is an outside tool, so it is asked about separately ([harness study](../harness-study.md), Y9).
- Each narration is stored as a versioned asset: script, engine, model version, voice, settings, the audio file's hash, and sentence times.
- An accepted narration is reused by its hash. Regenerating it makes a new version; it never replaces the old one silently.
- The Video chat can write the script, narrate it, place it on the timeline, and cut pictures to its sentences, through the project gateway and its undo.
- The creator hears the narration before it is used in a video that goes public, as with any creative choice.

**How Kokoro runs** (the creator, 2026-11-12)

- A Python worker, started, watched, and stopped by the narration gateway. Nothing else talks to it.
- It runs kokoro-onnx on Kokoro's ONNX model, with misaki turning text into phonemes. No PyTorch.
- The app ships a pinned standalone Python with the worker's packages locked by version and hash.
- The model file, voice files, and package versions are pinned and recorded with every narration.
- The worker talks to the gateway over standard input and output, never a network port.
- If the worker fails, the gateway restarts it once, then reports the error. A narration is never half stored.

## Alternatives considered

- Kokoro in pure Rust: smallest, with no Python, but misaki has no known permissive Rust version, so it would need porting.
- kokoro-js in the Node harness: no Python, but its phonemizer and licence are not stated, and heavy work would sit in the harness.
- Narration only in the tool that builds test inputs: smaller, but a narrated video would need an outside tool.
- One engine wired directly into the harness: simpler now, but each later engine would change the harness.

## Consequences

- The Python worker, Kokoro's model, and misaki ship with the app on macOS and Windows. Their size and speed are measured on both before building.
- Only permissive licences ship with the app. espeak-ng is GPL-3.0, so it is not bundled; Kokoro's phonemizer misaki runs with its espeak fallback off.
- The test input builder uses the same engine, version, and voice as the app.
- Before the first public video with generated narration, check YouTube's rules on disclosing synthetic content.

## Evidence

Read on 2026-11-12; nothing installed or run.

- Kokoro-82M weights and code: Apache-2.0 ([model](https://huggingface.co/hexgrad/Kokoro-82M), [library](https://github.com/hexgrad/kokoro)).
- An ONNX version, Apache-2.0, from 86 MB (q8f16) to 326 MB (fp32), phonemized with misaki ([Kokoro-82M ONNX](https://huggingface.co/onnx-community/Kokoro-82M-v1.0-ONNX)).
- misaki: Apache-2.0, Python, with an optional espeak-ng fallback ([misaki](https://github.com/hexgrad/misaki)).
- kokoro-onnx: MIT, Python on ONNX Runtime ([kokoro-onnx](https://github.com/thewh1teagle/kokoro-onnx)).
- kokoro-js runs in Node on CPU through Transformers.js; its phonemizer is not stated on its page ([kokoro.js](https://github.com/hexgrad/kokoro/tree/main/kokoro.js)).
