# WordPress block theme guidelines

## Theme structure
- Use `theme.json` for global settings, design tokens, spacing, typography, colour, and block style configuration.
- Keep templates, template parts, and patterns small, composable, and focused on native block composition.
- Prefer block style variations and theme.json settings over bespoke PHP or JavaScript behaviour.

## Block-first implementation
- Use semantic heading order and meaningful landmark structure across templates and patterns.
- Reuse native blocks whenever possible and keep any custom markup aligned with core block expectations.
- Ensure all translatable strings use WordPress i18n functions with the correct text domain.

## PHP and integrations
- Follow WordPress Coding Standards in any PHP entry points, hooks, or render logic.
- Sanitize incoming data, escape output for the correct context, and protect privileged actions with capability checks and nonces where relevant.
- If a theme includes REST endpoints or dynamic rendering, require explicit validation, sanitisation, and `permission_callback` coverage.

## Performance and accessibility
- Prioritise semantic HTML, keyboard access, and minimal front-end JavaScript.
- Avoid loading assets globally when they are only needed for a specific template, pattern, or feature.
- Favour maintainable native WordPress capabilities over custom abstractions that increase long-term support cost.
