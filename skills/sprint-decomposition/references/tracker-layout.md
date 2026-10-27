# Tracker layout

Used when the user picks **set up trackers**. The bugs folder matches `dev-debugger`'s
`capture-bug` (`planning/bugs/to-resolve/BUG-0001.md`), so both tools read and write the same files.
The other categories follow the same pattern.

```
<repo-root>/planning/
├── bugs/
│   ├── to-resolve/BUG-0001.md
│   └── resolved/
├── features/
│   ├── to-do/FEAT-0001.md
│   └── done/
├── docs/
│   ├── to-do/DOCS-0001.md
│   └── done/
└── ideas/
    └── IDEA-0001.md
```

- IDs: per category, zero-padded to 4 digits. Next ID = highest number across the open and closed
  folders, plus one.
- Screenshots: `<category>/<open-folder>/screenshots/<ID>-<slug>.<ext>`, referenced from the item.
- Create only the categories this batch needs. Others are created when first used.
- Ideas have no done folder: an idea is promoted (moved to `features/to-do/` with a new `FEAT-` ID
  and a `promoted_from:` line) or deleted.

## First run: committed or gitignored

When creating `planning/` for the first time, ask in the same message as the setup question whether
the trackers should be committed (visible in the repo, history kept) or gitignored (local only, e.g.
a public repo). If gitignored, add `planning/` to `.gitignore`.

## Item template

Same frontmatter for every category; drop body sections that don't apply.

```markdown
---
id: FEAT-0001
title: <short one-line title>
category: bug | feature | docs | idea
date_captured: YYYY-MM-DD
status: open
sprint: <sprint name or date, if any>
user_label: <the category the user gave, if different>
screenshots: []
---

## Summary

<1–2 sentences in clean prose>

## What the user said

> <their words, lightly cleaned of filler>

## Details

<bug: steps, expected, actual. feature: behaviour wanted. docs: what to write and where.
Only what the user said; TBD for the rest.>

## Done

- [ ] Fixed / built in commit: <sha>
- [ ] Verified
```
