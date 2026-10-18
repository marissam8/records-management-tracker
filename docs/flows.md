# Power Automate flows

All four flows use standard connectors (SharePoint, Office 365 Outlook, Approvals) included with Microsoft 365 business plans.

In each **Update item** action, the list's required fields (Request type, Requester, Student) are passed back from the trigger unchanged. Other fields are left blank unless listed, and blank fields are not modified. Flows 1 and 4 never write the same field, so they can run at the same time safely.

## Flow 1: New request setup

**Trigger:** When an item is created (Requests)
**Action:** Update item

| Field | Expression |
| --- | --- |
| Request ID | see below |
| Due date | see below |
| Status | see below |

Request ID (type prefix plus a running number):

```
concat(if(equals(triggerOutputs()?['body/RequestType/Value'],'Transcript'),'TR',if(equals(triggerOutputs()?['body/RequestType/Value'],'Subpoena'),'SB',if(equals(triggerOutputs()?['body/RequestType/Value'],'Enrollment verification'),'VR',if(equals(triggerOutputs()?['body/RequestType/Value'],'Records transfer'),'RT','DP')))),'-',formatNumber(add(triggerOutputs()?['body/ID'],140),'0000'))
```

Due date (7 calendar days for transcripts, 5 for verifications, 14 otherwise):

```
formatDateTime(addDays(coalesce(triggerOutputs()?['body/Received'],utcNow()),if(equals(triggerOutputs()?['body/RequestType/Value'],'Transcript'),7,if(equals(triggerOutputs()?['body/RequestType/Value'],'Enrollment verification'),5,14))),'yyyy-MM-dd')
```

Status (hold for a signed release):

```
if(equals(triggerOutputs()?['body/ReleaseOnFile/Value'],'No'),'Pending release',triggerOutputs()?['body/Status/Value'])
```

## Flow 2: Completion date stamp

**Trigger:** When an item is created or modified (Requests)
**Condition:** Status = `Completed` AND `empty(triggerOutputs()?['body/CompletedDate'])` = `true`
**If true:** Update item, Date completed = `formatDateTime(utcNow(),'yyyy-MM-dd')`

The empty-date check prevents the flow from re-triggering itself in a loop.

## Flow 3: Daily overdue report

**Trigger:** Recurrence, weekdays at 8:00 AM
**Get items** (Requests), Filter Query:

```
Status ne 'Completed' and DueDate lt '@{formatDateTime(utcNow(),'yyyy-MM-dd')}'
```

**Condition:** `length(body('Get_items')?['value'])` > 0
**If true:** Create HTML table (custom columns), then Send an email (V2) with the table as the body.

| Header | Value |
| --- | --- |
| Request ID | `item()?['RequestID']` |
| Type | `item()?['RequestType/Value']` |
| Requester | `item()?['Requester']` |
| Due | `item()?['DueDate']` |

## Flow 4: Subpoena legal approval

**Trigger:** When an item is created (Requests)
**Condition:** Request type = `Subpoena`

1. Update item: Legal approval = `Pending`
2. Start and wait for an approval (Approve/Reject, first to respond), assigned to Legal
3. If approved: Legal approval = `Approved`, and email a reminder to notify the parent or eligible student before producing records
4. If rejected: Legal approval = `Rejected`

In the demo, the builder approves as a stand-in for the Office of Legal Counsel.
