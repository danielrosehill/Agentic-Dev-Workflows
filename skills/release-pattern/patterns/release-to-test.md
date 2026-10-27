# Preset: release-to-test

The user's words, typically: "I'm using the release to test on my own device." The release *is* the
test build: build and publish first, then the user installs it and tests. Not the orthodox order,
but it is often the shortest route to a real device (CI does the signed build, the device updates
from the release channel).

| Field | Value |
| --- | --- |
| Trigger | Each change (or small batch) the user wants to try on their device. Ask "release this to test?" only if it's unclear whether a change is ready to try. |
| Between releases | Commit and push, then release. |
| Testing | After release, on the user's device. The agent's own checks (build, tests, lint) still run before the release, because a broken build wastes a cycle. |
| Versioning | Bump the smallest part (patch or build number) per test release. Releases are cheap and frequent; don't hold back to make them "worth it". |
| Hotfix | Not needed: every fix goes out the same way. |

## Agent behaviour

- After releasing, confirm the build actually published (CI run finished, artifact present on the
  release or channel) before telling the user it's ready. Give the version number they should see
  on the device.
- Then hand over: what to test, in one to three concrete steps, and what result means it worked.
- Wait for the user's report. Treat it as the test result: fix, release again, repeat.
- Keep release notes short and factual; they are test builds. If the repo has a stable channel,
  don't promote to it unless the user says so.
- If the user switches to "hold the next one, I'll batch it", switch to the sprint preset.
