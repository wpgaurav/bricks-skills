# Bricks Builder Skill

[![Buy me a coffee](https://img.shields.io/badge/Buy%20me%20a%20coffee-FFDD00?style=flat&logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/gauravtiwari)

An AI skill for one-shotting [Bricks Builder](https://bricksbuilder.io/) designs as JSON — hero sections, full landing pages, headers/footers, query loops with faceted filters, ACF-driven layouts, conditions, animations, and popups. Every settings key and value shape is verified against the Bricks **2.3.6** theme source, not guessed.

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
