# Fetching screenshots from a Google Photos album

There is no usable API: since 31 March 2025 the Google Photos Library API only returns media the
calling app uploaded, so it can't read the user's own albums. Use a browser that is signed in to the
user's Google account: a browser-automation tool (e.g. Claude in Chrome, a persistent Playwright
profile), whichever the session has.

## Album lookup

Remember album names per repo in `~/.config/agentic-dev-workflows/screenshot-steering.json`:

```json
{"albums": {"<repo folder name>": "Hebrish App"}, "updated": "YYYY-MM-DD"}
```

When the user names an album, save or update the entry. When they say "the album" without a name,
use the saved one for this repo; if none, ask.

## Steps

1. Open `https://photos.google.com/albums` and open the album whose title matches (case-insensitive;
   if several match, ask). Note the album URL.
2. **Pick the images.** The album grid is grouped by date taken, newest first by default. For
   "today" or "the last N", read the date headers and take those. A screenshot's taken date is
   when it was captured, which is usually what the user means by "added today". If they truly mean
   added date and it differs, say so.
3. **Download.**
   - Whole album: the album's ⋮ menu → **Download all** gives a zip.
   - Some images: hover each, tick its checkbox, then **Shift+D** (or ⋮ → Download). Several
     selected images also arrive as a zip.
   - One image: open it, **Shift+D**.
   Downloads land in the browser's download folder (usually `~/Downloads`). Find them by name and
   modified time; don't take older files.
4. Unzip into a fresh temporary folder and read the images from there. Leave the album and the
   download untouched.

## Fallback

If downloading fails, open each image in the viewer and take a browser screenshot of it. Quality is
lower, so say that the images were captured from the viewer.
