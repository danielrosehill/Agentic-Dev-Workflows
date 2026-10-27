# Scope

What belongs in `agentic-dev-workflows`, and what doesn't. Read this before adding a skill.

## In scope

Workflows for developing software *with an AI agent*, that work the same whatever is being built:
Android app, Linux desktop tool, web app, CLI, library. A skill belongs here when:

1. **It describes how the user and the agent work together**, not how a particular technology works.
   "Show me options for this UI friction point, then build the one I pick" is in. "Build an APK" is not.
2. **It is platform- and stack-neutral.** It must make sense in an Android repo, a Python CLI repo and
   a website repo without changes. Platform detail, if any, comes from the repo, not the skill.
3. **It is a pattern the user repeats across projects**, and states at the start of sessions.

Current skills, by area:

| Area | Skills |
| --- | --- |
| UI and UX | `ui-friction` |
| Releases and sprints | `release-pattern`, `pin-release-pattern` |
| Discovery | `complementary-plugins` |

## Out of scope

- **Platform or stack tooling** (Android SDK, Gradle, Debian packaging, KDE plasmoids, npm publishing).
  These live in their own plugins; `complementary-plugins` points to them.
- **Code review and QA passes**: the `dev-tools` plugin.
- **Specs and planning documents**: `spec-starter`, `planning-docs`.
- **Agent habits that apply to all work, not just development** (documenting as you go, skill-or-not,
  Kaizen): their own plugins.
- **Anything tied to one person's infrastructure** (their secrets store, registries, servers, devices).
  This plugin is public and generic.

## The test for a new skill

Before adding a skill, answer all three with yes:

1. Would it read the same in an Android repo and in a web repo?
2. Is it about the working pattern between user and agent, not a technology?
3. Is it not already covered by a plugin listed in `skills/complementary-plugins/SKILL.md`?

A no on 1 or 2 means a platform plugin. A no on 3 means extend that plugin instead.
