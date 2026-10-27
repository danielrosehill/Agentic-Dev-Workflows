# Agentic Dev Workflows

Cross-cutting workflows for developing software with AI agents. Not tied to a platform: the same
skills work in an Android app, a Linux tool, a web app or a CLI. Scope and the test for new skills:
[SCOPE.md](SCOPE.md).

## Skills

| Skill | What it does |
| --- | --- |
| `ui-friction` | You describe a UI friction point; the agent proposes 3–5 distinct resolutions with effort and trade-offs, waits for your pick, then builds it. |
| `release-pattern` | Sets how releases happen this session. Presets: **sprint** (commit and push freely, release only when you say the sprint is done) and **release-to-test** (release each change and test the released build on your own device). Or define a custom one. |
| `pin-release-pattern` | Writes the release pattern into the repo's `CLAUDE.md` so every future session follows it. |
| `voice-intake` | First step for a development voice note: fetches it from your saved location, a path or a Drive link, transcribes it, files the raw and lightly cleaned transcript in the repo, then hands off (usually to `sprint-decomposition`). Remembers where you keep notes and how to transcribe, in the plugin's user-data folder (`config.json`, key `voice_intake`). |
| `screenshot-steering` | You share screenshots (pasted, paths, or a Google Photos album by name, fetched in the browser) to show what's wrong, what's fixed, or how the app looks on your device now. The agent says what each shows and where it lives in the code, then fixes or hands off. |
| `video-steering` | A screen recording becomes scene-change key frames plus a transcript of your narration, then goes through `screenshot-steering`. Claude has no native video input; an external video model is the fallback for motion-only problems. |
| `sprint-decomposition` | You narrate a batch of bugs, features, docs and ideas; the agent splits them into separate items and files them in the repo's trackers. With no trackers, it asks: set them up, or use a scratchpad for this sprint only. |
| `complementary-plugins` | Suggests plugins that fit the current repo: platform tooling (Android, Linux packaging, KDE), QA, specs, planning, agent habits. |

## Installation

```bash
claude plugins marketplace add danielrosehill/Claude-Code-Plugins
claude plugins install agentic-dev-workflows@danielrosehill
```

## License

MIT
