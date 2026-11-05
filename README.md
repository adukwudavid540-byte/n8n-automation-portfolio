# n8n Automation Portfolio

AI automation projects built with [n8n](https://n8n.io) by **David Adukwu**, a data analyst with a background in public health and microbiology.

All projects were built and tested with **made-up practice data only**. The workflow files in this repository contain no personal data, credentials or live links.

- **Article:** [How I Automated Malaria M&E Reporting with n8n and AI](https://medium.com/@adukwudavid540/how-i-automated-malaria-m-e-reporting-with-n8n-and-ai-74a5d93fb16f)

---

## Project 1: Health M&E Reporting Automation

Three connected workflows that automate daily data collection, data quality checks and weekly reporting for a malaria programme.

| Workflow | File | What it does |
| --- | --- | --- |
| Health Daily Report | [`health-daily-report.json`](health-daily-report.json) | A form collects daily reports. Each report must pass four data quality rules and a duplicate check (same health worker and date) before it is saved to Google Sheets. Failed or duplicate reports trigger an email alert to the M&E officer. If a saved report shows clients seen but 0 malaria tests, a warning email is sent (the report is kept, since it may mean test kits ran out). |
| Weekly M&E Report | [`weekly-me-report.json`](weekly-me-report.json) | Every Monday at 8 AM, keeps reports from the previous Monday to Sunday (a fixed reporting week), totals them by facility, calculates malaria positivity rates, and emails an AI-written summary (OpenAI GPT-4o-mini). Shows "no tests this week" instead of a misleading 0% when a facility did no tests. Sends an urgent alert if any facility reaches 30% positivity or more. |
| Missing Report Reminder | [`missing-report-reminder.json`](missing-report-reminder.json) | Every evening at 6 PM, compares the register of health workers with the day's reports and emails a personal reminder to anyone who has not reported. |

**Data quality rules** (all must pass):

1. Clients seen is 0 or more
2. Malaria tests is not more than clients seen
3. Malaria positive is not more than malaria tests
4. Malaria positive is 0 or more

**Warning (report is saved, M&E officer is alerted):** clients seen is more than 0 but malaria tests is 0.

**Improvements from reader feedback:** a fixed Monday-to-Sunday reporting week, "no tests this week" instead of 0.0%, and the zero-tests warning. Thanks to Hayrullah and Divine for their comments on the Medium article.

**n8n nodes used:** Form Trigger, Schedule Trigger, If, Filter, Summarize, Aggregate, Compare Datasets, Google Sheets, Gmail, OpenAI

---

## Project 2: Gym Sign-up with AI Welcome

| Workflow | File | What it does |
| --- | --- | --- |
| AI Welcome Automation | [`gym-signup-ai-welcome.json`](gym-signup-ai-welcome.json) | A sign-up form checks Google Sheets for the person's email. New members get an AI-written welcome email and are saved with a tidied name and date. Returning members get an "already on our list" email instead, so no duplicates are created. |

---

## Shared: Error Alert

| Workflow | File | What it does |
| --- | --- | --- |
| Error Alert | [`error-alert.json`](error-alert.json) | Linked to every workflow above. If any step fails, it emails the workflow name, failed step, error message and a link to the run. |

---

## How to use these workflows

1. In n8n, create a new workflow, then choose **Import from File** (or paste the JSON onto the canvas).
2. Connect your own **Google Sheets**, **Gmail** and **OpenAI** credentials on each node.
3. Replace the placeholders:
   - `YOUR_GOOGLE_SHEET_ID`: pick your own sheet in each Google Sheets node
   - `you@example.com`: the email address that should receive alerts and reports
   - `YOUR_ERROR_WORKFLOW_ID`: in **Settings > Error Workflow**, choose your imported Error Alert workflow
4. Create Google Sheets with matching column names:
   - Reports sheet: `Date`, `Health Worker`, `Facility`, `Clients Seen`, `Malaria Tests`, `Malaria Positive`
   - Register tab (`Health Workers`): `Health Worker`, `Facility`, `Email`
   - Gym sheet: `Name`, `Email`, `Date`, `Welcome Message`
5. Test with practice data before publishing.

---

## Skills demonstrated

Workflow design, data quality automation, duplicate detection, scheduled workflows, disaggregation by facility, calculated indicators, list comparison and looping, prompt engineering, error handling, testing and debugging, and responsible use of health data.

## Contact

Connect with me on LinkedIn: **David Adukwu**
