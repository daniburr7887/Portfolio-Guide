# Case study: Moving a company's BI from Tableau to code

**Role:** Business Intelligence Analyst · **Company:** healthcare staffing marketplace · **Timeline:** July 2026 – present

**At a glance**
- Part of a small team that moved ~70 Tableau dashboards and reports to code: ~40 Shiny apps (Python) and an on-demand Slack report bot
- Reconciled rebuilt dashboards against the original Tableau numbers before the old versions were retired
- Fixed production problems: dashboards that froze, crashed or waited over an hour to load
- Contributed to a shared pitfalls guide (now 55+ entries) so each migration didn't repeat the last one's mistakes

---

## The situation

The company's reporting lived in Tableau: dozens of dashboards used daily by operations, sales and finance teams, many with years of accumulated logic in calculated fields and filters. The goal was to rebuild them as code (Shiny for Python apps deployed from Git, plus reports people request straight from Slack) so they could be versioned, reviewed, tested and changed quickly.

The hard part wasn't building new dashboards. It was making sure each rebuilt one said **exactly** what the old one did, and then keeping it fast and reliable once real users hit it.

## How I made sure the numbers matched

Every migration ended with a side-by-side reconciliation against the Tableau original. Most differences turned out to have causes that weren't in the visible formulas:

- **Hidden filters.** A KPI's formula had no "completed jobs only" condition, but the Tableau data source did, on a filter shelf you only see by opening it. Rebuilding from the formula alone gave the wrong number.
- **Timestamps off by exactly 6 hours.** It looked like a time-zone bug in my query. The real cause was upstream: certain columns lost their time-zone information when replicated to the data warehouse, and only *some* columns on the same table were affected. I confirmed it by comparing the column definitions on both databases, worked around it in SQL, and documented which columns were affected so other dashboards could be checked.
- **"Extra" rows that weren't errors.** Some Tableau workbooks used scheduled extracts, so the live rebuild correctly showed rows added after the last extract refresh.
- **Empty reports from case-sensitive filters.** A city filter typed in lowercase matched nothing because the source data is uppercase. Case-insensitive text filters became a documented standard for every report.
- **Checking my own check.** In one reconciliation, a 2× gap turned out to be my verification query using a full calendar week while the dashboard's date filter clipped that week to three days. The dashboard was right. Since then, the first step is always to match the check's filters exactly to what the dashboard shows.

## Production problems I solved

| Problem | Cause | Fix | Result |
|---|---|---|---|
| Dashboards waited **over an hour** to load, then timed out | Each user session held a database transaction open; a pipeline write queued behind it, and every other reader queued behind the write | Turned on autocommit in the shared database connection code and every app's own connection | Applied across the entire dashboard fleet in one change; the lock pile-ups stopped |
| Pages froze ("Page unresponsive") | Facility dropdowns sent **~40,000 options** to the browser at once. A teammate traced the first case; I found and fixed the same pattern in other dashboards | Server-side search: the browser only receives matches as the user types | Pages load normally; the dropdown stays searchable |
| A dashboard crashed when users picked **3–4 year** date ranges | The app pulled every row into memory before filtering | For long ranges, push every filter into SQL and cap the result size | A 3-year, single-state query that used to crash returned 61k rows in about 30 seconds |
| A marketing funnel timed out beyond **30 days** | One large query doing four kinds of work in sequence | Split it into four parts that run in parallel, with a longer time budget | 90-day ranges complete in about a minute, with results identical to before |

## Building a system, not one-off reports

- **Reports on demand in Slack.** People pick a report and its filters in a Slack form and get back a CSV or Excel file. I moved related reports into grouped menus, which the team later generalized into a single registry where adding a report group is a data change, not new code.
- **Shared standards.** A checklist for new dashboards, case-insensitive filters, and the pitfalls guide, so each migration got faster and more consistent.
- **Dependency audits.** When an upstream data schema was scheduled for retirement, I audited every dashboard and report that depended on it (21 dashboards reading 25 of its tables), mapped direct replacements, and flagged the tables that had none.
- **Cleanup.** Archived the 39 legacy copies of dashboards on the old hosting platform once their replacements were verified.

## How I work with an AI coding assistant

I built most of this with Claude Code as a development partner. What made that work:

- **Verify, don't trust.** Every change was checked against real data or the live dashboard before it counted as done. Plausible-sounding fixes that weren't checked against the actual Tableau definition cost deploy cycles, so pulling the real calculated-field formula became the first step for any parity bug.
- **Write down what's learned.** Recurring problems went into the shared pitfalls guide and the new-dashboard checklist, not just my notes.
- **Keep humans in the loop.** Code went through review where the team required it. Replies to the people who requested reports were mine to send.

## What I'd do differently

- Reconcile against Tableau **before** a dashboard reaches users, not after the first question comes in.
- Load-test with realistic data from the start. The dropdown freeze slipped past a local test because that test used a fake facility list with no entries.

**Skills:** SQL (Amazon Redshift, PostgreSQL) · Shiny for Python · Tableau · Slack automation · Git/GitHub · data reconciliation · performance tuning · AI-assisted development
