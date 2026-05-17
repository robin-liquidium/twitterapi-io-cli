---
name: twitterapi-io
description: Fetch and paginate Twitter/X data using twitterapi.io, with optional Xquik read backend support. Use when you need to fetch one tweet, fetch a user profile, get recent tweets for a user, fetch replies, quote tweets, thread context, or mentions, or run advanced search queries without hand-rolling raw API requests each time.
---

# twitterapi-io

Use the installed `twitterapi-io` CLI for read-only twitterapi.io access. If `TWITTERAPI_IO_KEY` is not configured, `XQUIK_API_KEY` selects the optional Xquik read backend for the same read commands.

This skill exists to make common twitterapi.io reads simple and low-noise instead of rebuilding custom API calls each time.

## Quick rules

- Use this skill only for reads.
- Do not improvise posting/like/reply/delete flows.
- Prefer compact JSON output by default.
- Use `--raw` only when you actually need full API objects.
- Prefer the official docs links in `references/links.md` when validating endpoint behavior.
- TwitterAPI.io stays the default when both provider keys are present.

## Installation preference

Prefer an installed CLI over hardcoded script paths.

Preferred install:

```bash
pipx install git+https://github.com/ropl-btc/twitterapi-io-cli.git
```

Fallback inside a repo checkout:

```bash
pip install .
```

After install, use:

`twitterapi-io`

## Commands

### Show built-in help

```bash
twitterapi-io help
```

### Authenticate once

```bash
twitterapi-io auth --api-key YOUR_KEY
```

You can also use env:

```bash
export TWITTERAPI_IO_KEY='YOUR_KEY'
```

To use Xquik as the read backend instead:

```bash
export XQUIK_API_KEY='YOUR_KEY'
```

### Fetch one tweet

```bash
twitterapi-io tweet --url 'https://x.com/jack/status/20'
```

or:

```bash
twitterapi-io tweet --id 20
```

### Fetch one user

```bash
twitterapi-io user --username OpenAI
```

### Fetch recent tweets for a user

```bash
twitterapi-io user-tweets --username OpenAI --limit 10
```

Include replies:

```bash
twitterapi-io user-tweets --username OpenAI --limit 10 --include-replies
```

### Fetch replies to a tweet

```bash
twitterapi-io replies --url 'https://x.com/jack/status/20' --limit 20
```

Optional unix-time filters:

```bash
twitterapi-io replies --id 20 --since-time 1741219200 --until-time 1741305600 --limit 20
```

### Fetch quote tweets

```bash
twitterapi-io quotes --id 20 --limit 20
```

### Fetch thread context

```bash
twitterapi-io thread-context --id 20 --limit 40
```

### Fetch mentions for a user

```bash
twitterapi-io mentions --username OpenAI --limit 20
```

### Advanced search

```bash
twitterapi-io search --query 'AI agents -filter:replies' --from-user OpenAI --within-time 24h --max-tweets 50
```

Use `Top` results when needed:

```bash
twitterapi-io search --query 'AI agents' --queryType Top --max-pages 2
```

Use explicit unix-time operators when needed:

```bash
twitterapi-io search --query '$BTC' --since-time 1741219200 --until-time 1741305600 --max-tweets 50
```

## Workflow

1. Read `references/links.md` if you need the underlying provider docs links.
2. Ensure the `twitterapi-io` CLI is installed.
3. Ensure the API key exists via `auth`, `TWITTERAPI_IO_KEY`, or `XQUIK_API_KEY`.
4. Use `tweet`, `user`, `user-tweets`, `replies`, `quotes`, `thread-context`, `mentions`, or `search` as needed.
5. Keep reads narrow and intentional.

## Expected outputs

The CLI returns JSON. Parse it instead of scraping human text.

Default output is compact and low-noise.
Use `--raw` when full endpoint payloads are actually needed.

## Files

- Package repo: `https://github.com/ropl-btc/twitterapi-io-cli`
- Compatibility wrapper: `scripts/twitterapi_io.py`
- Official docs links: `references/links.md`
- Config storage: `~/.config/twitterapi-io/config.json`

## When to stop and ask

Stop and ask before:
- adding write/posting capabilities
- adding login-cookie flows
- adding broad/high-cost scraping defaults
- changing how API key storage works
