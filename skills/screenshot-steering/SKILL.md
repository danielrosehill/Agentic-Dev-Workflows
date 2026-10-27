---
name: screenshot-steering
description: User shares screenshots (pasted, paths, or a Google Photos album by name) to steer the work, "this is what's wrong", "this is fixed", or "this is how it looks on my device now". Gather them, say what each shows and where it lives in the code, then act or hand off. Any platform.
---

# Screenshot steering

Screenshots are how the user shows the agent the real app: on their phone, in their browser, on
their desktop. Not always a bug report. Often it's "here's the latest UI" so the agent's picture
of the app matches the user's. This skill gathers the images, reads them, ties each to the code,
and confirms the agent has understood before anything is built.

## Inputs

- Screenshots from any of:
  - pasted into the chat;
  - file paths, or a folder ("the ones in `~/Pictures/Screenshots` from today");
  - a **Google Photos album** by name ("I made an album called Hebrish App"), optionally with a
    filter ("only the ones I added today", "the last five");
  - files already in the repo.
- The user's description, overall or per image. May be absent ("just look").

## Steps

1. **Gather.** Pasted images are already in context. Paths: read them. Google Photos: follow
   [references/google-photos.md](references/google-photos.md). Apply the user's filter; if it
   can't be applied exactly (album shows taken date, not added date), say which rule you used.
2. **Read every image** before saying anything. Note device and form factor (phone, tablet,
   desktop, window width), light or dark theme, app version or build if visible, and any text on
   screen.
3. **Locate each in the code**: which screen, route, component or command renders it. Use visible
   strings, titles and layout to search the repo.
4. **Reply with one line per image**: what it shows, where it lives in the code, and the user's
   point about it in your words. Classify each:
   - **Wrong**: broken or not as intended (a bug, layout break, wrong text).
   - **Fixed**: confirms an earlier change works on the device. Close the matching tracked item if
     there is one.
   - **Current state**: reference only. Update your understanding; flag anything that differs from
     what the code suggests (stale build, platform-specific rendering, a change not yet deployed).
   - **Friction**: works, but awkward.
   - **Unclear**: say what you see and ask what they meant. Don't guess a fix.
5. **Act or hand off**, by the mix:
   - One clear wrong item: fix it, then verify on the same platform if the repo has a run or
     preview route.
   - Several items, or bugs mixed with features: `sprint-decomposition`, with each item linking its
     screenshot.
   - Friction: `ui-friction`.
   - Only current state or fixed: no code change. Confirm in one line and carry on with the task.
6. **Keep screenshots that back an item.** If an item is filed or a fix is committed, copy its
   images into the repo next to the item (tracker `screenshots/` folder, or
   `planning/screenshots/YYYY-MM-DD-<slug>/`), named `<NN>-<what-it-shows>.png`. Reference-only
   screenshots aren't committed unless the user asks.

## Notes

- Compare like with like: a phone screenshot against the desktop CSS breakpoint is a common
  false alarm. Check which layout the device would get.
- If the build on the device is older than `HEAD`, say so before treating anything as a bug.
- Many images (more than ~10): read them in batches and keep only per-image notes in context.
