---
name: complementary-plugins
description: List plugins that complement agentic-dev-workflows for platform work (Android, Linux, web), QA, specs, planning and agent habits.
---

# Complementary plugins

`agentic-dev-workflows` holds only cross-cutting workflows (see `SCOPE.md`). Platform tooling and
neighbouring workflows live in separate plugins. This skill lists them and suggests which fit the
current repo.

## Steps

1. **Look at the repo**: platform and stack (e.g. `build.gradle` → Android, `package.json` → web/Node,
   `debian/` → Linux packaging, `metadata.json` with Plasma keys → KDE), and whether it has releases,
   specs, planning docs.
2. **Suggest from the table below**: the ones that match, each with one line on why it fits this repo.
   Then mention the rest in a single line, by name.
3. **Give install commands** for the ones suggested. Check what is already installed first
   (`claude plugins list`) and leave those out.

## The list

Marketplace `danielrosehill` is `danielrosehill/Claude-Code-Plugins`
(`claude plugins marketplace add danielrosehill/Claude-Code-Plugins`).

| Plugin | Install | For |
| --- | --- | --- |
| Android skills (Google, official) | from [android/skills](https://github.com/android/skills) | Android: `android` CLI, SDK, emulator, install and run APKs, screenshots, layout dumps |
| `linux-packaging` | `claude plugins install linux-packaging@danielrosehill` | Linux: .deb and other package builds and publishing |
| `kde-plasmoid-dev` | `claude plugins install kde-plasmoid-dev@danielrosehill` | KDE Plasma widgets: scaffold, debug, package |
| `userscript-development` | `claude plugins install userscript-development@danielrosehill` | Browser userscripts |
| `dev-tools` | `claude plugins install dev-tools@danielrosehill` | Scaffold repos, multi-agent QA review (incl. a UX-friction audit agent), templatize |
| `spec-starter` | `claude plugins install spec-starter@danielrosehill` | Spec-driven start: free-form description → spec, context files, decision records |
| `planning-docs` | `claude plugins install planning-docs@danielrosehill` | Lean decision log and plans in the repo |
| `stack-evaluator` | `claude plugins install stack-evaluator@danielrosehill` | Review a repo's technology stack and propose revisions |
| `kaizen` | `claude plugins install kaizen@danielrosehill` | Incremental improvement of a mature codebase, one atomic commit at a time |
| `session-hopper` | `claude plugins install session-hopper@danielrosehill` | Hand over and resume between agent sessions |
| `skill-or-not` | `claude plugins install skill-or-not@danielrosehill` | Before a task: should this become a skill? |
| `brainstorm-solutions` | `claude plugins install brainstorm-solutions@danielrosehill` | Stuck on a blocker: capture it and research solutions |
| Document-As-You-Go | from [danielrosehill/Document-As-You-Go](https://github.com/danielrosehill/Document-As-You-Go) | Agents record what they reverse-engineer as they go |

## Notes

- Keep this table in step with the marketplace: when a plugin is renamed or retired there, update it here.
- Add a row only for a plugin that a developer working with agents would plausibly want alongside
  this one. This is not a catalogue of everything.
