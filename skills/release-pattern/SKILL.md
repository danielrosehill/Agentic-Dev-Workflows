---
name: release-pattern
description: Set how releases happen this session: sprint (hold until I say), release-to-test (release, then test on device), or custom.
---

# Release pattern

For repos that ship releases (often through CI: tag or version bump, build, publish). At the
start of a session the user says how releases should go, for example:

- "Keep going until I'm ready to do a new release, then we'll do that." (**sprint**)
- "I'm using the release to test on my own device, so release each fix." (**release-to-test**)

This skill turns that into an explicit pattern the agent follows for the rest of the session.
To keep it beyond the session, run `pin-release-pattern` afterwards.

## Inputs

- What the user said about releases, if anything.
- The repo's release mechanism: read it, don't guess (CI workflow files, release scripts,
  version file, tag convention, the repo's own CLAUDE.md or contributing docs).

## Steps

1. **Check for a pinned pattern.** Look for a `## Release pattern` section in the repo's CLAUDE.md
   (or AGENTS.md). If one exists and the user hasn't said otherwise, follow it and say which in one line.
2. **Find the release step.** Identify exactly what counts as "a release" in this repo: which
   action triggers CI to build and publish (tag push, version bump on main, a release command).
   Write it down in one line. Plain commits and pushes that don't trigger it are not releases.
3. **Match the user's words to a preset** in `patterns/`:
   - `patterns/sprint.md`: hold releases until the user says the sprint is done.
   - `patterns/release-to-test.md`: every testable change goes out as a release, and the user tests
     the released build on their device.
   If neither fits, write a custom one with the same fields (see "Custom" below). If the user said
   nothing, ask once which they want, with sprint as the default suggestion.
4. **Confirm in two or three lines**: the pattern, what counts as a release here, and when the next
   release will happen. Don't ask for approval again; the user's words were the approval.
5. **Follow it for the rest of the session.** Before any release step, re-check the pattern.
6. **Offer to pin it** in one line: "Pin this to the repo's CLAUDE.md so future sessions follow it?"
   On yes, run `pin-release-pattern`.

## Custom

A custom pattern answers the same questions as the presets:

| Field | Question |
| --- | --- |
| Trigger | What makes a release happen (user's word, a list of items done, a date, every change)? |
| Between releases | What happens to finished work (commit and push? branch? local only?) |
| Testing | Is the release tested before, or is the release itself the test build? |
| Versioning | Which part of the version moves (patch per test build, minor per sprint, the repo's own scheme)? |
| Hotfix | When may a release go out early? |

## Notes

- Committing and pushing are separate from releasing. Under both presets, finished work is still
  committed; only the release step follows the pattern.
- A repo's own docs about *how* to release (commands, signing, store upload) always apply. This
  skill only decides *when*.
