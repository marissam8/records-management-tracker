# Power Automate flows

All three flows use standard connectors (SharePoint and Approvals) included with Microsoft 365 business plans. They reference SharePoint columns by **internal name** (see [schema.md](schema.md)), because a list created from a CSV gets generic names like `field_1`.

## Flow 1: New request setup

**Trigger:** When an item is created (Requests)

1. **Initialize variables:** `Prefix` (string, empty), `DaysToDue` (integer, 14), `NewStatus` (string, the request's Status).
2. **Switch** on request type, `triggerBody()?['field_1']?['Value']`:

| Case | Prefix | DaysToDue |
| --- | --- | --- |
| Transcript | TR | 7 |
| Subpoena | SB | 14 |
| Enrollment verification | VR | 5 |
| Records transfer | RT | 14 |
| Default (Diploma replacement) | DP | 14 |

3. **Condition:** release on file, `triggerBody()?['field_6']?['Value']`, equals `No` → set `NewStatus` to `Pending release`.
4. **Update item** (Requests):

| Field | Expression |
| --- | --- |
| Title (Request ID) | `concat(variables('Prefix'),'-',formatNumber(add(triggerBody()?['ID'],140),'0000'))` |
| DueDate | `formatDateTime(addDays(utcNow(),variables('DaysToDue')),'yyyy-MM-ddT12:00:00Z')` |
| Status | `variables('NewStatus')` |

Dates are written at noon UTC so time zone conversion can never shift them to the previous day.

## Flow 2: Subpoena legal approval

**Trigger:** When an item is created (Requests)

1. **Condition:** `triggerBody()?['field_1']?['Value']` equals `Subpoena`. Other request types end here.
2. **Update item:** LegalApproval = `Pending`. Status is left untouched so this flow never conflicts with Flow 1.
3. **Start and wait for an approval** (Approve/Reject, first to respond), assigned to Legal, with the student and a link to the request.
4. **Condition:** `outputs('Start_and_wait_for_an_approval')?['body/outcome']` equals `Approve`.
   - **Approved:** Update item, LegalApproval = `Approved`. Create item in the Communications Log: party *Office of Legal Counsel*, channel *Automated workflow*, subject *Subpoena approved: notify parent before producing records*, linked to the request ID, follow-up in 3 days, status *Open*.
   - **Rejected:** Update item, LegalApproval = `Rejected`. Create item in the Communications Log: subject *Subpoena rejected: do not produce records*, status *Closed*.

In the demo, the builder approves as a stand-in for the Office of Legal Counsel, using the Approvals center in Power Automate.

## Flow 3: Daily overdue report

**Trigger:** Recurrence, daily at 8:00 AM

1. **Get items** (Requests), with Filter Query entered as a single expression:

```
concat('field_7 ne ''Completed'' and field_8 lt ''', formatDateTime(utcNow(),'yyyy-MM-dd'), '''')
```

2. **Condition:** `length(body('Get_items')?['value'])` greater than `0`.
3. **Select:** maps each overdue item to its request ID, `item()?['Title']`.
4. **Create item** in the Communications Log: party *Records Management team*, channel *Automated workflow*, follow-up status *Open*, and subject:

```
concat('Daily overdue report: ', length(body('Get_items')?['value']), ' overdue (', join(body('Select'), ', '), ')')
```

Example result: *Daily overdue report: 3 overdue (DP-0142, TR-0143, TR-0145)*.
