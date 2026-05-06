# claude-voicebox-plugin

Claude Code plugin for VOICEVOX voice notifications with character personalities.

Enables desktop notifications and voice synthesis via a local [VOICEVOX](https://voicevox.hiroshiba.jp/) engine when Claude Code completes tasks or needs your attention.

## Prerequisites

- [Node.js](https://nodejs.org/) 18+
- [Docker](https://www.docker.com/)
- macOS: [terminal-notifier](https://github.com/julienXX/terminal-notifier) (`brew install terminal-notifier`)
- Linux: `notify-send` and one of `paplay` / `aplay` / `mpv`

## Installation

### 1. Add the plugin

```bash
claude plugin add /path/to/claude-voicebox-plugin
```

Or install directly from GitHub (once supported):

```bash
claude plugin add github:n-ngm/claude-voicebox-plugin
```

### 2. Run the install command

In a Claude Code session:

```
/voicebox:install
```

This will:

1. Check prerequisites
2. Run `npm install` in the plugin directory
3. Start VOICEVOX Engine via Docker
4. Let you pick a default character
5. Configure permissions and CLAUDE.md import

### 3. Restart Claude Code

Restart Claude Code to apply all changes.

## Character Configuration

Set `voicebox.current_speaker` in `settings.voicebox.json`:

```json
{
  "voicebox": {
    "current_speaker": "zundamon"
  }
}
```

**Resolution priority:**

1. `{project-directory}/.claude/settings.voicebox.json`
2. `~/.claude/settings.voicebox.json`
3. Default: `zundamon`

## Available Characters

| File | Character |
|------|-----------|
| `zundamon.json` | ずんだもん |
| `shikoku_metan.json` | 四国めたん |
| `kasukabe_tsumugi.json` | 春日部つむぎ |
| `tohoku_zunko.json` | 東北ずん子 |
| `tohoku_kiritan.json` | 東北きりたん |
| `tohoku_itako.json` | 東北イタコ |
| `shirakami_kotarou.json` | 白上虎太郎 |
| `kurono_takehiro.json` | 玄野武宏 |
| `chibishikijii.json` | ちび式じい |
| `ankomon.json` | あんこもん |

## MCP Tools

The plugin exposes the following MCP tools for direct use:

| Tool | Description |
|------|-------------|
| `get_voices` | List available voices from VOICEVOX |
| `text_to_speech` | Synthesize and play a single text |
| `text_to_speech_batch` | Synthesize and play multiple texts with pipeline playback |
| `sing` | Synthesize singing voice from a musical score |

## Troubleshooting

**Cannot connect to VOICEVOX Engine**

```bash
curl http://localhost:50021/version
docker compose -f /path/to/plugin/docker-compose.yml restart
```

**MCP server not responding / multiple processes**

```bash
pkill -f voicebox
```

Restart Claude Code to relaunch the MCP server.

## License

MIT — see [LICENSE](LICENSE)

This plugin connects to [VOICEVOX](https://voicevox.hiroshiba.jp/), a free voice synthesis engine. Each character voice has its own terms of use — please review them on the [VOICEVOX website](https://voicevox.hiroshiba.jp/) before use.
