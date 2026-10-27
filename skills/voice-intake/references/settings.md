# voice-intake settings

Path: `~/.config/agentic-dev-workflows/voice-intake.json`. Per user, per machine, never committed.
The plugin ships no defaults; everything here comes from the user.

```json
{
  "locations": [
    {"name": "dev notes", "type": "local", "path": "~/Recordings/dev-notes"},
    {"name": "phone uploads", "type": "gdrive", "folder_url": "https://drive.google.com/drive/folders/<id>", "access": "rclone remote 'gdrive:' | MCP server name | browser"}
  ],
  "transcription": {
    "route": "mcp | cli | api",
    "detail": "<MCP tool name, CLI command template with {input} and {output}, or service plus where its key comes from>",
    "language": "en"
  },
  "commit_audio": "never | private-repos | always",
  "updated": "YYYY-MM-DD"
}
```

## Filling it on first run

Ask only for what is missing, in one message:

1. **Where do you usually save development voice notes?** One or more: a local folder, a Drive
   folder, another cloud folder. For a cloud folder, check the session can list it before saving
   (MCP server, `rclone lsf`, browser); if it can't, say what's missing.
2. **Transcription.** Before asking, look for what's already there and offer it:
   - an MCP tool whose name contains `transcribe` or `transcription`;
   - a local CLI: `whisper`, `whisper-ctranslate2`, `faster-whisper`, `whisper.cpp` (`whisper-cli`);
   - an API key in the environment for a speech-to-text service.
   Propose the one found ("Found `whisper-cli` locally, use it?"). If none, ask which to use.
   Note whether the route keeps audio on the machine or uploads it.
3. **Audio in repos?** `never` (transcripts only; `source.json` points to the original),
   `private-repos`, or `always`.

Write the file, set `updated`, and confirm in one line what was saved.

## Changing it

The user can say at any time where notes are now, or which route to use. Edit the matching field,
keep the rest, update `updated`.
