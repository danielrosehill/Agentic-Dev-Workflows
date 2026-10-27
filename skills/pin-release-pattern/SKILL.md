---
name: pin-release-pattern
description: Record the repo's release pattern (sprint, release-to-test, custom) in its CLAUDE.md so every future session follows it.
---

# Pin the release pattern to the repo

Writes the user's chosen release pattern into the repository itself, so it doesn't depend on any
one session or on agent memory. Every future session that reads the repo's CLAUDE.md follows it.

## Inputs

- The pattern: a preset from `release-pattern` (`sprint`, `release-to-test`) or a custom one.
  If none has been set this session, run `release-pattern` first.
- The user's own words about it, if they gave any.

## Steps

1. **Pick the file.** The repo-root `CLAUDE.md`. If the repo uses `AGENTS.md` as its main agent file
   and has no CLAUDE.md, use `AGENTS.md`. If neither exists, create `CLAUDE.md`.
2. **Replace, don't duplicate.** If a `## Release pattern` section exists, replace it whole. Never
   leave two.
3. **Write the section** using the template below. Fill it from the preset file and from what you
   found about this repo's release step. Quote the user's words and date it (YYYY-MM-DD).
4. **Commit only that file**: `git add <file> && git commit -m "Pin release pattern: <pattern>" -- <file>`,
   then push if the repo is pushed as a matter of course.
5. **Confirm in one line**: which file, which pattern.

## Template

```markdown
## Release pattern

Set YYYY-MM-DD by the user: "<their words>".

**Pattern: <sprint | release-to-test | custom>.** <one-line summary>

- **A release here is:** <the exact step that triggers CI to build and publish, e.g. pushing a `v*` tag>.
- **Trigger:** <when a release happens>.
- **Between releases:** <what happens to finished work>.
- **Testing:** <before release, or the release is the test build>.
- **Versioning:** <which part moves per release>.
- **Hotfix:** <when a release may go out early>.

Agents: follow this unless the user says otherwise in the session. A change of pattern for one
session doesn't change this section; a lasting change does, by re-running `pin-release-pattern`.
```

## Notes

- This is repo configuration, deliberately in the repo, not in an agent's memory store, so it
  travels with the code and any agent or collaborator sees it.
- Keep the section short. How to release (commands, signing, upload) belongs in the repo's
  existing release docs; this section only says when.
- To unpin, remove the section and commit.
