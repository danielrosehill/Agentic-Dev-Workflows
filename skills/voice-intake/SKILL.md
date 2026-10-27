---
name: voice-intake
description: Fetch a development voice note (saved location, path or Drive link), transcribe it, file the transcript in the repo, then hand off.
---

# Voice intake: voice note in, transcript in the repo, next skill out

The user records a voice note about the project, then says something like "grab the latest voice
note", "I put a note in the repo", or "here's a Drive link". This skill only **gathers**: it gets the
audio, transcribes it, and files the transcript in the repo. The work happens in whichever skill
comes next: `sprint-decomposition`, `ui-friction`, a bug-capture skill, a spec skill.

## Inputs

- Where the note is: a local path, a file already in the repo, a Drive (or other cloud) link, or
  nothing, meaning "the newest one in my usual place".
- Optionally what it is for ("these are this sprint's changes", "spec for the export feature").

## Saved settings

Per-user, outside any repo: `${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/agentic-dev-workflows/config.json`, under the key
`voice_intake`. Format and how
to fill it: [references/settings.md](references/settings.md). It holds where the user keeps
development voice notes, which transcription route to use, and whether audio goes into repos.

- Read it at the start of every run.
- If it is missing and the run needs it (no explicit location given, or no transcription route
  known), ask the questions for the missing fields only, in one message, and write the answers.
- "My voice notes are in X now", "use Y for transcription" and the like: update the file, confirm
  in one line.

## Steps

1. **Find the note.**
   - Explicit path or link: use it.
   - Otherwise, list the configured locations, newest first. Pick the newest note not already
     processed in this repo (match `source.json` files under `planning/voice-notes/`). If more than
     one new note looks relevant, list them (name, date, length) and ask which.
   - Never move, rename or delete the original where the user keeps it.
2. **Fetch** into a temporary working folder, not the repo yet. For Drive and other cloud links,
   use whatever access the session has (an MCP server, `rclone`, `gdown`, a browser). Record the
   source location and the recording date (file created time, else modified time).
3. **Transcribe** with the configured route (see settings). If the route uploads audio to an
   outside service, say so in one sentence before sending. Pass expected names as hints where the
   route supports it: project name, product and library names from the repo's README. Keep the
   output as is: this is the raw transcript.
4. **File it** in the repo:

   ```
   planning/voice-notes/YYYY-MM-DD-<slug>/
     transcript-raw.txt     the transcription output, unedited
     transcript-clean.md    lightly cleaned (below)
     source.json            provenance
     voice-note.<ext>       only if settings say to commit audio
   ```

   Date is the recording date. Slug from the note's topic, a few words. If the repo already keeps
   notes elsewhere (`context/voice-notes/`, `docs/notes/`), use that instead.
5. **Clean lightly.** `transcript-clean.md` keeps the user's words, first person, every point, in
   order. Remove fillers, false starts and repeats. Resolve self-corrections to what they settled
   on. Fix obvious mishearings (names of products, libraries, files in the repo) and list each fix
   under `## Corrections` at the end. Add paragraphs and, for a long note, a few headings. **No
   summarising, no reordering, no added content.**
6. **Commit** (`Voice note YYYY-MM-DD: <slug>`) and push, so the transcript is the durable source
   before anything acts on it. Skip if the repo gitignores `planning/`.
7. **Hand off.** If the user said what the note is for, run that skill now on
   `transcript-clean.md`. Otherwise read the note and propose one route in one line, then run it on
   a yes:

   | The note is mostly | Hand off to |
   | --- | --- |
   | A list of changes for a sprint (bugs, features, docs, ideas mixed) | `sprint-decomposition` |
   | One or more bugs | the repo's bug-capture skill if it has one, else `sprint-decomposition` |
   | One UI friction point | `ui-friction` |
   | A new project or feature described end to end | a spec skill if available (e.g. `spec-starter`), else ask |
   | Debugging observations ("I tried X, then Y happened") | keep as context; start debugging from it |
   | None of these | report the transcript path and ask |

   A note that mixes kinds (a spec plus five bugs) goes to `sprint-decomposition`, which splits it.

## source.json

```json
{
  "source": "<path or URL where the note was found>",
  "file_name": "<original file name>",
  "recorded": "YYYY-MM-DD",
  "duration_s": 0,
  "transcription": {"route": "<tool, CLI or service>", "model": "<if known>", "hints": []},
  "audio_committed": false,
  "processed": "YYYY-MM-DD",
  "handed_off_to": "<skill name, or null>"
}
```

## Notes

- Several short notes recorded as one session: transcribe each, file them in one folder as
  `transcript-raw-1.txt`, `-2`, …, and one `transcript-clean.md` with a heading per note.
- The raw transcript is never edited. If the clean one is wrong, fix the clean one.
- Report: the transcript path, length of the note, any corrections worth checking, and what it was
  handed to. Nothing else.
