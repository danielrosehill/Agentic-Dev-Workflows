# Sprint scratchpad

Used when the user picks **scratchpad**: one file for this sprint, no standing trackers.

- Path: `planning/sprints/YYYY-MM-DD-<slug>.md` (date the sprint opened; slug from the main theme,
  e.g. `homepage-fixes`). If `planning/` would be the only thing in a repo that otherwise has no
  such folders, `SPRINT.md` at the root is fine; say which you used.
- Committed like any other file unless the user asks for it to be local, then gitignore it.
- One open scratchpad per repo. A later run finds it (`status: open`) and adds to it instead of
  starting another.
- When the sprint ships, set `status: closed` and the release version. Leave the file in place as
  the sprint's record. Unfinished items: carry them into the next sprint's scratchpad, listed under
  `Carried over`, or file them into trackers if the repo has gained some.

## Template

```markdown
---
sprint: <slug>
opened: YYYY-MM-DD
status: open
released_as: <version, when shipped>
---

# Sprint: <slug>

## Bugs

- [ ] **B1** <title>. <one or two lines; user's wording where it matters>

## Features

- [ ] **F1** <title>. <…>

## Docs

- [ ] **D1** <title>. <…>

## Ideas (not in this sprint)

- **I1** <title>. <…>

## Check

- <items whose split or category was guessed, and why>

## Waiting to ship

- <done items, with commit sha>
```

IDs (`B1`, `F1`, …) are local to the scratchpad. Tick an item and append ` (abc1234)` when done.
