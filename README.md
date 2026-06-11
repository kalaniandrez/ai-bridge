# ai-bridge

Three tiny shell scripts that let Claude, OpenAI Codex, and Google Gemini consult each other headlessly from any terminal, using the CLI subscriptions you already pay for. No API keys, no copy-paste between chat windows.

```bash
ask-claude "is this schema design sane?" < schema.sql
ask-codex  "review this diff for bugs"   < changes.diff
ask-gemini "summarize this doc in 5 bullets" < notes.md
```

## Why

If you run more than one AI subscription, the second opinion is one pipe away. The killer use case: an agent session in one model calling another model for a cross-check. Claude Code can run `ask-codex` mid-task, Codex can run `ask-claude`, and every exchange is logged.

## What each script does

- `ask-claude`: calls `claude -p` (Claude Code CLI, headless print mode)
- `ask-codex`: calls `codex exec` in a read-only sandbox (override with `ASK_CODEX_SANDBOX=workspace-write`)
- `ask-gemini`: calls `gemini -p` (Gemini CLI)

Every call writes a timestamped markdown log (prompt + reply) to `~/ai-bridge/log/` (override with `AI_BRIDGE_LOG=/path`), so cross-model consultations leave a paper trail you can grep later.

## Install

You need the underlying CLIs installed and authenticated first ([Claude Code](https://claude.com/claude-code), [Codex CLI](https://github.com/openai/codex), [Gemini CLI](https://github.com/google-gemini/gemini-cli)). Then:

```bash
git clone https://github.com/kalaniandrez/ai-bridge && cd ai-bridge
chmod +x bin/*
ln -s "$PWD"/bin/* /usr/local/bin/
```

## Notes

- Read-only by default everywhere it matters: `ask-codex` runs sandboxed, and nothing here grants write access to your repo.
- These are deliberately tiny. The value is the convention (one verb per model + a shared log), not the code.

MIT · Built by [Kalani André](https://kalani.place)
