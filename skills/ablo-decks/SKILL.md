---
name: ablo-decks
description: Create and edit slide decks in Ablo through the deck MCP server — run code in the slide sandbox, generate images, apply themes, and review your work with screenshots. Use when the user wants to make, edit, restyle, or review presentation slides, or shares an Ablo deck link (tryablo.com/.../deck/...).
---

# Ablo decks — make slides as an agent

Ablo is an AI-powered presentation editor. The `ablo-decks` MCP server lets you
create, edit, and transform slides by executing code against a deck; every
mutation commits through the sync engine, so the user watches your changes
appear live in their open deck.

## Connect and authenticate

The plugin configures the server (`https://www.tryablo.com/api/mcp-apps`). It
uses OAuth: on first use your MCP client opens a browser approval, and the
session is tied to the user's Ablo account and organization. There are no API
keys to manage.

## Getting the deck

All tools operate on an existing deck via `deckId`. The server cannot create
or list decks — ask the user to open (or create) the deck in Ablo and share
its link, then extract the id:

```
https://www.tryablo.com/<orgSlug>/deck/<deckId>
```

## The tools

| Tool | What it does |
| --- | --- |
| `execute_code` | **The primary tool.** Runs JavaScript/TypeScript in an isolated VM against the `deck.*` API; recorded mutations are committed atomically through the sync engine with real-time fan-out. Takes `code` and `deckId`. |
| `get_slide_screenshot` | Renders a slide to an image so you can review your work visually. |
| `get_guide` | Loads detailed API reference and workflow guides on demand — read the relevant guide before unfamiliar operations instead of guessing the API. |
| `memory` | Persistent notes across sessions (user preferences, deck conventions). |

## The `deck.*` API surface

Inside `execute_code`, the sandbox exposes (canonical registry — the tool
description lists the exact current set):

- **Deck**: `deck.info`, `deck.view`, `deck.screenshot`, `deck.metadata.update`
- **Slides**: `deck.slides.list / search / read / readMany / retrieve / create / update / delete`
- **Layers**: `deck.layers.list / find / read / create / update / delete / batchUpdate`, plus geometry helpers `bounds`, `overlaps`, `align`, `distribute`
- **Layouts**: `deck.layouts.list / read / create / delete`, layout layers, and `deck.layouts.placeholders.list / fill`
- **Themes**: `deck.theme.read / list / select / create / update / edit / append`, chart-color normalizers
- **Images**: `deck.images.generate` — AI image generation directly into layers
- **Comments**: `deck.listComments / createComment / resolveComment / unresolveComment / deleteComment`

## How to work

1. **Read before writing.** Start with `deck.info` and `deck.slides.list` to
   learn the deck's theme, layouts, and existing content. Match the deck's
   established style — don't impose a new one uninvited.
2. **Mutate in small batches.** Make 2–3 slide modifications, then **stop and
   review**: call `get_slide_screenshot` and evaluate as a senior designer —
   spacing (clear visual rhythm?), typography (hierarchy, readable sizes),
   contrast, alignment (shared lanes), clipping (text overflowing layer
   bounds is invisible to the viewer — estimate fontSize × lineHeight ×
   lineCount and size the layer to fit), and balance (vary scale and layout
   between slides). Fix what you find before moving on.
3. **Use `get_guide` instead of guessing.** Layer payloads, text styling, and
   chart options have exact schemas — load the guide for the operation you're
   about to do.
4. **Describe your work naturally** — "I'll take a look at the deck", "making
   those changes now" — rather than naming tools or narrating steps.

The user sees every change live, so there is no separate publish step: when
your code commits, the deck is already updated for everyone viewing it.
