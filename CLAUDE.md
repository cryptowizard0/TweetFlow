# TweetFlow Claude Guide

## Purpose

This file is for Claude Code working inside the `TweetFlow` repository.

TweetFlow is a dual-runtime plugin repo:

- Codex support is provided through `.codex-plugin` plus `.agents/plugins/marketplace.json`
- Claude support is provided through `.claude-plugin`
- The implementation under `plugins/tweetflow/` is shared across both runtimes

Default rule: preserve the shared layout unless the user explicitly asks for a migration.

## Source of truth

Treat these paths as authoritative:

- `plugins/tweetflow/.claude-plugin/plugin.json`
  Claude plugin manifest
- `plugins/tweetflow/.codex-plugin/plugin.json`
  Codex plugin manifest
- `plugins/tweetflow/skills/`
  Shared skills
- `plugins/tweetflow/scripts/`
  Shared runtime scripts
- `.agents/plugins/marketplace.json`
  Codex-only marketplace entry

## Claude-specific rules

1. Claude should load the plugin from `plugins/tweetflow`, not from the repository root.
2. Do not treat `.agents/plugins/marketplace.json` as a Claude marketplace manifest.
3. Keep Claude plugin components at the plugin root. Only `plugin.json` belongs inside `plugins/tweetflow/.claude-plugin/`.
4. Do not move shared `skills/`, `scripts/`, or other plugin assets into `.claude-plugin/`.
5. Do not remove or rewrite `.codex-plugin` or the Codex marketplace just to simplify Claude usage.

## Cross-runtime compatibility rules

1. Assume every change under `plugins/tweetflow/` may affect both Claude and Codex.
2. Preserve the current shared layout unless the user explicitly asks for runtime-specific divergence.
3. Do not convert shared skill script paths to Claude-only conventions unless the user explicitly asks and Codex compatibility has been verified.
4. Do not rename the plugin root, skill folders, or script locations without updating both runtime entrypoints.
5. If a Claude recommendation conflicts with the current dual-runtime structure, prefer documenting the distinction instead of forcing a structural rewrite.

## Claude usage

From the repo root:

```bash
claude --plugin-dir ./plugins/tweetflow
```

From the parent directory:

```bash
claude --plugin-dir ./TweetFlow/plugins/tweetflow
```

Absolute path example:

```bash
claude --plugin-dir /Users/webbergao/work/src/TweetFlow/plugins/tweetflow
```

Claude should expose these namespaced skills:

- `/tweetflow:tweetflow-setup`
- `/tweetflow:tweet-quote-commentary`
- `/tweetflow:tweet-reply-top3`
- `/tweetflow:tweet-reply-top3-english-only`
- `/tweetflow:cross-project-adapter-migration`

## Runtime requirements

- `opencli` is auto-installed on first use if missing
- default private install path is `~/.tweetflow/vendor/opencli`
- Chrome must be running locally
- the opencli Browser Bridge extension must be installed and connected
- an authenticated `x.com` session must already exist in Chrome

## First-run setup

Run:

```bash
./plugins/tweetflow/scripts/tweetflow-setup
```

Expected setup outcomes:

- `STATUS: ready`
- `STATUS: needs browser bridge`
- `STATUS: needs login`

## Environment variables

- `TWEETFLOW_OPENCLI_BIN`
  Use an existing `opencli` binary instead of auto-installing one
- `TWEETFLOW_OPENCLI_HOME`
  Change the private install directory used for `@jackwener/opencli`

## Editing guidance

- Prefer documentation updates over structure changes unless the user explicitly requests a migration.
- Keep README user-focused.
- Keep this file focused on Claude usage and guardrails.
