# Data model: lists, columns and views

When SharePoint creates a list from a CSV, it assigns generic internal names (`field_1`, `field_2`, ...) and turns one column into `Title`. Flows and formatting reference internal names, so this page lists both. To check any column's internal name, open List settings, click the column, and read the end of the web address after `Field=`.

## Requests

| Column | Internal name | Type | Settings |
| --- | --- | --- | --- |
| Request ID | Title | Single line of text | Filled by the request setup flow |
| Request type | field_1 | Choice (drop-down) | Transcript, Subpoena, Enrollment verification, Records transfer, Diploma replacement |
| Date received | field_2 | Date | Default: today |
| Requester | field_3 | Single line of text | Required |
| Requester category | field_4 | Choice (drop-down) | College/university, Law firm, Employer/agency, Parent/guardian, Former student, Health department, School/central office |
| Student (fictional) | field_5 | Single line of text | |
| Release on file | field_6 | Choice (drop-down) | Yes, No, Not required |
| Status | field_7 | Choice (drop-down) | Received, In progress, Pending release, Completed |
| Due date | field_8 | Date | Filled by the request setup flow |
| Date completed | field_9 | Date | Reserved for turnaround reporting |
| Legal approval | field_10 | Choice (drop-down) | Not needed, Pending, Approved, Rejected |
| Notes | field_11 | Multiple lines of text | |

All choice columns must be **drop-down (single selection)**. The CSV import created them as checkboxes (multiple selection), which stores values as lists and breaks comparisons in flows and formatting.

### Views and formatting

| View | Filter | Sort |
| --- | --- | --- |
| Open requests | Status ≠ Completed | Due date ascending |
| Overdue | Status ≠ Completed AND Due date < `[Today]` | Due date ascending |
| Pending release | Status = Pending release | Date received |
| Subpoenas | Request type = Subpoena | Due date |
| Status board | Board view grouped by Status | |

Conditional formatting: completed rows show green, and the Request ID turns red when the due date has passed and the request isn't completed.

## Communications Log

Title holds the **party**. Other columns: CommDate (date), Category (choice: Central office, School, Parent/guardian, Student, External agency, Law firm), Channel (choice: Email, Phone, In person, Mail, Secure email, Help desk ticket, Automated workflow), Subject, LinkedRequest, FollowUpDate (date), FollowUpStatus (choice: Open, Closed).

View: **Open follow-ups** (FollowUpStatus = Open). The approval and daily report flows write entries here with channel *Automated workflow*.

## Resources

Asset (Title), Tag, AssignedTo, Location, Deployed (date), DueBack (date), Status (choice: In service, On loan, In repair, Retired). View: **Due back soon** (DueBack < `[Today]+7`).

## System Change Log

ChangeDate, System, Change, Reason, Tested, ApprovedBy. Sorted by date, newest first. Version history is enabled on all lists.
