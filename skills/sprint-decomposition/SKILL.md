---
name: sprint-decomposition
description: User narrates a fire hose of changes (bugs, features, docs, ideas) for a sprint; split it into separate items and record them in the repo's trackers, or offer to set trackers up or use a one-sprint scratchpad. Any platform.
---

# Sprint decomposition: fire hose in, tracked items out

The user narrates a batch of changes in one go, often dictated: "These are the bugs: … features:
… and for the docs …". The categories blur into each other and items arrive half-formed. This skill
is the **first stage** of the sprint: split the narration into separate items, sort them, and record
them somewhere durable. It does not start building.

Works in any repo: website, Android app, CLI, library.

## Inputs

- The narration, in the user's words. Typed, pasted or transcribed.
- Optionally screenshots or URLs attached to particular items.

## Steps

1. **Detect tracking infrastructure** in the repo, before parsing anything. Look for, in this order:
   - `planning/bugs/`, `planning/features/`, `planning/docs/`, `planning/ideas/` (the layout in
     [references/tracker-layout.md](references/tracker-layout.md), shared with `dev-debugger`'s
     `capture-bug`).
   - Other file trackers: `BACKLOG.md`, `TODO.md`, `ROADMAP.md`, `planning/*.md`, `docs/backlog/`,
     an issues folder.
   - GitHub Issues, if the repo's `CLAUDE.md` or `README` says that is where work is tracked.
     Don't assume Issues just because the repo is on GitHub.
   - An open sprint scratchpad (`planning/sprints/*.md` with `status: open`) from an earlier run.
2. **Ask only if nothing was found.** If trackers exist, skip to step 3 and use them as they are,
   including their naming and ID scheme. If none exist, ask one question, then stop:
   > There's no tracking set up in this repo. For this batch, should I:
   > 1. **Set up trackers** in the repo (`planning/bugs`, `features`, `docs`, `ideas`) and file
   >    each item there. They stay for future sprints.
   > 2. **Use a scratchpad** for this sprint only: one file with every item, closed when the sprint
   >    ships.

   If the repo has *some* trackers but not one for a category the narration needs (e.g. bugs are
   tracked, docs aren't), don't ask the whole question: file what fits, and offer in one line to add
   the missing tracker.
3. **Split the narration into items.** One item per distinct change. Rules:
   - The user's category label is a hint, not a verdict. "Feature: the contact form doesn't send"
     is a bug; file it as one and say so in the report.
   - One sentence can hold two items ("fix the header and make it sticky" is a bug and a feature).
   - Merge repeats: the user circling back to the same thing is one item with the extra detail.
   - Keep the user's wording in each item's notes. Don't invent steps, causes or acceptance criteria;
     write `TBD` where they didn't say.
   - If you can't tell whether something is one item or two, or which category, file your best
     guess and list it under **Check** in the report rather than stopping to ask.
4. **Categorise** each item: **bug** (broken: doesn't do what it should), **feature** (new or
   changed behaviour), **docs** (README, user docs, in-app help, comments), **idea** (not committed
   to: "maybe", "at some point", "what if"). Ideas are recorded but are not in the sprint unless the
   user says so. Use other categories only if the repo's trackers already have them (e.g. `chore`,
   `content`).
5. **Record.** Into existing trackers in their own format, into new trackers per
   [references/tracker-layout.md](references/tracker-layout.md), or into a scratchpad per
   [references/scratchpad.md](references/scratchpad.md). Assign IDs by scanning the existing
   highest number per category, open and closed together.
6. **Report** in one table: ID, category, title, where it was filed. Then a **Check** list (items
   whose split or category you guessed, and any re-categorised from the user's label), and a
   proposed order of work for the sprint, bugs first unless the user said otherwise.
7. **Stop.** Don't start on the items. The user may reorder, merge, drop or promote ideas first.
8. **Commit** the tracker files with a message like `Sprint intake: 3 bugs, 2 features, 1 docs`,
   and push, unless the repo's trackers are gitignored.

## Later in the sprint

- When an item is done, close it where it lives: move the file to the category's `resolved/` or
  `done/` folder, tick it in the scratchpad, or close the issue. Note the commit.
- If the session's release pattern is **sprint** (`release-pattern`), done items also go on the
  `Waiting to ship` list. In scratchpad mode that list is the scratchpad's own section.
- New items the user adds mid-sprint go through steps 3–5 again; no need to re-run detection.

## Notes

- The point is one place per item. Never leave items only in the chat.
- A short narration with one or two items doesn't need this skill; file them directly.
- If the narration mentions more than one repo, group items per repo and run detection in each.
