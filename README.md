# Bricks Builder Skill

[![Buy me a coffee](https://img.shields.io/badge/Buy%20me%20a%20coffee-FFDD00?style=flat&logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/gauravtiwari)

An AI skill for one-shotting [Bricks Builder](https://bricksbuilder.io/) designs as JSON — hero sections, full landing pages, headers/footers, query loops with faceted filters, ACF-driven layouts, conditions, animations, and popups. Every settings key and value shape is verified against the Bricks **2.3.6** theme source, not guessed.

**Want the agent to edit the live site, not just hand you JSON?** Pair this skill with [Site Agent](https://gauravtiwari.org/product/site-agent/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills), my free WordPress MCP plugin that runs Bricks' own abilities on Bricks 2.4+. [How it works](#push-it-into-a-live-bricks-site).

## What's inside

```
bricks-skills/            ← the repo is the skill
├── SKILL.md              ← start here: workflow, rules, routing table
├── references/           ← 20 focused references (loaded on demand)
│   ├── json-formats.md       clipboard / template / postmeta formats + validation
│   ├── style-settings.md     every _setting + exact JSON value shapes
│   ├── elements.md           full element catalog + nestable structures
│   ├── layout-recipes.md     grids, heroes, cards, overlap, sticky, full pages
│   ├── query-loops.md        hasLoop + query JSON for posts/terms/users
│   ├── query-filters.md      live search, facets, sort, AJAX pagination
│   ├── dynamic-data.md       {tags}, filters, args, {echo:}
│   ├── acf-providers.md      ACF deep dive (repeaters, groups, flexible) + Meta Box etc.
│   ├── conditions.md         _conditions JSON (incl. dynamic data conditions)
│   ├── interactions.md       _interactions, full animation list, recipes
│   ├── popups.md             popup settings, triggers, limits, AJAX context
│   ├── templates.md          template types, conditions, header behaviors
│   ├── components-classes.md components, global classes/variables, palettes
│   ├── theme-styles.md       theme styles, breakpoints, CSS pipeline
│   ├── external-assets.md   reference remote images/video/SVG/icons by URL
│   └── forms / woocommerce / hooks / custom-elements / assets-permissions
└── patterns/              ← 17 validated, paste-ready JSON files (see INDEX.md)
```

## Install & use

The repo **is** the skill: a [SKILL.md](SKILL.md) router → on-demand [references/](references/) → paste-ready [patterns/](patterns/). Give it to any AI tool two ways:

- **Point at it** — clone the repo, then aim your tool's instruction/rules file at `SKILL.md` (it routes to the right reference on demand).
- **Paste it** — drop `SKILL.md` + the one reference your task needs into the chat. The references are self-contained.

Grab the files once:

```bash
git clone https://github.com/wpgaurav/bricks-skills.git
```

### Claude Code

Install as a global skill (available in every project):

```bash
git clone https://github.com/wpgaurav/bricks-skills.git ~/.claude/skills/bricks
```

Claude auto-loads it whenever your request looks like a Bricks task — just ask. Update later with `git -C ~/.claude/skills/bricks pull`. For one project only, clone into `.claude/skills/bricks/` inside that project — or simply run `claude` inside the repo (its `CLAUDE.md` routes automatically).

### Claude.ai (web & desktop)

**As a Skill** — Pro/Max/Team/Enterprise with *code execution* enabled:

1. Zip the folder: `zip -r bricks.zip bricks-skills -x '*.git*'`
2. **Settings → Capabilities → Skills → Upload skill**, choose `bricks.zip`.
3. Start a chat and ask for a layout.

**Any plan** — create a **Project**, upload `SKILL.md` and the `references/*.md` you need as project knowledge, then add *"Follow SKILL.md for all Bricks tasks"* to the project instructions.

### ChatGPT

**Custom GPT** — Create a GPT → **Configure → Knowledge → Upload files** and add `SKILL.md` plus the reference files (upload `bricks.zip` if you hit the file limit). In **Instructions** paste: *"You author Bricks Builder JSON. Always follow SKILL.md and the matching file in references/."*

**Project / single chat** — attach `SKILL.md` plus the reference for your task (e.g. `references/query-loops.md`) and ask.

### OpenAI Codex

Codex reads `AGENTS.md`. Clone the repo, then add a **pointer** (don't paste the whole skill — `AGENTS.md` is size-capped):

```markdown
# Bricks Builder
For any Bricks Builder task, follow the skill at ./bricks-skills/SKILL.md and its references/.
```

Put that in your project's `AGENTS.md`, or in `~/.codex/AGENTS.md` (with an absolute path to the clone) to make it global.

### Cursor

Clone the repo into your project, then add `.cursor/rules/bricks.mdc`:

```mdc
---
description: Bricks Builder JSON authoring
alwaysApply: false
---
For any Bricks Builder task, follow ./bricks-skills/SKILL.md and the files in ./bricks-skills/references/.
Output paste-ready JSON using the verified value shapes from those docs.
```

Cursor attaches it automatically when relevant, or type `@bricks` to force it. You can also `@`-mention `SKILL.md` directly in chat. (Cursor reads `AGENTS.md` too.)

### GitHub Copilot

Clone the repo into your project, then create `.github/copilot-instructions.md`:

```markdown
For any Bricks Builder (WordPress) work, follow ./bricks-skills/SKILL.md and the matching file
in ./bricks-skills/references/. Always output paste-ready JSON with verified value shapes.
```

Copilot picks it up automatically across VS Code, JetBrains, GitHub.com, and the CLI. For file-scoped rules, use `.github/instructions/bricks.instructions.md` with an `applyTo:` glob. In chat you can also attach `#file:SKILL.md`.

### VS Code

Open the repo (or your project with the clone inside) as a workspace, then use whichever assistant you have:

- **Copilot Chat** — set up `.github/copilot-instructions.md` (above) and/or attach `#file:SKILL.md`.
- **Claude Code / Continue / Cline / other extensions** — `@`-mention or attach `SKILL.md` in the chat, or use the skill / `AGENTS.md` / rules files from the other sections.

Tip: keeping `SKILL.md` open in a tab gives most assistants the context for free.

### OpenCode

Clone the repo, then either reuse the `AGENTS.md` pointer (project, or global `~/.config/opencode/AGENTS.md`) — or list the files in `opencode.json` (globs and remote URLs both work):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["./bricks-skills/SKILL.md", "./bricks-skills/references/*.md"]
}
```

### Any other AI tool

It's all plain Markdown, so it works anywhere — Gemini, Windsurf, Aider, Zed, JetBrains AI, and more:

- **Reads `AGENTS.md`?** (Zed, Aider, Gemini CLI, Jules, Windsurf…) Drop in the one-line pointer to `bricks-skills/SKILL.md`.
- **Otherwise** paste `SKILL.md` into the system prompt / custom instructions, plus the single reference your task needs — the routing table in `SKILL.md` tells you which. Self-contained, no internet required.

## What to ask

> "Build a SaaS landing page: split hero, logo strip, 3-col features, pricing, FAQ, CTA. Brand color #6d28d9."

> "Create a filterable course archive — search, category filter, price sort, AJAX pagination — as paste-ready JSON."

> "Team section from my ACF repeater `team_members` (photo, name, role, linkedin_url)."

You get a `.json` file: paste it into the builder (clipboard format) or import it via **Bricks → Templates** (template format).

## Push it into a live Bricks site

Pasting works for one section. When you want the agent to read the page, change it and check the result itself, connect it to the site with [**Site Agent**](https://gauravtiwari.org/product/site-agent/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills), my free WordPress MCP plugin. It's the first route I'd pick for Bricks, for three reasons:

- **It uses Bricks' own tools.** On Bricks 2.4 and later, switch on Bricks tools and the agent gets Bricks' abilities next to Site Agent's: page summaries, page edits, imports, revisions and CSS regeneration, with Bricks' own permission and input checks in charge.
- **It won't corrupt layout data.** Bricks stores element arrays in post meta. Site Agent's content writes refuse raw Bricks data, so changes go through Bricks' save path and the CSS doesn't go stale.
- **Big pages still fit.** A summary read returns element counts, headings and structure, so the agent can understand a long layout before it touches it.

```json
{
  "name": "site-agent-run-bricks-ability",
  "arguments": {
    "ability_name": "bricks/get-page-elements",
    "parameters": { "postId": 42, "responseFormat": "summary" }
  }
}
```

Clients sign in through WordPress with OAuth (Claude, ChatGPT, Claude Code, Codex, Cursor, VS Code, Gemini, Zed). Tools that change things stay off until you switch them on. On Bricks 2.3.x, generate the JSON with this skill and paste or import it by hand. Staging first, always.

## Tools that pair well with this skill

The skill is free and works without any of this. These are products I build, and each one covers something a layout skill can't.

| You want to | Use | Why it fits |
|-------------|-----|-------------|
| Let the agent edit the site, not just the chat | [**Site Agent**](https://gauravtiwari.org/product/site-agent/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) (free) | Runs Bricks' own abilities over MCP. See [Push it into a live Bricks site](#push-it-into-a-live-bricks-site). |
| Give every section the same look | [**Design Systems Library**](https://gauravtiwari.org/product/design-systems-library/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) | Agent-ready design systems: tokens, a `DESIGN.md` rulebook, components, fonts and live demos. Map the tokens to Bricks global variables and classes once (see [components-classes.md](references/components-classes.md)), and every layout the agent writes uses the same palette, type and radius. |
| Run a WooCommerce store on Bricks | [**Stackless for WooCommerce**](https://gauravtiwari.org/product/stackless-for-woocommerce/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) ([free Lite](https://gauravtiwari.org/product/stackless-lite-for-woocommerce/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills)) | Subscriptions, Stripe, PayPal, Paddle, licensing, multi-currency and invoices in one plugin on core WooCommerce. Build the shop templates with [woocommerce.md](references/woocommerce.md); let Stackless replace the add-on stack behind them. Switched-off modules load no code. |
| Sell digital downloads with Easy Digital Downloads | [**Stackless for EDD**](https://gauravtiwari.org/product/stackless-for-edd/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) ([free Lite](https://gauravtiwari.org/product/stackless-lite-for-edd/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills)) | Subscriptions, licensing, gateways, multi-currency and invoices for EDD in one plugin. Pairs with the query-loop and dynamic-data references for download archives. |
| Keep the finished pages fast | [**GT Performance**](https://gauravtiwari.org/product/gt-performance/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) (free) | Page cache, Cloudflare Free integration, unused CSS removal and Redis, with commerce safeguards. |
| Get more skills like this one | [**AI Skills Library**](https://gauravtiwari.org/product/ai-skills-library/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) or [**All-Access Pass**](https://gauravtiwari.org/product/all-access/?utm_source=github&utm_medium=readme&utm_campaign=bricks-skills) | Writing and visual skills for Claude Code, Codex and Cursor. All-Access adds the Design Systems Library and Notion templates in one yearly pass. |

### More open-source skills

- [GenerateBlocks skills](https://github.com/wpgaurav/generateblocks-skills): GenerateBlocks V2 layouts, conversions and live publishing, verified in the editor
- [Kadence skills](https://github.com/wpgaurav/kadence-skills): Kadence Blocks markup for Gutenberg templates
- [WordPress skills](https://github.com/wpgaurav/WordPress-skills): 125+ skills for WordPress, design, SEO and marketing
- [Claude Code skills](https://github.com/wpgaurav/claude-code-skills): stop-slop, red-pen, polish and other anti-slop skills

## Why this exists

LLMs reliably produce Bricks JSON that *parses* but silently fails: camelCase typography keys, flat box-shadows, invented breakpoint names, accordion items missing their structural classes. This skill pins the exact shapes from the source:

| Silent failure | Verified shape |
|----------------|----------------|
| `"_typography": {"fontSize": "18px"}` | `"_typography": {"font-size": "18px"}` |
| `"_boxShadow": {"offsetY": "8px"}` | `"_boxShadow": {"values": {"offsetY": "8"}, "color": {…}}` |
| `"_gradient": {"stops": […]}` | `"_gradient": {"colors": [{"color": {…}, "stop": "0"}]}` |
| `"_padding:tablet"` | `"_padding:tablet_portrait"` |
| accordion item = plain blocks | title/content wrappers need `_hidden._cssClasses` |

Plus the things docs rarely cover: the exact clipboard wrapper the builder writes, template condition objects, popup setting keys (including Bricks' own `popupJustifyConent` typo), filter element wiring, ACF repeater loop `objectType`s, and the `key:breakpoint:pseudo` settings grammar.

## Patterns

17 validated files in [patterns/](patterns/) — heroes, feature grid, pricing, testimonials slider, FAQ accordion (with FAQ schema), CTA, contact form, ACF team loop, filterable blog archive, header/footer/popup templates, plus three **BEM class-based test builds** (`bem-hero`, `bem-card-grid`, `bem-pricing`) where every style lives in `block__element--modifier` global classes. Each file passes a structural integrity check (tree reciprocity, class references, format keys). See [INDEX.md](patterns/INDEX.md).

## Support This Project

This free, MIT-licensed skill gives your AI tool the exact JSON shapes Bricks Builder expects for heroes, query loops, popups and ACF-driven layouts. I checked every settings key and value shape against the Bricks 2.3.6 theme source and each of the 17 patterns passes a structural integrity check.

If the JSON pasted cleanly into Bricks and you didn't have to chase a wrong typography key or a made-up breakpoint name, you can buy me a coffee.

<a href="https://buymeacoffee.com/gauravtiwari"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy me a coffee" height="50"></a>

You can also star the repo, and if a setting silently goes missing in the builder, please open an issue with your prompt and the JSON the skill produced.

## License

- Skill documentation & patterns: MIT
- Bricks theme: proprietary (not included) — [bricksbuilder.io](https://bricksbuilder.io/)
