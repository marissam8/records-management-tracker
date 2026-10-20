# Rebuilding the system

Requires a Microsoft 365 business plan with SharePoint and Power Automate.

1. **Create the site.** In SharePoint, create a private Team site named *Records Management Tracker*. Set its time zone in Regional settings.
2. **Import the lists.** For each file in [`../data`](../data), select **+ New → List → From CSV**. The dates in the files include a noon time so they can't shift a day during import.
3. **Fix the choice columns.** In List settings, set every choice column to **Drop-Down Menu**; the import makes them checkboxes. Refill any choice list that comes through empty.
4. **Configure views and formatting** as described in [schema.md](schema.md).
5. **Build the flows** in [flows.md](flows.md).
6. **Build the dashboard.** Create a page with List web parts for the Overdue, Pending release, Open follow-ups and Due back soon views, plus a Quick chart of requests by type. Set it as the site home page.
7. **Lock down access.** Keep the site private. In production, subpoena records belong in a list with unique permissions limited to records staff and Legal.
8. **Document changes** in the System Change Log as each piece is built.

## Lessons learned

- **CSV imports rename columns.** Internal names become `field_1`, `field_2` and so on. Look them up before writing any expression.
- **Imported choice columns allow multiple selections.** That turns values into lists, which makes flows add unwanted loops and makes comparisons fail. Switch them to drop-down.
- **Dynamic content can land as plain text.** If a condition never matches, open its Code view: a value like `"RequestType Value"` in quotes is typed text, not a reference. Replace it with an expression.
- **Mixing typed text and expressions in a filter is fragile.** Building the whole Filter Query as one `concat()` expression is more reliable.
- **Approvals must go to a user in the organization**, and the first approval run in a new environment sets up Approvals' storage automatically.
- **Logging beats email for audit trails.** Writing decisions to a list creates a permanent, searchable record and works even without a mailbox.
