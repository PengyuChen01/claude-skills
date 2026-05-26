---
name: tldr
version: 0.1.0
description: |
  Produce a TL;DR of a long input (file, URL, pasted text, conversation history,
  or PR/issue thread). Returns a three-tier summary: 1-line gist, 3-5 bullets,
  optional paragraph.
  Use when asked to "tldr", "summarize this", "what's the gist", "短一点",
  "一句话总结", or when the user pastes a wall of text and just wants the point.
---

# /tldr — Three-Tier Summary

## Output format (always)

```
**TL;DR**: <one sentence, ≤25 words>

**Key points**:
- <bullet 1 — fact, not category>
- <bullet 2>
- <bullet 3>
(3-5 bullets total, ordered by importance)

**Detail** (optional, only if user asks "more" or content >2000 words):
<one paragraph, ≤100 words>
```

## Rules

1. **One sentence TL;DR** — the user can stop reading after this line.
2. **Bullets are facts, not topics** — "API rate limit dropped from 1000 to 100/min" beats "discusses rate limits".
3. **Quote numbers, names, dates** verbatim from source — TL;DR's job is to compress, not paraphrase to fuzziness.
4. **No "the article discusses…"** — write as if you're the author summarizing your own work.
5. **Skip throat-clearing** — no "Here's a summary:" preamble. Output starts with `**TL;DR**:`.
6. **Cite source briefly** at the end if URL-based: `Source: <url or file path>`.

## Length guidance by input size

| Input size | Output |
|---|---|
| <500 words | TL;DR only (1 line) |
| 500-2000 words | TL;DR + 3 bullets |
| 2000-10000 words | TL;DR + 5 bullets + 1 paragraph |
| >10000 words / video / book | TL;DR + 5 bullets + 1 paragraph + offer to drill into sections |

## What inputs work

- File path: read the file, summarize.
- URL: fetch (use WebFetch / mcp__fetch__fetch), then summarize.
- YouTube URL: try to get transcript; if blocked, search for written coverage of the talk and summarize that.
- Pasted text: summarize directly.
- "tldr the conversation": summarize this Claude Code session so far.
- GitHub PR/issue: fetch via `gh pr view` / `gh issue view`, summarize.

## Failure modes to avoid

| ❌ | ✅ |
|---|---|
| "This article explores various aspects of…" | "Rust 1.80 ships async traits in stable." |
| 8 bullets that hedge | 3 bullets that commit |
| "It might be useful for some use cases" | "Useful when latency <10ms matters." |
| Vague paraphrases | Verbatim numbers/names/quotes |

## Example

**Input**: 3000-word blog post on MCP roadmap

**Output**:
```
**TL;DR**: MCP ships stateless transport, server discovery, and cross-app SSO in June 2026 — making it deployable on standard cloud platforms and SSO-compatible for enterprise.

**Key points**:
- Stateless transport (Google proposal) → deploy MCP on Cloud Run / K8s like normal REST
- well-known URL server discovery → agents auto-find MCP servers, no manual config
- Cross-App Access → one SSO login (Okta/Google) covers all MCP servers
- Skills over MCP → server ships skills with itself, bypassing plugin registry
- Python SDK v2 incoming (long overdue)

**Detail**: David Soria Parra's AIE Europe keynote frames 2026 as "the year agents go to production." Three pillars matter: Skills (knowledge), CLI (local tools), MCP (connectivity with auth/state/UI). Client-side wins come from Progressive Discovery (load tools on demand) and Programmatic Tool Calling (model writes code that orchestrates tools, not one-call-per-turn).

Source: https://tldrecap.tech/posts/2026/aie-europe/agent-ecosystem-production-ready/
```
