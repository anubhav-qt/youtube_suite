# First workflow

The first test video proves the editor and the harness end to end ([roadmap step 1](roadmap.md#1-define-the-first-workflow)).
Decisions were taken one at a time with the creator on 2026-11-12, and redecided for the channel on 2026-11-13.

## Shape

**A song-driven animated piece in 16:9, set to one full Jello song** (the creator, 2026-11-13).
The channel is cinematic 2D animated stories set to the creator's Jello songs ([project brief](project-brief.md#channel-direction)).

- Inputs: one Jello song with its song file, generated test scenes, and a frozen edit list.
- Output: 1920x1080, 30 fps, H.264 video.
- It exercises cuts, trims, transitions, keyframes, compositing, titles, text cues, preview, undo, save and reopen, and export.
- That is the list roadmap step 2 asks Kdenlive to prove, so one video checks all of it.
- Narration stays an app feature ([ADR 0008](adr/0008-narration-through-the-harness.md)); it is not part of this test.

Superseded on 2026-11-13: a 60 to 90 second narrated explainer (the creator, 2026-11-12). Alternatives considered then: a vertical Short under 60 seconds, and a trimmed talking-head take.

## Inputs

**The song: a Jello export with its tempo map** (the creator, 2026-11-13).

- One of the creator's Jello songs, rendered whole to WAV.
- Beat times come from the song file, not from audio analysis, so they are exact.
- The song file and the WAV are kept by their hashes, and a rerun reuses them.

Jello facts, read from its source at `96b2412` on 2026-11-13. Nothing was run.

- A song is a TOML text file ([Jello ADR 0038](https://github.com/anubhav-qt/jello/blob/main/docs/adr/0038-song-files.md)).
- Each section stores its bars and a whole-number tempo (`SongSection` in `song_file.rs`), so tempo can change between sections.
- Every bar has 4 beats (`BEATS_PER_BAR` in `engine.rs`).
- A whole-song render exists (`render_song` in `commands.rs`).
- Not yet verified: that the render's beats land where the section bars and tempos predict. Measure this first; if it fails, Jello needs a tempo map export.
- On 2026-11-13 the only saved song, `Untitled 1`, had sounds and tracks but an empty arrangement and no chords, so the test song is not made yet.

Alternatives considered: a Jello export with beats detected by analysis (approximate), and a generated test track (does not prove the Jello hand-off).

**Length: the full song** (the creator, 2026-11-13).

- The video runs from the song's first beat to its end, so its length is the song's length.
- This replaces the 60 to 90 second length.
- A full song means many more cuts than a fixed list of nine items, so each check loops over every planned item, not only a fixed few.
- The hand build in roadmap step 2 is larger. Whether it is typed in by hand or imported is decided with the edit (question 11).

Alternatives considered: a 60 to 90 second excerpt (fewer cuts, but not how real videos will be made).

**Pictures: generated test scenes** (the creator, 2026-11-13).

- A script makes plain scenes at 1920x1080. Each shows its scene ID, and moving scenes also show a frame counter.
- Code reads the ID and counter in the export, so it knows exactly which picture and frame is on screen.
- Kdenlive animates them with keyframes; no separate animation renderer is needed yet.
- Real art from our 2D asset set replaces them in roadmap step 5, once the renderer is chosen.
- Each file's hash is recorded, and the whole pack is rebuilt from the script, not stored in git.

Alternatives considered: stills from our existing 2D assets (closer to the channel, but checks need markers added), and waiting for real animated clips (blocks step 2 until step 5).

Gap: real 2D art is not tested in this workflow. Revisit it in roadmap step 5.

Superseded on 2026-11-13: the narrated explainer used generated narration from Kokoro-82M, a generated music bed, and captions from the script (the creator, 2026-11-12).

## The edit

**No cut rule: cuts follow the music, the story, and the direction** (the creator, 2026-11-13).

- A cut can fall anywhere: on a beat, between beats, or several times within one bar.
- Nothing in the editor or the harness may force cuts onto bars, beats, or a fixed count.
- Beat times from the song file are a guide for direction and snapping, not a constraint.
- The test still uses one frozen edit list, so the checks compare the export against exactly what that list says.
- The test list must include cuts off the beat and more than one cut within a single bar, so neither case is missed.

Alternatives considered: a cut at every section start and every 4 bars, and a cut on every bar. Both impose a rule the channel does not want.

**Claude writes the frozen test edit list** (the creator, 2026-11-13).

It is a test of the editor, not a creative piece. Its design is below.
Exact times are filled in once the test song exists, since they depend on its tempo, sections, and length.
The list is saved as data next to the brief, and both builds and every check read it.

1. The song starts at frame 0 on audio track 1 and plays to its end, unchanged in level.
2. A title card over the song's opening, fading in.
3. Scenes on video track 1 for the whole song, alternating stills and moving scenes, with hard cuts by default.
4. Cut placement covers every case: on a downbeat, on an off-beat, at a time between beats, and a burst of at least three cuts inside one bar.
5. Each moving scene starts partway into a longer source, not at its first frame.
6. One crossfade and one dip to black between scenes. Every other change is a hard cut.
7. Keyframes: one slow zoom, one pan, and one opacity fade, each on a different scene.
8. One text overlay on video track 2 over a scene, to prove compositing.
9. A few story text cues on Kdenlive's subtitle track, also exported as an SRT file. Lyrics replace them if a later song has words.
10. A 2 second fade to black over the song's last 2 seconds.

Times are planned in seconds from the song file, then placed on the nearest frame at 30 fps.
For example, at 85 BPM a beat is 0.706 seconds, or 21.18 frames, so most beats fall between frames. The checks therefore compare against the planned frame, not the raw beat time.

Alternatives considered: the creator directs a shot plan that Claude turns into the list (closer to production, but slower to the first hand build).

Superseded on 2026-11-13: the narrated explainer's nine-item list, with cuts at sentence boundaries, a narration track, and a music bed (the creator, 2026-11-12).

## Text-to-speech facts

Read from the sources on 2026-11-12. Nothing has been installed or run.
Kept for the narration feature ([ADR 0008](adr/0008-narration-through-the-harness.md)); the song-driven test does not use speech.

- Kokoro-82M: weights and code are Apache-2.0. v1.0 (2025-01-27) has 8 languages and 54 voices. A Python library on PyTorch, with Windows and Apple Silicon notes. No word timestamps or determinism are documented.
- Piper: now developed as `piper1-gpl`, licensed GPL-3.0. Local, using espeak-ng. No release or platform list on its page.
- Gemini TTS: `gemini-3.8-flash-tts` and `gemini-3.8-flash-lite-tts` are current; older ones are previews. Output is 24 kHz mono 16-bit PCM WAV. No seed, determinism, or word timestamps are documented.
- None documents repeatable output, so a rerun reuses the accepted narration file by its hash ([project brief](project-brief.md#reproducible-production)). Whether a fresh generation matches is measured, not assumed.

Sources: [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M), [kokoro library](https://github.com/hexgrad/kokoro), [piper1-gpl](https://github.com/OHF-Voice/piper1-gpl), [Gemini speech generation](https://aistudio.google.com/docs/speech-generation).

## Acceptance checks

**Exact checks against the frozen edit list** (the creator, 2026-11-12). Code runs every check on the export.
**The song keeps Jello's mastered level; the editor never normalizes it** (the creator, 2026-11-13).
The table was rewritten for the song piece on 2026-11-13 from those decisions.
Every scene shows its ID, and moving scenes show a frame counter, so code can read which scene and source frame appear in the export.

| Area | Passes when |
| --- | --- |
| Format | MP4 with fast start; H.264 High Profile, progressive, 4:2:0; 1920x1080 at 30 fps; about 8 Mbps; AAC-LC stereo, 48 kHz, 384 kbps. |
| Duration | The song's length rounded to the nearest frame, within 1 frame. Audio and video streams end within 1 frame of each other. |
| Cuts and trims | At every planned cut, the frame before shows the outgoing scene and the frame at the cut shows the incoming scene's ID and planned source frame. This covers on-beat, off-beat, between-beat, and burst cuts alike. |
| Crossfade and dip | Frames inside each blend both scenes, or reach black at the dip's middle. Each starts and ends within 1 frame of plan. |
| Keyframes | Zoom scale and pan position at their first and last frames are within 1% of the keyframes. Opacity is within 2% of plan at the fade's start, middle, and end. |
| Overlay | The text overlay appears at its planned position on exactly its planned frames, and nowhere else. |
| Song | Matching the export against the song WAV finds it starting at frame 0, within 5 milliseconds. If the WAV is not 48 kHz stereo, the conversion is recorded. |
| Loudness | Integrated loudness is within 0.5 LU of the song WAV's, and true peak is no more than 0.5 dB above the WAV's. Both values are recorded. No target is imposed. |
| Text cues | The SRT has one cue per planned cue, text matching the list exactly, each start and end within 1 frame of plan. |
| Ending | The picture fades to black over the last 2 seconds, and the last frame is black. The song is not faded. |
| Save and reopen | The reopened project has the same timeline, and a second export decodes to the same frames. |
| Undo | Undoing every step returns to the empty project. Redoing returns to the same export. |
| Speed | Export time is recorded. No limit is set until it has been measured. |

The 0.5 LU and 0.5 dB allowances cover AAC encoding, which can shift loudness and add peaks. They are first guesses; tighten them once the first export is measured.

Each check must fail when one deliberate mistake breaks it ([testing](testing.md)).

Alternatives considered: format checks only (a cut off by frames would pass), and a frame-by-frame reference video (breaks on any Kdenlive or encoder change).

## Creator review

**A checklist in the app** (the creator, 2026-11-12). Each item is marked pass or needs change, with a note.
Items rewritten for the song piece (the creator, 2026-11-13):

- Cuts land at good moments for the music.
- The crossfade, dip to black, zoom, pan, and opacity fade look smooth.
- The title card and the overlay text are legible.
- Text cues are readable: size, placement, and line length.
- The song sounds as it did in Jello, with no change in level or quality.
- The ending feels finished.

The review is a versioned record tied to the export's hash.
A needs change note reaches that video's chat as a steer ([ADR 0006](adr/0006-long-running-chats-and-the-improvement-loop.md)).

Alternatives considered: free notes (no record of what was checked), and a 1 to 5 score per item (little to compare against).

## Two builds

**By hand first, then by the harness** (the creator, 2026-11-12).

1. Roadmap step 2: the edit list is built by hand in unchanged Kdenlive, with exact timecodes typed in. The export must pass every code check. This proves Kdenlive itself can do it.
2. Roadmap step 3: the Video chat builds it from one instruction and the brief. It places each item through the project gateway with undo, previews, exports, and runs the checks. The creator then reviews with the checklist.

Both exports must pass the same checks. A failure in the harness build then points to the harness, not Kdenlive.

Alternatives considered: harness only (a failure could be either), and the harness planning while the creator applies it (never proves the harness can edit).

## Where it ends

**At export in steps 2 and 3, then a private upload in step 4** (the creator, 2026-11-12).

- In roadmap step 4, the same export is the channel gateway's first test.
- It is uploaded privately with title, description, tags, chapters, the SRT captions, and a thumbnail.
- The test covers quota tracking, the resumable upload, and the approval prompt, which the creator refuses.
- It never goes public. It is generated test content, and it is marked as a test so grades never use it as a baseline.

Alternatives considered: export only (step 4 would test with a file not yet known to be good), and going public (a test video on the channel, skewing grades).

## YouTube upload settings

YouTube's recommended upload settings, read on 2026-11-12 ([YouTube Help](https://support.google.com/youtube/answer/1722171?hl=en)):

- MP4 with no edit lists and the moov atom at the front (fast start).
- H.264, progressive, High Profile, CABAC, 2 consecutive B-frames, closed GOP of half the frame rate, 4:2:0 chroma.
- Frame rate as recorded. For 1080p at 24 to 30 fps, 8 Mbps is recommended.
- Audio: AAC-LC (or Opus), stereo, 48 kHz, 384 kbps for stereo.

## Open questions

Asked one at a time, in this order.

1. ~~Where the test inputs come from.~~ Decided: all generated, with text-to-speech narration.
2. ~~Which text-to-speech engine.~~ Decided: Kokoro-82M, local. Whether its output repeats exactly is measured when it is first run.
3. ~~The edit itself.~~ Decided: the fixed edit list above.
4. ~~How Kokoro runs inside the app.~~ Decided: a pinned Python worker owned by the Rust narration gateway ([ADR 0008](adr/0008-narration-through-the-harness.md)).
5. ~~Acceptance criteria the code checks.~~ Decided: the exact checks above.
6. ~~What the creator reviews.~~ Decided: the checklist above.
7. ~~How the harness takes part.~~ Decided: two builds, by hand then by the harness.
8. ~~Where it ends.~~ Decided: at export, then a private upload in step 4.

Reopened on 2026-11-13 for the song-driven piece, asked one at a time, in this order:

9. ~~Where the song comes from, and how its beat times are known.~~ Decided: a Jello export with its tempo map, from the song file.
10. ~~The length and the pictures.~~ Decided: the full song, with generated test scenes.
11. ~~The edit.~~ Decided: no cut rule, and Claude writes the frozen test list to the design above.
12. ~~The acceptance checks that change.~~ Decided: the song keeps Jello's level, and the table above is rewritten for the song piece.
13. ~~The creator review items that change.~~ Decided: the checklist above.

Every question for the song-driven first workflow is answered. Exact times wait for the test song.
