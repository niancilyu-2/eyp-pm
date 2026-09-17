# ERPNext design notes

## Established visual patterns

ERPNext screens are Frappe "Desk" screens, so nearly every pattern is inherited. At 1440×900 you will see:

- A fixed 48px top navbar with global search ("awesome bar"), notifications bell, and user menu.
- A left sidebar of workspaces and shortcuts; workspaces (Stock, Buying, Home) are grids of shortcut cards, number cards, and charts.
- List views: compact 30px rows, checkbox column, filter bar, and a coloured status indicator (dot or pill) per row, for example Draft, To Receive and Bill, Completed, Cancelled.
- Form views: page head with title, status and primary action; fields in tabs and sections; child tables (item lines) as an inline editable grid; a right-hand sidebar for assignments, tags and attachments; an activity timeline at the bottom recording saves, submits, comments and workflow actions.
- Submittable documents move Draft, Submitted, then Cancelled or Amended; amending creates a new numbered version rather than editing history.
- Base font is Inter, 16px root size. Light and dark themes switch via a `data-theme` attribute.
- A newer "Espresso" token set (colours, spacing, typography, shadows, borders, `.es-*` components such as `.es-badge`) is being phased in; `.indicator-pill` is marked deprecated.

## Where the design system lives

Paths are relative to the sparse clone; "Verified" means confirmed at the pinned commit.

Frappe (companion), Desk CSS and tokens:
- `frappe/public/scss/espresso/`: `_colors`, `_spacing`, `_typography`, `_shadows`, `_borders` token partials. Verified.
- `frappe/public/css/espresso/`: compiled token CSS (`colors.css`, `spacing.css`, `typography.css`, `radius.css`, `effects.css`) and `components/`. Verified.
- `frappe/public/scss/common/css_variables.scss` and `frappe/public/scss/desk/css_variables.scss`: CSS variables for padding, margins, navbar height, input height, list row height, breakpoints (`--xxl-width: 1440px`). Verified.
- `frappe/public/scss/desk/`: per-surface styles: `navbar.scss`, `sidebar.scss`, `list.scss`, `form.scss`, `form_sidebar.scss`, `timeline.scss`, `dark.scss`. Verified.
- `frappe/public/scss/common/indicator.scss`, `buttons.scss`, `controls.scss`, `modal.scss`: status indicators, buttons, inputs, dialogs. Verified.

Frappe, shared JS components:
- `frappe/public/js/frappe/form/`: `form.js`, `toolbar.js`, `grid.js`, `workflow.js`, `sidebar/`, `footer/form_timeline.js`, `controls/`. Verified.
- `frappe/public/js/frappe/list/`: `list_view.js`, `list_filter/`, `list_settings.js`. Verified.
- `frappe/public/js/frappe/ui/`: `page.js`, `dialog.js`, `toolbar/`, `sidebar/`, `notifications/`, `theme_switcher.js`. Verified.
- `frappe/public/js/frappe/views/workspace/`: workspace rendering. Verified.

Frappe, icons:
- `frappe/public/icons/lucide/icons.svg` (SVG sprite) and `frappe/public/icons/desktop_icons/`. Verified.

ERPNext (primary), module-specific UI:
- `erpnext/public/scss/erpnext.scss`: small overrides only. Verified.
- `erpnext/public/js/controllers/` and `erpnext/public/js/utils/`: shared stock and buying form behaviour, including the serial/batch selector dialog. Verified.
- `erpnext/public/icons/desktop_icons/` and `erpnext/public/images/`: module icons and illustrations. Verified.

Public docs for the Desk UI: https://docs.frappe.io/framework/user/en/desk/ and https://docs.frappe.io/framework/user/en/guides/desk.

## Public UI references

- Public demo site (opens on a login page). https://erpnext-demo.frappe.cloud/
- Purchase order form walkthrough. https://docs.frappe.io/erpnext/user/manual/en/purchase-order
- Stock entry form walkthrough. https://docs.frappe.io/erpnext/user/manual/en/stock-entry
- Workspace and sidebar customisation. https://docs.frappe.io/erpnext/user/manual/en/workspace
- Workflow states and actions on a form. https://docs.frappe.io/erpnext/user/manual/en/workflows

## Synthetic data guidance

Use one fictional company, "Harborline Supply Co.", throughout. Warehouses: Main Store, Receiving Bay, Quarantine, Returns. Items: codes like `ITM-0042`, generic descriptions ("M8 hex bolt, zinc"), fake batch numbers. Suppliers: invented names ("Kestrel Fasteners Ltd"), no real addresses. Purchase orders: `PUR-ORD-2026-00017` style numbers, round quantities, one fictional currency. Users: fictional people with real ERPNext role names (Stock User, Stock Manager, Purchase User, Purchase Manager). Never use real supplier names, price lists, barcodes, or colleagues.

## Prototype scope reminders

- One single-file HTML prototype per lane: inline CSS and JS, no build step or external dependencies.
- Cover one main path plus two alternate states (empty, error, or another role's view).
- Lay out for 1440×900; do not design for mobile.
- Match the Desk's shape (navbar, sidebar, page head, list or form body) so reviewers recognise it; visual similarity is not a completion gate.
- Do not copy CSS, JS, or icon files from either repository; approximate them.
