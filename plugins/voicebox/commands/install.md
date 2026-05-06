Install the VoiceBox plugin. Follow these steps in order. Interact with the user in their language throughout the process.

## 1. Check Prerequisites

Verify the following commands are available. If any are missing, notify the user and stop.

- `node` (Node.js 18+)
- `npm`
- `docker`
- macOS: `terminal-notifier` (`brew install terminal-notifier`)
- Linux: `notify-send` (e.g. `sudo apt install libnotify-bin`) and one of `paplay`/`aplay`/`mpv` for audio playback

## 2. Install npm Dependencies

Install the plugin's dependencies.

```bash
cd <plugin_root> && npm install
```

Replace `<plugin_root>` with the actual path of `${CLAUDE_PLUGIN_ROOT}`.

## 3. Start VOICEVOX Engine

Start the VOICEVOX Engine using the bundled `docker-compose.yml`.

```bash
docker compose -f <plugin_root>/docker-compose.yml up -d
```

After starting, verify the engine responds with `curl -s http://localhost:50021/version`.

## 4. Select Default Character

Set `voicebox.current_speaker` in `~/.claude/settings.voicebox.json`.

Skip if already configured. Otherwise, read the `.json` files in the plugin's `speakers/` directory and extract each `character.name`. Present 4 popular characters as choices via AskUserQuestion. Include "Other" as an option so the user can type any character name.

Write the selection to `~/.claude/settings.voicebox.json`:

```json
{
  "voicebox": {
    "current_speaker": "<selected-character-name>"
  }
}
```

## 5. Configure Permissions

If `~/.claude/settings.json` does not already allow reading `settings.voicebox.json`, ask the user for permission to add the following to `permissions.allow`:

```
Read(~/.claude/settings.voicebox.json)
Read(**/.claude/settings.voicebox.json)
```

If approved, append to the `permissions.allow` array. Skip if already present.

## 6. Add CLAUDE.md Import

If `~/.claude/CLAUDE.md` does not already import the voicebox CLAUDE.md, ask the user for permission to add the following to the `### Import` section:

```
VoiceBox: @<plugin_root>/CLAUDE.md
```

Replace `<plugin_root>` with the actual absolute path of `${CLAUDE_PLUGIN_ROOT}`.

If approved, append to the `### Import` section. Skip if already present.

## 7. Report Completion

Report the setup results to the user. Inform them that Claude Code must be restarted to apply the changes.
