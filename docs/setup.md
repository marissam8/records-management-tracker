# Rebuilding the system

Requires a Microsoft 365 business plan with SharePoint and Power Automate.

1. **Create the site.** In SharePoint, create a private Team site named *Records Management Tracker*.
2. **Import the lists.** For each file in [`../data`](../data), select **+ New → List → From CSV**. Set date columns to Date and the columns listed in [schema.md](schema.md) to Choice. Name the lists *Requests*, *Communications Log*, *Resources* and *System Change Log*.
3. **Configure columns and views** as described in [schema.md](schema.md), including the overdue row formatting.
4. **Build the flows** in [flows.md](flows.md).
5. **Build the dashboard.** Create a page with List web parts for the Overdue, Pending release, Open follow-ups and Due back soon views, plus a Quick chart of requests by type. Set it as the site home page.
6. **Lock down access.** Keep the site private. In production, subpoena records belong in a list with unique permissions limited to records staff and Legal.
7. **Document changes** in the System Change Log as each piece is built.
