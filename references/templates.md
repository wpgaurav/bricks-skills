# Templates

Template types, conditions, storage, and import/export. Verified against `includes/templates.php` + `includes/database.php` (2.3.6).

## Template types (`_bricks_template_type` / export `type`)

| Type | Renders |
|------|---------|
| `header` | site header (elements go in export key `header`) |
| `footer` | site footer (export key `footer`) |
| `content` | generic content template |
| `section` | reusable section (insertable) |
| `popup` | popup (see popups.md) |
| `single` | single post/page layout |
| `archive` | post-type/term/author/date archives |
| `search` | search results |
| `error` | 404 page |
| `password_protection` | password form page |
| `wc_archive` `wc_product` `wc_cart` `wc_cart_empty` `wc_form_checkout` `wc_form_pay` `wc_thankyou` `wc_order_receipt` `wc_account_*` | WooCommerce (see woocommerce.md) |

Templates are posts of type `bricks_template`. Taxonomies: `template_tag`, `template_bundle`.

## Template conditions (`templateSettings.templateConditions`)

Array — template applies where **any** condition matches; `exclude: true` entries subtract.

```json
"templateSettings": {
  "templateConditions": [
    { "main": "any" },
    { "main": "postType", "postType": ["post", "page"] },
    { "main": "archiveType", "archiveType": ["postType"], "archivePostTypes": ["project"] },
    { "main": "terms", "terms": ["category::12", "post_tag::all"] },
    { "main": "ids", "ids": [42, 77], "idsIncludeChildren": true },
    { "main": "frontpage" },
    { "main": "search" },
    { "main": "error" },
    { "main": "ids", "ids": [99], "exclude": true }
  ]
}
```

| `main` | Extra keys |
|--------|-----------|
| `any` | — (entire website) |
| `frontpage` | — |
| `postType` | `postType: ["post", ...]` (singular views) |
| `archiveType` | `archiveType: ["any"\|"postType"\|"author"\|"date"\|"term"]`, `archivePostTypes`, `archiveTerms` (`"tax::term_id"` or `"tax::all"`), `archiveTermsIncludeChildren` |
| `terms` | `terms: ["category::12"]` (singular posts with the term) |
| `ids` | `ids: [42]`, `idsIncludeChildren` |
| `search` / `error` | — |
| `hook` | `hookName`, `hookPriority` — render a section template at any theme hook |

### Preview context (builder only)
`templatePreviewType` (`single` `archive-term` `archive-cpt` `archive-author` `archive-date` `search` `error`), `templatePreviewPostId`, `templatePreviewTerm` (`"tax::id"`), `templatePreviewPostType`, `templatePreviewAuthor` — set these so the builder shows real data; harmless to include in exports.

## Header behavior settings (template settings on header templates)

| Key | Effect |
|-----|--------|
| `headerSticky` | `true` — sticky header (`.brx-sticky`) |
| `headerStickyOnScroll` | static on load, fixed after scrolling |
| `headerStickySlideUpAfter` | px — hide on scroll down, reveal on scroll up |
| `headerStickyScrollingBackground` | background object when scrolled (`.scrolling` state) |
| `headerStickyScrollingColor` / `ColorHover` | text colors when scrolled |
| `headerStickyScrollingBoxShadow` | shadow when scrolled |
| `headerStickyTransition` | default `"background-color 0.2s, transform 0.4s"` |
| `headerPosition` | `left` / `right` vertical header (+ `headerWidth`) |

Logo swap on sticky: `logo` element's `logoInverse`.

## Archive / single template essentials

- **Single**: use `post-title`, `post-content` (or `text` with `{post_content}` — prefer the element), `post-meta`, `featured_image` via image element + dynamic data, `post-navigation`, `related-posts` or a loop with `exclude_current_post`.
- **Archive**: a query loop with `"is_archive_main_query": true` so WP pagination/SEO work; `{archive_title}` + `{archive_description}` up top.
- **Search**: same as archive + `{search_term}`; add a `search`/`filter-search` element for refinement.
- **404 (`error`)**: static section + `search` element + popular-posts loop.

## Storage & programmatic creation

See json-formats.md for full meta-key table and code. Short version:

```php
$id = wp_insert_post( [ 'post_type' => 'bricks_template', 'post_status' => 'publish', 'post_title' => 'Main Header' ] );
update_post_meta( $id, '_bricks_template_type', 'header' );
update_post_meta( $id, '_bricks_page_header_2', wp_slash( $elements ) );   // header templates use the header meta key; wp_slash keeps backslashes
update_post_meta( $id, '_bricks_template_settings', wp_slash( [ 'templateConditions' => [ [ 'main' => 'any' ] ] ] ) );
update_post_meta( $id, '_bricks_editor_mode', 'bricks' );
```

Element meta key by type: `header` → `_bricks_page_header_2`, `footer` → `_bricks_page_footer_2`, everything else → `_bricks_page_content_2`.

## Import/export behavior

- **Export** (Bricks → Templates → row action): produces the JSON in json-formats.md §2; bundles global classes/variables the template uses.
- **Import** accepts `.json` or `.zip` of several JSONs. It regenerates element IDs, merges global classes (id match → keep local; name match → remap; else add), imports remote images to the media library (tracks origin URL in `_bricks_image_origin_url`), creates missing pseudo-class entries, regenerates CSS files.
- The `template` element (`{ "template": 123 }`) embeds a section template by ID — useful for shared CTAs across templates.

## Render hooks

```php
// Inject sections without templates
add_action( 'bricks_after_header', fn() => ... );
add_action( 'bricks_before_footer', fn() => ... );

// Or condition-based via 'hook' templateCondition (no code)
```

`{template}` rendering order: header template → content (page or content-type template) → footer template; popups render on matching pages.
