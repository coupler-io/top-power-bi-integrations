---
title: Connect Xero to Power BI with Coupler.io
slug: connect-xero-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md
source: "Xero, Xero Reports"
destination: Power BI
meta_title: Connect Xero to Power BI with Coupler.io
meta_description: Load Xero invoices, bank transactions, payroll, projects, Profit And Loss and Balance Sheet into Power BI on a schedule. Live finance reports, no CSVs.
---

![Xero](https://static.coupler.io/importers/xero.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Xero to Power BI with Coupler.io

Xero's reporting tab is built for reading one statement at a time. As soon as you want to compare organisations, put payroll next to project hours, or chart receivables week by week, you end up exporting to Excel and rebuilding the same workbook every month. Power BI doesn't ship a Xero connector you can rely on, and the Xero API is awkward to call from Power Query: tokens expire after half an hour, each organisation needs its own tenant header, and payroll lives in separate regional APIs. Coupler.io handles all of that and gives Power BI one link per flow that serves fresh Xero data each time it is refreshed.

## How to export Xero data to Power BI

Coupler.io has two Xero connectors that share the same login. **Xero** exports records: accounting entities such as invoices and contacts, UK/NZ and Australian payroll, and Xero Projects. **Xero Reports** exports Xero's calculated reports, from aged payables to the Profit And Loss. The steps below cover the Xero connector first, then what changes in Xero Reports.

### Step 1. Basic settings

- **Xero tenant** — the Xero organisation to read from. If your login covers several organisations and one is missing, reconnect the account under Connections.
- **Data entity** — the kind of record to export. Options are grouped into Accounting, Payroll (for United Kingdom or New Zealand), Payroll (for Australia), and Projects; the complete list is further down.
- **Report type** — shown for Bank transactions, Contacts, Credit notes, Invoices, Manual journals, Overpayments and Prepayments. **Detailed** (the default) returns an extended field set; **Simple** returns the standard fields.
- **Projects** — only for Tasks by projects and Time by projects. Pick one or more Xero projects to pull from.

### Step 2. Date filters

Which date fields appear depends on the entity. They use Coupler.io's relative date picker, so you can choose *days ago*, *this month*, *last month*, *this year* or *last year* and the window moves forward by itself. For a fixed rolling year, `{{365daysago}}` works too.

- **Changed after** — export only records updated after this date. Available for most accounting entities (Invoices, Contacts, Bank transactions, Payments, Journals and others) and for the Australian payroll entities. It keeps scheduled runs fast on organisations with years of history.
- **Start date** and **End date** — the period for Timesheets (UK or NZ) and Time by projects.
- **Created after** and **Created before** — creation-date window for Purchase orders and Quotes.
- **Expiring after** and **Expiring before** — expiry-date window for Quotes, useful for spotting quotes about to lapse.

### Step 3. Advanced settings

- **Statuses** — for Invoices, pick which invoice statuses to include: Draft, Submitted, Authorised, Paid, Voided, Deleted. For Projects, the same field title filters by In progress or Closed.
- **Status** — a single status filter for Pay runs (UK or NZ), Timesheets (UK or NZ), Linked transactions, Purchase orders, Quotes, and Time by projects (Active or Invoiced).
- **Journal number more than** and **Journal number less than** — for Journals, export a range of journal numbers. Handy for pulling a long ledger history in chunks.
- **Split by** — turn nested lines into separate rows. Invoices and Credit notes default to `LineItems`, Journals to `JournalLines`, and Tracking categories to `Options`, so each line arrives as its own row ready for item-level or account-level visuals.
- **Advanced settings** — extra property-and-value filters such as Where and Order by. Some entities add their own keys: Invoices accept Invoice Ids, Contact Ids and Invoice Numbers; Quotes accept Quote number; Time by projects accepts User Id, Task Id and Is chargeable.
- **Include archived** — for Invoices, Contacts and Tracking categories, include archived records as well as active ones.

**Xero Reports** walks through different steps:

- **Basic settings** — **Xero tenant**, **Report type** (the report to export), and **Report structure** for Balance Sheet, Budget Summary, Profit And Loss and Executive Summary. Xero-style format keeps dates as column headers; Date-by-row format puts dates in one column, which suits Power BI's date table and line charts far better.
- **Contact** — **Contact ID**, required for Aged Payables By Contact and Aged Receivables By Contact, since those reports cover one contact each.
- **Bank account** — **Bank Account ID**, required for Bank Statement.
- **Report period** — **Date** is the as-of date for aged reports, Balance Sheet, Budget Summary, Executive Summary and Trial Balance. **Start date** and **End date** set the range for aged reports, Bank Statement, Bank Summary and Profit And Loss. These use the same relative picker.
- **Parameters** — **Number of periods to compare** and **Period** (Month, Quarter or Year) for Balance Sheet, Budget Summary and Profit And Loss; **Tracking category ID**, **Tracking category ID-2** and **Tracking option ID** to break down the Profit And Loss by tracking category; **Tracking option ID** and **Tracking option ID-2** to filter the Balance Sheet; **Include cash transactions only** for Balance Sheet, Profit And Loss and Trial Balance; and **Standard layout**.

### Step 4. Blend and transform data

- **Column management** — hide Xero's internal IDs you won't chart, rename fields to your own reporting terms, put columns in a sensible order, and fix data types so amounts sum correctly.
- **Join** — Coupler.io's Join adds contact names and groups to invoice lines, or account codes to journal lines, before anything reaches Power BI — or load the tables separately and relate them in Power BI's model view instead.
- **Append** — run the same entity against several Xero tenants and stack the results, so a group of companies gets one Invoices table with an organisation column.
- **Aggregate** — summarize invoice lines to revenue per contact per month, or timesheet lines to hours per employee per pay period.
- **Custom SQL** — query the flow's data directly for logic such as payment delay buckets or gross margin per item.

### Step 5. Power BI destination

You don't sign in to Power BI from Coupler.io. The destination step contains only **Basic settings**: click **Generate link** and copy the **Integration URL** it produces.

Open Power BI Desktop, go to **Get data** → **Web**, paste the link, and click **OK**. Xero data appears in Power Query as a table. Rename the query to match the entity, for example "Xero Invoices" or "Xero P&L by month", since you will likely load several. In model view, connect Invoices and Payments to Contacts on the contact ID, connect Journals to Accounts on the account ID, and link each date column (invoice date, due date, journal date) to a date table. Mark that date table as such so time intelligence measures behave.

If you began from a Coupler.io dashboard template, click **Download template**, open the file in Power BI Desktop, paste the Integration URL shown below the button, and click **Load**.

The Integration URL contains an access token, so share it as carefully as a password. Regenerate it in the flow if it ever leaks, then update the Power BI source with the new link.

No schema or field mapping is needed. Coupler.io reads the Xero data and sets the columns and their types itself.

### Step 6. Schedule

Pick a refresh interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Accounting entities usually need a daily run; a cash or receivables page used during the day may justify hourly; payroll entities only change around pay runs, so weekly is often enough.

Every run swaps the data behind the Integration URL for a complete new copy. Power BI reads it on its own refresh: hit **Refresh** in Power BI Desktop, or publish to the Power BI Service and schedule the semantic model to refresh shortly after Coupler.io finishes. For trend reports, set a wide window (for example **Start date** at `{{365daysago}}` or several years back, or more periods to compare on the Profit And Loss). This destination always holds the latest copy rather than a growing archive, so it can't recover figures Xero itself no longer returns.

### Bonus: or just ask the AI Agent to build it for you

You can also skip the setup screens. Open the Coupler.io AI Agent from the right sidebar in the Coupler.io app and describe the outcome:

> *"From Xero, export Authorised and Paid invoices split by line items for the last two years, add a monthly Profit And Loss in date-by-row format comparing 11 periods, and prepare both for Power BI."*

The Agent selects the entity and report, sets the statuses, **Split by** and date window, and configures the report structure. It asks only when it genuinely needs a choice from you.

What you still do yourself:

1. **Connect Xero** — signing in to Xero and granting access is your call.
2. **Pick the Xero tenant** — choose which organisation, or organisations, feed the flow.
3. **Paste the Integration URL** — into **Get data** → **Web** in Power BI Desktop.
4. **Set up the refresh in Power BI** — manual Refresh, or a scheduled refresh in the Power BI Service.

## What data can you export from Xero to Power BI with Coupler.io?

The Xero connector covers 57 data entities in four groups, and Xero Reports adds 9 reports. A flow carries one entity or one report. Coupler.io's Join and Append combine them before loading, for instance invoices with contacts, or the same entity from several organisations — or bring each in as its own table and relate them in Power BI.

Accounting entities:

- **Accounts** — the chart of accounts with code, type, tax type and status.
- **Bank transactions** — spend and receive money transactions on bank accounts, with line items and reconciliation status.
- **Bank transfers** — transfers between your own bank accounts, so they can be excluded from income and spend.
- **Branding themes** — the invoice and quote templates defined in Xero.
- **Contact groups** — groups used to segment customers and suppliers.
- **Contacts** — customers and suppliers with addresses, payment terms and outstanding balances.
- **Credit notes** — credits raised for customers or received from suppliers, with allocations.
- **Currencies** — currencies enabled in the organisation.
- **Employees** — employees as recorded in the accounting side of Xero.
- **Expense claims** — staff expense claims and their status.
- **Invoices** — sales invoices and supplier bills with dates, due dates, status, totals and amount due; split by line items for item-level analysis.
- **Items** — products and services with codes, sale and purchase prices, and stock quantity.
- **Journals** — every ledger posting generated by Xero, line by line. The most complete basis for a custom general ledger.
- **Linked transactions** — billable expenses linked from bills to customer invoices.
- **Manual journals** — journals entered by hand, for reviewing adjustments.
- **Organisation** — organisation name, base currency, country and financial year end.
- **Overpayments** — payments that exceeded the amount due and their allocations.
- **Payments** — payments applied to invoices and credit notes, with dates and amounts.
- **Prepayments** — payments received or made before an invoice existed.
- **Purchase orders** — orders to suppliers, with status and delivery dates.
- **Receipts** — receipts attached to expense claims.
- **Repeating invoices** — invoice templates scheduled to repeat, which make recurring revenue visible ahead of time.
- **Tax rates** — tax rates and their components.
- **Tracking categories** — tracking categories and, split by options, every option within them.
- **Users** — users with access to the organisation.
- **Quotes** — quotes sent to customers, with status and expiry date.

Payroll entities (United Kingdom or New Zealand):

- **Benefits** — benefit types set up in UK or NZ payroll.
- **Deductions** — deduction types used in pay runs.
- **Earning rates** — earnings rate definitions such as regular hours or overtime.
- **Earnings orders (Statutory deductions)** — statutory deductions such as court orders applied to employees.
- **Employees (UK or NZ)** — payroll employees with start dates and payroll details.
- **Leave types** — leave categories available to UK or NZ employees.
- **Pay run calendars** — the pay frequencies and period dates.
- **Pay runs** — each pay run with period, status and totals.
- **Reimbursements** — reimbursement types paid through payroll.
- **Accounts (UK or NZ)** — the accounts payroll posts to.
- **Timesheets** — employee timesheets with hours and status.
- **Tracking categories (UK or NZ)** — tracking categories used by payroll.

Payroll entities (Australia):

- **Employees (AU)** — Australian payroll employees and their details.
- **Leave applications** — leave requests with dates and leave type.
- **Leave applications with leave periods** — the same requests broken into each leave period.
- **Earnings rates** — earnings rate definitions in Australian payroll.
- **Deduction types** — deduction definitions.
- **Leave types** — leave categories for Australian employees.
- **Reimbursement types** — reimbursement definitions.
- **Pay runs** — Australian pay runs with period dates and totals.
- **Payroll calendars** — pay frequencies and period start dates.
- **Settings** — payroll settings for the organisation.
- **Accounts (AU)** — accounts used by Australian payroll.
- **Tracking categories (AU)** — tracking categories used by Australian payroll.
- **Super funds** — superannuation funds employees contribute to.
- **Timesheets** — Australian timesheets at the header level.
- **Timesheets with timesheet lines** — timesheets with every line, for hours by earnings rate and day.

Projects entities:

- **Projects** — Xero projects with status, estimate and contact.
- **Tasks by projects** — tasks within the selected projects, with rates and estimates.
- **Time by projects** — time entries logged against project tasks, with user, duration and charge status.
- **Users** — project users who can log time.

> **Note:** The reports below are available in the **Xero Reports** source in Coupler.io, separate from the Xero source above. Pick Xero Reports when you create the flow, then choose the report in **Report type**.

- **Aged Payables By Contact** — what you owe a single supplier, aged by due date. Needs a **Contact ID**.
- **Aged Receivables By Contact** — what a single customer owes you, aged by due date. Needs a **Contact ID**.
- **Balance Sheet** — assets, liabilities and equity as of the chosen month, optionally compared over several periods.
- **Bank Statement** — statement lines for one bank account between two dates. Needs a **Bank Account ID**.
- **Bank Summary** — opening balance, cash in, cash out and closing balance for each bank account.
- **Budget Summary** — budgeted figures by account and period.
- **Executive Summary** — monthly totals and common ratios such as gross margin and debtor days.
- **Profit And Loss** — income and expenses for the period, with period comparison or a tracking category breakdown.
- **Trial Balance** — the balance of every account on the selected date.

## Why export Xero to Power BI — real life cases

Here is what finance teams and accountants build in Power BI once Xero data arrives there on a schedule.

- **Receivables page with saved views for the credit team**

  Load Invoices with **Statuses** set to Authorised and a Contacts table, then calculate days overdue from the due date with a DAX measure. Slicers for contact group and overdue bucket sit next to a table of open invoices, and bookmarks save views such as "over 60 days" or "top 20 debtors" so the credit team switches between them with one click.

- **Profit And Loss by tracking category with switchable measures**

  Run Profit And Loss in Date-by-row format, once overall and once with **Tracking category ID** set to your region or department category. A combo chart shows revenue as columns and net margin as a line, and a field parameter lets the reader switch the axis between month, tracking option and account without separate pages.

- **Cash movement trend across bank accounts**

  Bank Summary gives cash in and out per account; Bank transactions adds the detail. A line chart of closing balance over time with the analytics pane's forecast shows where cash is heading, and a drill-through page per bank account lists the largest transactions behind any dip.

- **Payroll cost per pay run, shared in Teams**

  Pay runs, Timesheets with timesheet lines and Employees (AU), or the UK or NZ equivalents, show labor cost and hours per pay period. Row-level security keeps salary detail limited to HR and leadership, and the report is pinned as a tab in the HR channel in Microsoft Teams so people use it where they already work.

- **Project margin from logged time**

  Projects, Tasks by projects and Time by projects relate on project and task IDs. A scatter chart plots each project's hours against its estimate, and the key influencers visual points to which tasks or users tend to push projects over budget. Filtering Time by projects to Active or Invoiced with **Status** separates work still to bill from work already billed.

- **Group overview across Xero organisations**

  Coupler.io's Append combines the same Invoices or Profit And Loss flow from every **Xero tenant** into one table, and the Organisation entity supplies each company's country and base currency. A filled map shows revenue by country, and the finished report is distributed as a Power BI app to directors who don't have Xero logins.

- **Budget versus actual with a waterfall**

  Budget Summary and Profit And Loss, both by month, relate on account in the model. A waterfall chart walks from budgeted to actual profit account by account, so the main variances jump out, and conditional formatting in the accompanying table highlights lines more than 10% off plan.

- **Quote pipeline with alerts on expiring quotes**

  Export Quotes with **Created after** set to *this year* and **Status** left empty, so Sent, Accepted, Declined and Invoiced quotes all arrive. A funnel chart shows how many move from sent to invoiced. A card counting sent quotes whose expiry date falls within the next 14 days is pinned to a dashboard with a data alert, so sales follows up before the offer lapses.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
- [Connect Google Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md)
- [Connect Google Search Console to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-search-console-to-power-bi.md)
- [Connect Instagram Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-ads-to-power-bi.md)
- [Connect Instagram Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-insights-to-power-bi.md)
- [Connect LinkedIn Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-linkedin-ads-to-power-bi.md)
- [Connect LinkedIn Public Data to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-linkedin-public-data-to-power-bi.md)
- [Connect Microsoft Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-microsoft-ads-to-power-bi.md)
- [Connect QuickBooks to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-quickbooks-to-power-bi.md)
- [Connect Shopify to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md)
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
