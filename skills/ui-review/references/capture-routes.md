# Capture routes by platform

`ui-review` only needs: start the app, navigate to a view, get an image of it, and ideally read its
structure. How depends on the platform. Prefer a dedicated plugin where one exists; it knows the
platform's traps.

| Platform | Route | Notes |
| --- | --- | --- |
| Linux, KDE Plasma (Wayland) | `kde-agent-workspace` plugin, skill `test-app-in-agent-workspace` | Runs the app on the agent's own virtual desktop. Navigate with AT-SPI (`a11y-tree`, `a11y-click`), capture with `capture --window <handle> --peek` (plain window capture of a hidden desktop returns a stale frame on Wayland). `use-sandbox` keeps everything off the user's screen. |
| Linux, other desktops | Window screenshot tool for the session (`spectacle`, `grim`, `gnome-screenshot`); AT-SPI for structure | Wayland compositors restrict capture of other windows; check the tool works before planning the tour. |
| Web | Playwright or a browser-automation tool | Set the viewport to the target size. Use the accessibility snapshot or DOM for structure, a screenshot for the image. Capture both light and dark if the site supports them and the focus is visual. |
| Android | `adb exec-out screencap -p > NN-view.png`; `adb shell uiautomator dump` for structure; Google's Android skills / `android` CLI | Emulator or a connected device. Navigate with `adb shell input tap` on coordinates from the dump. |
| Desktop Electron / Tauri | As the host OS, or the web route against its dev server | Electron needs `--force-renderer-accessibility` for an accessibility tree. |
| TUI / CLI | Run in a terminal and capture the text (for a TUI, the terminal's screen buffer) | Text beats an image here; review the text. |

If none of these works in the session, say which was tried and how it failed, then ask the user for
screenshots and continue with `screenshot-steering`.
