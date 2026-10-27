# Preset: sprint

The user's words, typically: "Let's keep going until I'm ready to do a new release, and then we'll
do that, rather than pushing out a new release every time."

| Field | Value |
| --- | --- |
| Trigger | The user says the sprint is done ("release it", "sprint's done, ship it"). Nothing else triggers a release. |
| Between releases | Commit and push finished work as usual, without triggering the release step (no tag, no version bump, no release command). Keep a running list of what is waiting to ship. |
| Testing | Tested before release, by whatever the repo uses (tests, local run, emulator, preview). |
| Versioning | One version bump per sprint, covering everything in it. |
| Hotfix | Only if the current release is broken for users (won't install, crashes on start, loses data). Say why in the same message. |

## Agent behaviour

- When a change is done: commit, push, add it to the waiting list, and say in one line that it is
  held for the sprint.
- When something would normally trigger a release (a skill or repo doc that says "release after each
  fix"): don't. Say in one line that the sprint pattern overrides it.
- When the user says the sprint is done: run the repo's release procedure once for everything on the
  waiting list, write the list into the changelog or release notes the repo keeps, and confirm the
  release built and published (check the CI run or the release itself, not just the tag push).
- Then open the next sprint with an empty list.

## Where the waiting list lives

In the session by default. If the repo has a backlog, sprint or planning file, keep it there under
a `Waiting to ship` heading so it survives the session.
