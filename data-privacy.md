# About the data and FERPA

Every record in this project is **fictional**. I created it on purpose, because real student records should never appear in a public portfolio.

## What is fictional

| Data | How it was created |
| --- | --- |
| Student names | Invented (for example, "Rivera, Ana" and "Park, Dana"); they don't refer to real people |
| Request IDs and student IDs | Made-up numbers in a realistic format, such as TR-0143 |
| Requesters | Generic or invented organizations, such as "State University Admissions" and "Hale & Ortiz LLP" |
| Schools, dates and outcomes | Invented to show realistic workflows: transcripts, subpoenas, verifications and records transfers |
| Subpoena approvals | Test approvals I made myself, standing in for an Office of Legal Counsel |

Any resemblance to a real student, family or case is coincidental. The SharePoint site is private, and screenshots were taken from test data only.

## What FERPA is

The **Family Educational Rights and Privacy Act (FERPA)** is a federal law passed in 1974 that protects the privacy of student education records. It applies to schools and districts that receive funding from the U.S. Department of Education, which includes nearly every public school system. The law is codified at 20 U.S.C. § 1232g, with regulations at 34 CFR Part 99.

FERPA gives parents these rights over their children's education records:

- **Inspect and review** the student's education records.
- **Request corrections** to records they believe are inaccurate or misleading.
- **Consent before disclosure:** in general, a school must have written consent before releasing personally identifiable information from a student's records.

These rights transfer to the student, called an **eligible student**, at age 18 or when the student attends a school beyond high school.

FERPA allows disclosure without consent in specific situations, including to school officials with a legitimate educational interest, to another school where the student is enrolling, in a health or safety emergency, and to comply with a **judicial order or lawfully issued subpoena**, generally after a reasonable effort to notify the parent or eligible student in advance. Schools must also keep a record of most disclosures they make.

## How the system reflects FERPA

| FERPA principle | How the system handles it |
| --- | --- |
| Consent before release | Any request without a signed release is automatically set to *Pending release* |
| Subpoenas and parent notice | Every subpoena is routed to Legal for approval, and approval logs a reminder to notify the parent before records are produced |
| Recording disclosures | The Communications Log keeps a dated record of decisions and contacts tied to each request |
| Limiting access | The site is private, and in production subpoena records would be limited to records staff and Legal |

*This is a plain-language summary for a portfolio, not legal advice. Districts apply FERPA alongside state law and board policy, so local procedures may add requirements. The U.S. Department of Education's Student Privacy Policy Office publishes official guidance at [studentprivacy.ed.gov](https://studentprivacy.ed.gov).*
