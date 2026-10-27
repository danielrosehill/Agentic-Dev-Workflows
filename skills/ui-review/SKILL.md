---
name: ui-review
description: Capture the running UI screen by screen, review it for a stated focus (e.g. simplify the menu), propose changes, then build.
---

# UI review: capture, look, review, then build

The user says "look at what we have and review it", "review the settings screen", or "streamline
the menu". The agent captures the real, running UI itself, screen by screen, reads the captures,
and reviews them against the focus the user gave. Like `ui-friction`, nothing is built until the
user picks.

Differs from its neighbours: `screenshot-steering` is the user showing screenshots;
`ui-friction` starts from one problem the user named. This starts from a whole app or surface,
and the agent finds the problems.

## Inputs

- **Scope**: the whole app, or a surface (a menu, settings screen, onboarding flow, a dialog).
- **Focus**, if given. See the table below. If none, use **general**.
- Optionally a target device or form factor ("on my phone", "at 1366 wide").

## Focuses

| Focus | Looks for |
| --- | --- |
| **general** | The most important few problems across the others |
| **simplify** | Items to cut, merge, regroup, rename or push into overflow; nesting deeper than needed; duplicate routes to one action; options almost nobody changes |
| **consistency** | Same thing named, placed or styled differently across screens; mixed icon sets, spacing, capitalisation |
| **layout** | Alignment, spacing, hierarchy, overflow and clipping, behaviour at the target size |
| **copy** | Labels, empty states, error text, jargon, tone |
| **accessibility** | Contrast, target size, focus order, labels a screen reader would announce |
| **first run** | What a new user sees and can understand without help |

## Steps

1. **Build or start the current version.** Use the repo's normal run route. Say which commit or
   build is being captured.
2. **Plan the tour.** From the code (routes, screens, menus, dialogs), list the views in scope.
   For a menu or settings review, include every level of it. Keep it to what the scope needs.
3. **Capture** each view into a fresh temporary folder (not the repo), named
   `NN-<view>.png`, using the platform route in
   [references/capture-routes.md](references/capture-routes.md). Prefer reading structure (the
   accessibility tree, the DOM) to navigate, and screenshots to see. Record every view you could
   not reach and why.
4. **Read every capture** before reviewing. For a menu or settings focus, also write out the full
   item tree as text (from the accessibility tree or the code), since screenshots show one level
   at a time.
5. **Review against the focus.** Findings, most important first, each with: the capture it is
   seen in, what is wrong for the user, and where it lives in the code. Cap at about 10; say if you
   stopped there.
6. **Propose.** For **simplify**, give 2–3 whole plans that differ in how far they go (light
   trim / regroup / restructure), each with the before and after item tree, what moves where, and
   what a user loses. For other focuses, give fixes per finding with effort (small / medium /
   large). Mark the one you recommend.
7. **Stop and wait.** The user picks plans or findings, or reshapes them.
8. **Build** the picked changes only. Re-capture the same views and show before and after.
9. **Commit** naming the review and the changes. Captures stay in the temp folder unless the user
   asks to keep them; then `planning/ui-review/YYYY-MM-DD-<scope>/`.

## Notes

- Review what is on screen, not what the code suggests should be there. If they differ (stale
  build, feature flag, platform-specific rendering), that is a finding.
- Several findings worth tracking but not building now: offer `sprint-decomposition` in one line.
- If the user's device would show a different layout than the capture (phone vs desktop), capture
  at that size or say the review doesn't cover it.
