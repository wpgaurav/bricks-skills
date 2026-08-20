# Theme Styles, Breakpoints & CSS Pipeline

Site-wide styling defaults and how settings become CSS. Verified against `includes/theme-styles.php`, `includes/breakpoints.php`, `includes/assets.php` (2.3.6).

## Breakpoints

Defaults (option `bricks_breakpoints` when customized):

| Key | Width | Media query |
|-----|-------|-------------|
| `desktop` | base (1279 builder width) | none — base styles |
| `tablet_portrait` | 991 | `max-width: 991px` |
| `mobile_landscape` | 767 | `max-width: 767px` |
| `mobile_portrait` | 478 | `max-width: 478px` |

- Desktop-first by default: bare key = desktop, suffixed keys cascade down.
- Custom breakpoints get their own keys; a site can switch to **mobile-first** (base = smallest, `min-width` queries) — check before authoring responsive JSON for an unfamiliar site.
- Settings-key grammar: `key:breakpoint:pseudo` (see style-settings.md).

## Theme styles

Option `bricks_theme_styles`. Shape:

```json
{
  "mystyle1": {
    "label": "Main",
    "settings": {
      "general": { "siteBackground": { "color": { "hex": "#ffffff" } } },
      "colors": { "colorPrimary": { "hex": "#1d4ed8" }, "colorSecondary": { "hex": "#0f172a" } },
      "typography": {
        "typographyBody": { "font-family": "Inter", "font-size": "1rem", "line-height": "1.6", "color": { "hex": "#334155" } },
        "typographyHeadings": { "font-family": "Inter", "font-weight": "700", "line-height": "1.1" },
        "typographyHeadingH1": { "font-size": "3rem" },
        "h1Margin": { "bottom": "1rem" }
      },
      "links": {
        "typography": { "color": { "hex": "#1d4ed8" } },
        "typography:hover": { "color": { "hex": "#1e40af" } }
      },
      "button": {
        "primaryBackground": { "hex": "#1d4ed8" },
        "primaryTypography": { "color": { "hex": "#ffffff" } },
        "primaryBorder": { "radius": { "top": "8px", "right": "8px", "bottom": "8px", "left": "8px" } }
      },
      "container": { "width": "1200px" },
      "conditions": { "conditions": [ { "id": "cond1", "main": "any" } ] }
    }
  }
}
```

### Control keys are NOT guessable

`update_option( 'bricks_theme_styles', … )` accepts **any** key. Bricks reads
only the ones it knows, so an invented key is stored, survives a re-read, and
renders nothing — the option says the style is applied and the page disagrees.

The authoritative list is one file per group in the theme:

```
wp-content/themes/bricks/includes/theme-styles/controls/*.php
```

Names that read plausibly but do **not** exist:

| Wrong | Right | Group |
|---|---|---|
| `general.containerWidth` | `container.width` | container |
| `colors.primary` / `.secondary` | `colors.colorPrimary` / `colorSecondary` (also `colorLight`, `colorDark`, `colorMuted`, `colorBorder`, `colorInfo`, `colorSuccess`, `colorWarning`, `colorDanger`) | colors |
| `general.backgroundColor` | `general.siteBackground` (a `background` control → `{color:{hex}}`) | general |
| `typography.fontFamily` | `typography.typographyBody['font-family']` | typography |
| `typography.h1Typography` | `typography.typographyHeadingH1` | typography |
| `typography.bodyTypography` | `typography.typographyBody` | typography |
| `typography.linkColor` | `links.typography.color` | links |
| `button.backgroundColor` | `button.primaryBackground` | button |
| `button.border` | `button.primaryBorder` | button |

Hover and breakpoint variants use a `:` suffix on the **key**, same grammar as
element settings: `primaryBackground:hover`, `typographyBody:mobile_portrait`.

Two gotchas when grepping the control files yourself:

- `typographyHeadingH1`–`H6` and `h1Margin`–`h6Margin` are real literals, but a
  pattern like `grep -oE "\$controls\['[a-zA-Z]+'\]"` drops them — `[a-zA-Z]+`
  does not match the digit. Include `0-9`.
- Colour values want `{hex, id, name}` where `id` is the **palette colour id**.
  A bare `{hex}` renders correct CSS but the Styles Manager picker has nothing
  to bind to, so it shows an empty swatch and the value stops tracking the
  palette. Bound colours compile to `var(--bricks-color-<id>)`; unbound ones to
  a literal — that is how you tell.

The most reliable reference is a theme style **Bricks itself authored** on a
real site: `wp option get bricks_theme_styles --format=json`.

Groups (each is a file under `includes/theme-styles/controls/`): `general` (site layout/background/border, lightbox), `colors`, `typography` (body, all-headings, per-heading H1–H6, hero, lead, blockquote), `links`, `content`, `contextualSpacing`, `popups`, `css`, plus per-element groups named `element-*` but keyed without the prefix (`section`, `container`, `block`, `button`, `heading`, `form`, `image`…).

Conditions nest under their own key — `"conditions": { "conditions": [ … ] }` — and each entry needs an `id`. **A style with no conditions applies to nothing, silently.** Same shape as template conditions otherwise (`{ "main": "any" }`, `postType`, `ids`…). Multiple styles can apply; specificity: site < post type < template < specific ids.

**Authoring rule:** lean on theme styles for site identity (type scale, container width, button colors) and keep generated layout JSON about *structure*. When the user asks for a full design system, deliver a theme-styles JSON snippet alongside layout JSON.

## CSS generation order (inline or file)

1. `:root` global variables
2. Theme styles (matching conditions)
3. Global classes
4. Color palette variables
5. Page settings CSS
6. Header / content / footer element CSS
7. Popup CSS
8. Global custom CSS (Settings → Custom code)

Later wins at equal specificity — element settings beat global classes, which beat theme styles. `_cssCustom` outputs verbatim where the element's CSS lands.

## CSS loading methods

Bricks Settings → Performance:
- **Inline** (default): per-page `<style>`.
- **External files**: generated under `wp-content/uploads/bricks/css/` (`post-{id}.min.css`, theme styles, global classes…). After programmatic content changes, run `wp bricks regenerate_assets`.

## Page-level CSS/JS

Postmeta `_bricks_page_settings`:

```json
{
  "customCss": ".hero { background-blend-mode: multiply; }",
  "customScriptsHeader": "<script>…</script>",
  "customScriptsBodyHeader": "",
  "customScriptsBodyFooter": "<script>…</script>"
}
```

## Custom fonts

- Google fonts: set `font-family` by name — Bricks enqueues automatically (mind GDPR setting that disables remote Google fonts).
- Custom fonts post type (`bricks_fonts`): upload woff2; then `font-family` is the custom font's name.
- Adobe fonts via project ID in settings.
- Variable axes: typography supports `font-variation-settings` as a property key.

## Performance defaults worth setting in generated sites

- Disable unused icon fonts (Settings → Performance) — prefer SVG icons.
- `loading: "lazy"` on below-fold images (default), `eager` + `fetchpriority` for LCP hero images (via `_attributes`).
- Element CSS files method for heavily cached sites; inline for small ones.
