# TweetFlow Agent Guide

## Overview

TweetFlow is a dual-runtime plugin repository.

- Codex uses `.agents/plugins/marketplace.json` plus `plugins/tweetflow/.codex-plugin/plugin.json`
- Claude uses `plugins/tweetflow/.claude-plugin/plugin.json`
- The implementation under `plugins/tweetflow/` is shared across runtimes

Default rule: preserve the current structure unless the user explicitly asks for a migration.

## Repository layout

- `README.md`
  User-facing usage and setup documentation
- `CLAUDE.md`
  Claude-specific operating rules for this repo
- `.agents/plugins/marketplace.json`
  Codex-only marketplace entry for the local plugin
- `plugins/tweetflow/.claude-plugin/plugin.json`
  Claude plugin manifest
- `plugins/tweetflow/.codex-plugin/plugin.json`
  Codex plugin manifest
- `plugins/tweetflow/skills/`
  Shared skills
- `plugins/tweetflow/scripts/`
  Shared runtime scripts

## Rules for all agents

1. Do not move the Claude plugin root away from `plugins/tweetflow`.
2. Do not treat `.agents/plugins/marketplace.json` as a Claude marketplace file.
3. Do not delete or collapse `.claude-plugin` and `.codex-plugin` into a single manifest location.
4. Keep shared skills and scripts reusable by both runtimes unless the user explicitly asks for divergence.
5. Prefer documentation clarification over structural changes when the issue is runtime-specific guidance.
6. Before changing shared plugin files, consider both Claude and Codex behavior.

## Editing priorities

- Safe by default:
  - updating docs
  - refining manifests without changing layout semantics
  - adding compatible skills or scripts under `plugins/tweetflow/`
- Requires extra care:
  - changing skill paths
  - changing script locations
  - renaming plugin folders
  - changing marketplace assumptions
  - introducing runtime-specific path conventions into shared skills

## Runtime notes

- Claude should be run with `claude --plugin-dir ./plugins/tweetflow`
- Codex should use `.agents/plugins/marketplace.json` to discover the plugin
- `tweetflow-setup` remains the first-run entrypoint for browser-backed workflows

## Documentation split

- Put end-user usage, setup, and runtime examples in `README.md`
- Put Claude-specific behavior and guardrails in `CLAUDE.md`
- Keep this file short, repo-wide, and agent-oriented
