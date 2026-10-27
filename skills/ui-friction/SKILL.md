---
name: ui-friction
description: User describes a UI/UX friction point; propose several ways to resolve it, wait for their pick, then build it. Any platform.
---

# UI friction: options first, then build

The user describes something in the interface that is awkward, slow, confusing or annoying
("I have to tap three times to get to X", "the save button is easy to miss", "the list jumps when
it loads"). The agent's job is **not** to fix it straight away. It proposes a few distinct
resolutions, the user picks or adjusts one, and only then is it built.

Works in any UI: Android, iOS, web, desktop, TUI, CLI output.

## Inputs

- The friction point, in the user's words.
- Optionally a screenshot, screen recording or the screen/component name.

## Steps

1. **Locate it.** Find the screen, component or command the user means in the repo. Read the code
   that renders it and any handler behind it. If you can't tell which one they mean, ask one question,
   then stop.
2. **Restate the friction in one line**, as the user experiences it, not as the code does
   ("Getting to Settings takes three taps from the main screen"). If you inferred something
   (e.g. which screen), say so.
3. **Propose 3 to 5 resolutions** that differ in kind, not just in detail. Draw on the levers below,
   and include at least one small change and one bolder one. For each, give:
   - **What changes** for the user, in one or two lines.
   - **Effort**: small / medium / large, and which files it touches.
   - **Trade-off**: what it costs or could break (discoverability, consistency, other flows).
   Number them. Mark the one you recommend and say why in one sentence.
4. **Stop and wait.** Do not edit code until the user answers. They may pick one, combine two,
   reject all, or reshape one.
5. **Build the chosen resolution.** Keep the change limited to what was agreed. If a better idea
   appears while building, finish the agreed one and mention the idea in one line.
6. **Show it.** Run, screenshot or describe how to see the change on the target platform, using
   whatever run or preview route the repo already has. Say what you verified and what you didn't.
7. **Commit** with a message that names the friction and the fix.

## Levers to draw options from

| Lever | Examples |
| --- | --- |
| Remove a step | default value, remember last choice, skip a confirmation that is never declined |
| Move it closer | shortcut, pinned action, swipe or long-press, home-screen entry |
| Make it visible | larger target, contrast, label, empty-state hint, inline status |
| Make it forgiving | undo instead of confirm, autosave, tolerant input |
| Make it faster | optimistic update, cached result, background load, skeleton in place of a jump |
| Change the flow | merge two screens, split one overloaded screen, reorder steps |
| Automate it | do it on a trigger, batch it, settle it once in settings |
| Leave it, explain it | when the friction protects against a costly mistake, say so as an option |

## Notes

- The pause in step 4 is the point of the skill. A user who wanted a direct fix would have asked
  for one.
- Keep each option short. Five tight options beat two essays.
- If the friction is a bug (something broken, not awkward), say so and offer to fix it directly
  instead of running the options round.
