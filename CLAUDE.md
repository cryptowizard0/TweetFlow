```
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.
```

# TweetFlow Codebase Guide

## Overview
TweetFlow is a plugin repository that packages reusable Twitter/X workflow skills using `opencli` as its runtime. It supports both Codex and Claude Code runtimes, sharing skills and scripts across both platforms.

## Repository Structure
- **`.claude-plugin/`**: Claude Code plugin manifest (`plugin.json`)
- **`.codex-plugin/`**: Codex plugin manifest (`plugin.json`) 
- **`skills/`**: Shared skill implementations for both runtimes
- **`scripts/`**: Utility scripts for setup and execution

## Key Skills
Available TweetFlow skills (in `skills/` directory):
- `tweetflow-setup`: Prepares TweetFlow for first use by installing `opencli`, checking browser bridge connectivity, and verifying login
- `twitter-quote-commentary`: Generates quote commentaries for tweets
- `twitter-reply-top3`: Generates top 3 replies for tweets
- `twitter-reply-top3-english-only`: English-only version of the top 3 replies skill
- `cross-project-adapter-migration`: Handles adapter migration across projects

## Runtime Requirements
- TweetFlow auto-installs `@jackwener/opencli` if not available in PATH (default: `~/.tweetflow/vendor/opencli`)
- Requires Chrome browser with opencli Browser Bridge extension installed and connected
- Requires authenticated Twitter/X session in Chrome

## First Run Setup
```bash
./scripts/tweetflow-setup
```
The setup script:
1. Auto-installs `opencli` if needed
2. Runs `opencli doctor --live`
3. Reports status: `ready`, `needs browser bridge`, or `needs login`

## Using Claude Code Plugin
From repository root:
```bash
claude --plugin-dir .
```

From parent directory:
```bash
claude --plugin-dir ./TweetFlow
```

## Environment Variables
- `TWEETFLOW_OPENCLI_BIN`: Specify existing `opencli` binary instead of auto-installing
- `TWEETFLOW_OPENCLI_HOME`: Change private install directory for `@jackwener/opencli`
