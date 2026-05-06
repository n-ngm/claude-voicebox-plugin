Set up or update the VOICEVOX plugin. All steps are idempotent — safe to re-run after a plugin update. Interact with the user in their language throughout the process.

## 1. Check Prerequisites

Verify the following commands are available. If any are missing, notify the user and stop.

- `node` (Node.js 18+)
- `npm`
- `docker`
- macOS: `terminal-notifier` (`brew install terminal-notifier`)
- Linux: `notify-send` (e.g. `sudo apt install libnotify-bin`) and one of `paplay`/`aplay`/`mpv` for audio playback

## 2. Install npm Dependencies

Install (or update) the plugin's dependencies.

```bash
cd <plugin_root> && npm install
```

Replace `<plugin_root>` with the actual path of `${CLAUDE_PLUGIN_ROOT}`.

## 3. Start VOICEVOX Engine

If VOICEVOX Engine is not already running on port 50021, start it using the bundled `docker-compose.yml`.

```bash
docker compose -f <plugin_root>/docker-compose.yml up -d
```

Verify the engine responds with `curl -s http://localhost:50021/version`.

## 4. Create Stable Symlink

Create (or update) a stable symlink so the CLAUDE.md import path is version-independent:

```bash
ln -sfn <plugin_root> ~/.claude/plugins/voicevox
```

This symlink is updated each time `/voicevox:setup` is run, so the CLAUDE.md import stays valid across plugin updates.

## 5. Select Default Character

Set `voicevox.current_speaker` in `~/.claude/settings.voicevox.json`.

Skip if already configured. Otherwise, read the `.json` files in the plugin's `speakers/` directory (excluding `call_names.json`). For each file, extract the `character.name` for display. Present 4 popular characters as choices via AskUserQuestion showing the display name, but record the filename stem (without `.json`) as the value. Include "Other" as an option with a note that all available characters can be typed by filename stem.

For example, `zundamon.json` → display "ずんだもん", value `zundamon`.

Available filename stems: `zundamon`, `shikoku_metan`, `kasukabe_tsumugi`, `tohoku_zunko`, `tohoku_kiritan`, `tohoku_itako`, `shirakami_kotarou`, `kurono_takehiro`, `chibishikijii`, `ankomon`.

Write the **filename stem** (not the display name) to `~/.claude/settings.voicevox.json`:

```json
{
  "voicevox": {
    "current_speaker": "<filename-stem>"
  }
}
```

## 6. Configure Permissions

If `~/.claude/settings.json` does not already allow reading `settings.voicevox.json`, ask the user for permission to add the following to `permissions.allow`:

```
Read(~/.claude/settings.voicevox.json)
Read(**/.claude/settings.voicevox.json)
```

If approved, append to the `permissions.allow` array. Skip if already present.

## 7. Add CLAUDE.md Import

Check `~/.claude/CLAUDE.md` for an existing VOICEVOX import in the `### Import` section.

- If `~/.claude/plugins/voicevox/CLAUDE.md` is already imported → skip.
- If an older versioned path (e.g. `.../voicevox/1.x.x/CLAUDE.md`) is present → replace it with the symlink path.
- If not imported at all → ask the user for permission to add it.

The import line to write:
```
VoiceVox: @~/.claude/plugins/voicevox/CLAUDE.md
```

## 8. Report Completion

Report the setup results to the user. Inform them that Claude Code must be restarted to apply the changes.

For updates: remind the user to run `/voicevox:setup` again after each `claude plugin update voicevox@claude-voicevox-plugin`.
