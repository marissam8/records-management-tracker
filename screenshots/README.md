# Records Management Tracker

A working records management system built in **SharePoint** and **Power Automate** to track student records requests (transcripts, subpoenas, enrollment verifications, records transfers), route subpoenas for legal approval, and log every decision and daily overdue check automatically.

> **All data in this project is fictional.** No real student information is used, in keeping with FERPA.

**Demo video:** [Watch the walkthrough on Loom](LOOM_LINK_HERE)
**Portfolio site:** [View the live portfolio](https://YOUR-USERNAME.github.io/records-management-tracker/)

## What it does

- **Tracks every request** from intake to completion, with status, due date, requester and release-form status.
- **Assigns IDs and due dates automatically** when a request comes in (for example, `TR-0153`, due in 7 days for transcripts).
- **Blocks release without consent** by moving any request with no signed release to *Pending release*.
- **Routes every subpoena to Legal** through Microsoft Approvals before any records are produced, and writes the decision back to the request.
- **Logs decisions and daily checks automatically** in the Communications Log, so every legal decision and overdue report has a dated record.
- **Flags problems at a glance**: completed requests show green, overdue request IDs show red.
- **Tracks communications and equipment** with schools, central office, parents and outside agencies.
- **Documents every system change** in a change log backed by SharePoint version history.

## How it fits together

```mermaid
flowchart LR
  subgraph Lists[SharePoint lists]
    R[Requests]
    C[Communications Log]
    E[Resources]
    L[System Change Log]
  end
  F1[New request setup: ID, due date, release check] --> R
  R --> F4[Subpoena legal approval]
  F4 --> R
  F4 --> C
  R --> F3[Daily overdue report]
  F3 --> C
  R --> D[Records Dashboard]
  C --> D
  E --> D
```

## Subpoena workflow

```mermaid
flowchart LR
  A[Subpoena logged] --> B[ID and due date assigned]
  B --> C[Marked Pending; approval sent to Legal]
  C --> D{Legal decision}
  D -- Approve --> E[Marked Approved]
  E --> F[Logged: notify parent before producing records]
  D -- Reject --> G[Marked Rejected]
  G --> H[Logged: do not produce records]
```

## Built with

| Tool | Used for |
| --- | --- |
| SharePoint Online lists | Data storage, views, status board, conditional formatting |
| Power Automate | Three automated flows (request setup, legal approval, daily report) |
| Microsoft Approvals | Legal sign-off on subpoenas |
| SharePoint pages | Dashboard with list web parts and a chart |

These are standard Microsoft 365 business tools, so the system can run inside an existing Microsoft 365 environment with no new software or licenses. It needs no email connector: decisions and reports are logged in SharePoint, where they're searchable and permanent.

## Repository contents

| Path | Contents |
| --- | --- |
| [`index.html`](index.html) | Portfolio site (also served by GitHub Pages) |
| [`data/`](data/) | Fictional sample data for the four lists, ready to import |
| [`docs/schema.md`](docs/schema.md) | Lists, columns, internal names and views |
| [`docs/flows.md`](docs/flows.md) | All three Power Automate flows, with expressions |
| [`docs/setup.md`](docs/setup.md) | How to rebuild the system, including lessons learned |
| [`screenshots/`](screenshots/) | Screenshots of the working system |

## Screenshots

![Records Dashboard](screenshots/12-dashboard.png)

### How it was built

| 1. Site created | 2. Importing the data |
| --- | --- |
| ![Site](/records-management-trackerscreenshots/01-site-created.png) | ![CSV import](records-management-trackerscreenshots/02-csv-import.png) |
| **3. Requests list** | **4. Status choices** |
| ![Requests](screenshots/03-requests-list.png) | ![Choices](screenshots/04-choice-colors.png) |
| **5. Completed in green, overdue in red** | **6. Request setup flow** |
| ![Overdue](screenshots/05-requests-overdue.png) | ![Flow](screenshots/07-flow-designer.png) |
| **7. Successful run** | **8. ID and due date filled in automatically** |
| ![Run](screenshots/08-flow-success.png) | ![Auto-filled](screenshots/09-new-request-auto.png) |
| **9. Subpoena sent to Legal for approval** | **10. Daily overdue report logged** |
| ![Approval](screenshots/10-approval-email.png) | ![Overdue log](screenshots/11-overdue-log.png) |
| **11. Dashboard: overdue requests** | **12. Dashboard: communications follow-ups** |
| ![Records Dashboard](screenshots/12-dashboard.png) | ![Dashboard: communications](screenshots/12-dashboard01.png) |
| **13. Dashboard: equipment due back** | **14. System change log** |
| ![Dashboard: equipment](screenshots/12-dashboard02.png) | ![Change log](screenshots/13-change-log.png) |

## Design decisions

- **Policy enforced by automation**, not memory: no release without consent, no subpoena production without legal review.
- **Decisions logged, not just emailed.** Every approval and daily check lands in the Communications Log, giving a permanent, searchable audit trail.
- **Flows never write the same field**, so they can run at the same time without overwriting each other.
- **Fictional data only**, so the whole system can be shown publicly.

## Next phase

- A completion-date flow to measure turnaround time per request type.
- A Records Repository document library using the indexing standard (student ID, document type, school, year, retention).

## About me

I combine law enforcement records experience, as a sheriff's deputy in detention center booking and on patrol, with cloud systems administration, data platform engineering and PKI training. I built this project to show how records offices can be both careful and technically sound.

Marissa M. · www.linkedin.com/in/marissa-m-3a826b224 
