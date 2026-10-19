# Data model: lists, columns and views

Column internal names have no spaces. SharePoint fixes the internal name when a column is created, and the flows and formatting reference these names directly.

## Requests

| Internal name | Display name | Type | Settings |
| --- | --- | --- | --- |
| Title | Request ID | Single line of text | Filled by Flow 1 |
| RequestType | Request type | Choice | Transcript, Subpoena, Enrollment verification, Records transfer, Diploma replacement |
| Received | Date received | Date | Default: Today |
| Requester | Requester | Single line of text | Required |
| RequesterCategory | Requester category | Choice | College/university, Law firm, Employer/agency, Parent/guardian, Former student, Health department, School/central office |
| Student | Student (fictional) | Single line of text | Required |
| ReleaseOnFile | Release on file | Choice | Yes, No, Not required |
| Status | Status | Choice | Received, In progress, Pending release, Completed |
| DueDate | Due date | Date | Filled by Flow 1 |
| CompletedDate | Date completed | Date | Filled by Flow 2 |
| LegalApproval | Legal approval | Choice | Not needed, Pending, Approved, Rejected |
| Notes | Notes | Multiple lines of text | |

### Views

| View | Filter | Sort |
| --- | --- | --- |
| Open requests | Status ≠ Completed | Due date ascending |
| Overdue | Status ≠ Completed AND Due date < `[Today]` | Due date ascending |
| Pending release | Status = Pending release | Date received |
| Subpoenas | Request type = Subpoena | Due date |
| Status board | Board view grouped by Status | |

### Overdue row formatting

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/row-formatting.schema.json",
  "additionalRowClass": "=if([$DueDate] < @now && [$Status] != 'Completed', 'sp-field-severity--severeWarning', '')"
}
```

## Communications Log

| Internal name | Type | Choices |
| --- | --- | --- |
| CommDate | Date | |
| Party | Text | |
| Category | Choice | Central office, School, Parent/guardian, Student, External agency, Law firm |
| Channel | Choice | Email, Phone, In person, Mail, Secure email, Help desk ticket |
| Subject | Text | |
| LinkedRequest | Text | Request ID |
| FollowUpDate | Date | |
| FollowUpStatus | Choice | Open, Closed |

View: **Open follow-ups** (FollowUpStatus = Open).

## Resources

| Internal name | Type | Choices |
| --- | --- | --- |
| Asset | Text | |
| Tag | Text | |
| AssignedTo | Text | |
| Location | Text | |
| Deployed | Date | |
| DueBack | Date | |
| Status | Choice | In service, On loan, In repair, Retired |

View: **Due back soon** (DueBack < `[Today]+7`).

## System Change Log

| Internal name | Type |
| --- | --- |
| ChangeDate | Date |
| System | Text |
| Change | Text |
| Reason | Text |
| Tested | Choice: Yes, No |
| ApprovedBy | Text |

Version history is enabled on all lists.
