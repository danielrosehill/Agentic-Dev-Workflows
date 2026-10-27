---
name: video-steering
description: User shares a screen recording; turn it into key frames plus a narration transcript, then handle it as screenshot steering.
---

# Video steering

Claude reads images, PDFs and text, not video. So a video becomes **key frames** plus a
**transcript** of what the user says over it, and then goes through `screenshot-steering`. That
covers most recordings: a flow, a wrong screen, a crash, a narrated tour. What frames lose is
motion itself (animation smoothness, jank, timing). For that, see "Motion" below.

## Inputs

- A video file: path, in the repo, a Drive link, or a Google Photos album
  (fetch it as in `screenshot-steering`'s `references/google-photos.md`).
- The user's description, if any.

## Steps

1. **Fetch** into a fresh temporary folder. Get duration and resolution with `ffprobe`.
2. **Extract frames.** Scene changes first, so each distinct screen appears once:

   ```bash
   ffmpeg -i in.mp4 -vf "select='gt(scene,0.25)',scale='min(1280,iw)':-2,showinfo" -vsync vfr frames/%03d.png 2> frames.log
   ```

   Take timestamps from `pts_time` in `frames.log` and rename to `NNN-<mm>m<ss>s.png`. If that
   gives fewer than 3 frames or more than ~30, use a fixed rate instead
   (`-vf fps=1/2` for every 2 s, tuned so the total stays near 20). Always keep the first and
   last frame.
3. **Transcribe narration** if the video has speech: extract the audio
   (`ffmpeg -i in.mp4 -vn -ac 1 -ar 16000 audio.wav`) and transcribe it with the route saved for
   `voice-intake`. Keep segment timestamps where the route gives them, to pair speech with frames.
4. **Build the timeline**: one line per frame with its timestamp and what was said around it.
   This is what the user's "at this point it breaks" refers to.
5. **Hand to `screenshot-steering`** from its step 2, with the frames as the screenshots and the
   timeline as the description. If the user points at a moment ("around 0:40"), extract 1 frame per
   0.25 s for a few seconds around it and look again.
6. **Keep what backs an item**: frames and timeline go into the repo next to the item, as in
   `screenshot-steering`. The video itself goes in only if the user asks.

## Motion

When the problem is only visible in motion (stutter, animation curve, flicker between two frames),
frames are weak evidence. Options, in order:

1. Dense frames over the short window (step 5) and compare them side by side.
2. An external model with native video understanding (e.g. Gemini) via whatever API or MCP the
   session has. It uploads the video, so say so first, and treat its answer as a lead to confirm in
   code, not as a finding.
3. Ask the user to describe what they see, and reproduce it on the agent's side if the platform
   allows (emulator, browser).
