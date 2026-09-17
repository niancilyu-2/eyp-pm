# ERPNext orientation

## What the product is

ERPNext is a free, open-source business management system (an "ERP": software that keeps a company's stock, purchasing, sales, and money records in one place). It runs in a web browser. Its screens are built on the Frappe framework, a separate open-source toolkit that supplies the building blocks of every screen: forms, list views, workflows (approval chains), and role-based permissions (rules about who may see or change a record). Companies self-host it or use the paid Frappe Cloud version. Typical users are small and mid-sized manufacturers, distributors, retailers, and service firms.

You will read both pinned repositories but only propose changes to ERPNext.

## Who the persona is

An inventory or procurement operations manager. They work in ERPNext most of the day, own the accuracy of stock counts and purchase records, and are the person colleagues escalate to when something looks wrong: a quantity that does not match the shelf, a purchase order stuck waiting for someone, a receipt without paperwork. Their mandate is to resolve those exceptions without weakening controls or auditability: any fix must leave a clear trail of who changed what and why, and must not let anyone bypass required approvals. They know ERPNext's vocabulary (Stock Entry, Material Request, Purchase Order, Workflow) but are not developers.

## The three lanes

**Inventory exceptions** (`inventory-exceptions`). Product area: the Stock module, specifically stock entries, stock reconciliations, material requests, warehouses, stock settings, and the stock ledger, stock balance, and stock ageing reports. Surface: desktop web at 1440×900. Repo folders: `sources/primary/erpnext/stock/doctype/` (one folder per record type, each with a `.json` layout, `.py` server logic, and `.js` form behaviour), `sources/primary/erpnext/stock/report/`, `sources/primary/erpnext/controllers/stock_controller.py`, and `sources/primary/erpnext/public/js/controllers/stock_controller.js`.

**Purchase-approval exceptions** (`purchase-approval-exceptions`). Product area: the Buying module (purchase orders, suppliers, buying settings, requests for quotation) plus the approval machinery that sits under it: Frappe workflows and ERPNext authorization rules. Surface: desktop web at 1440×900. Repo folders: `sources/primary/erpnext/buying/doctype/`, `sources/primary/erpnext/setup/doctype/authorization_rule/`, `sources/primary/erpnext/controllers/buying_controller.py`, plus `sources/companion/frappe/workflow/`, `sources/companion/frappe/model/workflow.py`, and `sources/companion/frappe/public/js/frappe/form/workflow.js`.

**Role-based daily navigation** (`role-based-daily-navigation`). Product area: what a signed-in user sees and clicks between tasks: the home, Stock, and Buying workspaces, left sidebar, top navbar and search, notifications, and list-view defaults. Surface: desktop web at 1440×900. Repo folders: `sources/primary/erpnext/workspace_sidebar/`, `sources/primary/erpnext/desktop_icon/`, the `workspace/` folders inside `erpnext/stock`, `erpnext/buying`, and `erpnext/setup`, plus `sources/companion/frappe/desk/`, `sources/companion/frappe/public/js/frappe/views/workspace/`, `sources/companion/frappe/public/js/frappe/ui/`, and `sources/companion/frappe/core/doctype/role/`.

## Repository map

Both repositories are Python server code plus JavaScript forms; the Desk UI comes from the Frappe framework.

Primary, `sources/primary/` (frappe/erpnext):
- `erpnext/stock`: stock record types, reports, dashboards, and the Stock workspace.
- `erpnext/buying`: purchasing record types, reports, and the Buying workspace.
- `erpnext/setup`: company, item group, UOM, authorization rules, and the home workspace.
- `erpnext/public`: ERPNext's own JavaScript form controllers and small stylesheet additions.
- `erpnext/controllers`: shared server logic that buying and stock documents inherit.
- `erpnext/workspace_sidebar`: JSON definitions of the left sidebar per module.
- `erpnext/desktop_icon`: JSON definitions of module icons on the home screen.

Companion, `sources/companion/` (frappe/frappe):
- `frappe/public`: Desk JavaScript (forms, lists, toolbar, sidebar, workspaces), SCSS design tokens, and icons.
- `frappe/desk`: server side of forms, lists, workspaces, notifications, and search.
- `frappe/workflow`: the workflow engine record types and the visual workflow builder page.
- `frappe/core`: users, roles, role profiles, permission rules, and the Permission Manager page.
- `frappe/model`: the document lifecycle, including where workflow and permission checks are applied.

Cone-mode sparse checkout also keeps files directly inside `frappe/`, so `frappe/permissions.py` and `frappe/hooks.py` will appear.

## Where to start public research

1. **Community forum, discuss.frappe.io.** Highest volume of real user questions. Search phrases such as "stock reconciliation negative", "purchase order workflow stuck", "role cannot see", "workspace sidebar"; sort by latest and read the replies, not just the opening post.
2. **GitHub issues, frappe/erpnext.** Filter by labels `stock` and `buying`; try `is:issue is:open "material request"`. Note which issues were closed as "working as intended" versus fixed.
3. **Official docs, docs.frappe.io/erpnext.** Read the Stock and Buying manual pages for your lane's record types first; they say what the product is supposed to do.
4. **GitHub issues, frappe/frappe.** For workflow, permission, list-view, and sidebar behaviour, search here instead of the ERPNext repo.
5. **Release notes, github.com/frappe/erpnext/releases.** Scan the last few releases for anything touching your lane so you do not propose what already shipped.
6. **Review sites.** Use for tone and recurring themes only; ratings are not evidence.

## Cautions

- Use public information only. Do not import data, screenshots, or process descriptions from a real employer or client.
- Excluded topics: tax, payroll, accounting policy, regulatory or compliance claims, anything that changes accounting ledger entries, and country-specific compliance. If a finding leads there, note it and stop.
- ERPNext is GPL-3.0 and Frappe is MIT. That permits reading; it does not permit copying source files, snippets, or icon assets into your outputs. Describe, do not paste.
- Both repositories are pinned at 2026-09-17; the live product, demo, and docs may have moved on. Say which you are describing.
- The repositories contain `bench` commands, install scripts, and setup wizards. Do not run them; you are reading code, not installing an ERP.
- This is the advanced case. Two repositories, a framework layer, and an approval engine mean more places to get lost. Start from your lane's entry points and widen only when a specific question forces you to.
