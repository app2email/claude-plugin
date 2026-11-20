---
name: app-setup
description: Change the setup of the app2email apps on a WhatsApp number - Book-and-Pay (services, hours, staff, booking settings), Tickets-and-Pay (events, dates, ticket types, seat plans, ticket settings) and Order-and-Pay (shop settings, catalog, items, prices, stock). Use when the user wants to set up, change, copy, back up or undo the configuration of bookings, event tickets or the WhatsApp shop.
---

# Set up the app2email apps

Use the tools of the **app2email** connector. They work on the one WhatsApp number the user connected, and changing the apps needs a connection with "everything" access.

## Small changes: the app's own tools

- **Book-and-Pay:** `get_booking_settings`, then `update_booking_settings` (also switches Book-and-Pay on or off). Day-to-day work: `list_booking_services`, `get_calendar`, `find_next_booking_slots`, `create_time_block`, `delete_time_block`.
- **Order-and-Pay:** `get_shop_settings`, `update_shop_settings`; the catalog with `list_shop_categories`, `search_shop_items`, `get_shop_item`, `create_shop_item`, `update_shop_item`, `archive_shop_item`, `set_shop_stock`, `set_shop_sold_out`.
- **Tickets-and-Pay:** `list_ticket_events`, `get_ticket_event`, `get_ticket_availability`, `get_ticket_stats`. Events, ticket types, seat plans and ticket settings are changed with a setup file (below).

Read the current value first, change only what the user asked for, and read it back afterwards.

## Bigger changes: a setup file

A setup file holds the whole setup of one or more apps as JSON Lines, one record per line (`{"type": ..., "key": ..., ...fields}`). It carries the apps' settings and records (for Book-and-Pay its settings sections, services and staff; for Tickets-and-Pay its events, dates, ticket types and seat plans; for Order-and-Pay its settings and catalog). Payment provider keys are never part of a file: the user enters those in the dashboard.

1. **Learn the format.** Call `get_setup_guide` once: it lists the record types and fields of each app, the rules, the limits and examples. Do not guess field names.
2. **Export.** `export_setup` with `products` set to the apps concerned (`booking`, `tickets`, `shop`, comma separated). Pictures (logos, item photos, a seat plan's floor picture) come as links by default.
3. **Change.** Edit only the records and fields the user asked about. Keep each record's `key`: it is how the record is matched. In the default `merge` mode, records you leave out stay as they are, so a file with just the changed records is enough.
4. **Check.** `check_setup` with the file (`jsonl` text or `lines`). Nothing changes. It lists every problem with its line number and what an apply would create, change and remove. Fix the problems and check again until `ok` is true.
5. **Confirm.** Show the user the summary of what will be created, changed and removed, and get a clear yes.
6. **Apply.** `apply_setup` with the `check_id` of the passing check: it applies exactly what was checked. `enable: true` also switches the apps of the file on.
7. **Report.** Give the job id and what changed. `list_setup_jobs` and `get_setup_job` show past checks and applies.

Be careful with these options:

- `mode: "replace"` also removes every record of the file's types that the file does not name. Use it only when the user wants the file to be the complete setup, and point out each removal from the check.
- `if_unchanged: true` refuses the apply when someone changed the setup after the export. Use it when the user may be editing in the dashboard at the same time.
- Order-and-Pay stock figures are left alone unless `stock: "set"` is given.

## Pictures

To add a new picture, upload it with `upload_setup_asset` (PNG, JPEG or WEBP up to 5 MB, by `url` or base64 `data`). It is kept for 7 days. Then name it in the file by its sha256, as the guide shows.

## Undo

`undo_setup` with the apply's job id puts back what that apply changed, within 30 days and while no later apply changed the same apps. Confirm with the user first.

## Copy a setup to another number

Export from one number, then connect the app2email connector to the other number and check and apply the same file there. If the check reports a picture it cannot fetch, export again with `assets: "inline"`, or upload the picture to the other number with `upload_setup_asset`.
