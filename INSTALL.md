# Install i-am-intj

One skill, one harness: Claude Code. Adapters for other agents were removed deliberately; if you need one, the upstream project ships several.

## Plugin (recommended)

```bash
claude plugin marketplace add chamberlainjd/i-am-intj
claude plugin install i-am-intj@i-am-intj
```

Type `/i-am-intj`.

### Verify

```bash
claude plugin list
```

### Update

```bash
claude plugin marketplace update i-am-intj
```

### Uninstall

```bash
claude plugin uninstall i-am-intj
claude plugin marketplace remove i-am-intj
```

Or keep it installed and turn it off: `claude plugin disable i-am-intj`.

### Always-on (optional)

A `SessionStart` hook loads the full ruleset at the start of every session, no `/i-am-intj` needed:

```bash
touch ~/.claude/.i-am-intj-always
```

Back to on-demand:

```bash
rm ~/.claude/.i-am-intj-always
```

The hook only fires when the flag file exists, so installing the plugin changes nothing by itself. Honors `$CLAUDE_CONFIG_DIR` if you've moved your config dir. "stop intj mode" still turns it off for the current session.

## Skill folder (no `claude` CLI)

Claude Code also reads personal skills straight from `~/.claude/skills/`:

```bash
git clone https://github.com/chamberlainjd/i-am-intj
cp -R i-am-intj/skills/i-am-intj ~/.claude/skills/
```

New session, type `/i-am-intj`. Update by re-copying after `git pull`.

This route skips the plugin's hooks, so the always-on flag does nothing by itself. To get always-on without the plugin, keep the full clone somewhere permanent and register the hook manually in `~/.claude/settings.json` (the script resolves `SKILL.md` relative to its own location, so it must run from a full clone, not from the copied skill folder):

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume|clear|compact",
        "hooks": [
          {
            "type": "command",
            "command": "node /path/to/i-am-intj/hooks/always-on.js",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

Then `touch ~/.claude/.i-am-intj-always` as above.

## How activation works

1. **Installed, not invoked.** Nothing happens: `SKILL.md` sets `disable-model-invocation: true`, so the model never sees the skill and never applies the rules on its own.
2. **You type `/i-am-intj`.** Rules on for that session. "stop intj mode" or "normal mode" turns them off.
3. **You touch `~/.claude/.i-am-intj-always`.** The `SessionStart` hook loads the full ruleset from message one, every session.

No middle ground: if you did not turn it on, it is off.

## Troubleshooting

**`/i-am-intj` not in autocomplete.** Restart Claude Code. The plugin index is read at startup.

**Always-on flag has no effect.** Update the plugin (`claude plugin marketplace update i-am-intj`) and restart. Hooks are read at startup. On the skill-folder route, the flag needs the manually registered hook above.

**`claude plugin marketplace add` fails.** Use the `owner/repo` form. A local path must point at the repo root, not `.claude-plugin/`.

**Installed but replies still open with a compliment.** Open a new session. If it still drifts, tighten the wording in `skills/i-am-intj/SKILL.md`.

**Want different rules.** Fork, edit `skills/i-am-intj/SKILL.md`, then swap your copy in:

```bash
claude plugin uninstall i-am-intj
claude plugin marketplace remove i-am-intj
claude plugin marketplace add <your-username>/i-am-intj
claude plugin install i-am-intj@i-am-intj
```

Restart, then re-invoke `/i-am-intj`.
