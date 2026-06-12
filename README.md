# Ablo Decks Plugin

The official [Ablo](https://www.tryablo.com) Decks plugin for AI agents —
make and edit slide decks directly from Claude Code, Claude Cowork, or
Grok Build.

Ablo is an AI-powered presentation editor. This plugin connects Ablo's deck
MCP server, so your agent can build presentations by executing code against a
deck: slides, layers, themes, layouts, and AI image generation, with a
screenshot review loop for design quality. Every change commits through Ablo's
sync engine and appears live in the open editor — no export or publish step.

| Component | What it covers |
| --- | --- |
| `skills/ablo-decks` | The full workflow: get the deck from its link, the `execute_code` slide sandbox and `deck.*` API, the mandatory screenshot review checkpoints, themes, layouts, and image generation |
| `.mcp.json` | Ablo's hosted deck MCP server (`execute_code`, `get_slide_screenshot`, `get_guide`, `memory`) — OAuth browser approval on first use, no API keys |

## Install

**Claude Code**

```
/plugin marketplace add abloatai/decks-plugin
/plugin install ablo-decks
```

**Grok Build** — Grok Build reads Claude Code plugins natively; add this repo
as a marketplace source, or install from the marketplace tab once listed.

## How it works

1. Open (or create) a deck in [Ablo](https://www.tryablo.com) and share its
   link with your agent: `https://www.tryablo.com/<org>/deck/<deckId>`.
2. The first tool call opens a browser approval tying the session to your
   Ablo account.
3. Ask for what you want — "restyle this deck in our brand colors", "add a
   competitive landscape slide", "tighten the typography" — and watch the
   changes land live in the editor.

Looking for the developer SDK instead — shared state, schemas, and
multi-agent coordination on your own Postgres? That's the
[ablo plugin](https://github.com/abloatai/plugin).

## License

Apache-2.0
