# WordPress block plugin guidelines

## Architecture
- Treat `block.json` as the single source of truth for block metadata and asset handles.
- Prefer dynamic blocks with server-side PHP render callbacks when output depends on runtime data, permissions, or sanitised attributes.
- Keep editor logic in React/JSX components and keep front-end assets limited to what the block actually needs.

## JavaScript and block registration
- Use Gutenberg Block API v3 and ensure the editor root element uses `useBlockProps()`.
- Use `@wordpress/block-editor`, `@wordpress/components`, and `@wordpress/i18n` instead of ad-hoc replacements.
- Wrap every user-facing string with `__()`, `_x()`, `sprintf()`, or related i18n helpers and use the correct text domain.
- Define block attributes explicitly in `block.json`, including `type`, defaults where needed, and stable attribute names.

## PHP, rendering, and REST APIs
- Follow WordPress Coding Standards and add PHPDoc to public hooks, functions, and classes.
- Validate and sanitise all block attributes before use. Escape output with the correct context-specific function such as `esc_html()`, `esc_attr()`, `esc_url()`, or `wp_kses_post()`.
- Ensure dynamic render callbacks handle missing or malformed attributes safely.
- For REST API endpoints, always register a `permission_callback`, validate request arguments, and return standard WordPress REST responses.

## Performance and accessibility
- Prefer native blocks, block supports, and theme.json-compatible features before adding custom runtime logic.
- Keep markup semantic, keyboard accessible, and compatible with assistive technology. Use ARIA only when native semantics are insufficient.
- Avoid unnecessary JavaScript, duplicate asset loading, and oversized dependencies in both editor and front-end bundles.
