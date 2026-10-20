---
title: Connect QuickBooks to Power BI with Coupler.io
slug: connect-quickbooks-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-quickbooks-to-power-bi.md
source: "QuickBooks, QuickBooks Reports"
destination: Power BI
meta_title: Connect QuickBooks to Power BI with Coupler.io
meta_description: Send QuickBooks invoices, bills, payments, customers, P&L, Balance Sheet and aging reports to Power BI on a schedule. Month-end packs without exports.
---

![QuickBooks](https://static.coupler.io/importers/quickbooks.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect QuickBooks to Power BI with Coupler.io

QuickBooks Online is good at keeping the books and much less good at letting you slice them. Its built-in reports cap out at a few customization options, Power BI has no dependable built-in route to QuickBooks Online, and the usual workaround is an exported spreadsheet that goes stale the moment someone posts a journal entry. Calling the QuickBooks API from Power Query means handling OAuth tokens, pagination, and nested line items yourself. Coupler.io takes over that plumbing: it pulls QuickBooks records and QuickBooks reports on a schedule and hands Power BI a single web link that always returns the latest copy.

## How to export QuickBooks data to Power BI

There are two connectors behind one QuickBooks login. **QuickBooks** exports raw records such as invoices, bills, customers, and journal entries. **QuickBooks Reports** exports finished statements and aging reports exactly as QuickBooks calculates them. The steps below follow the QuickBooks connector; where QuickBooks Reports asks for something different, it is called out.

### Step 1. Basic settings

- **Data entity** — the type of record this flow exports. One flow carries one entity, so an invoice table and a customer table are two flows. Every option is listed further down this page.

In the **QuickBooks Reports** connector, this step asks for different fields:

- **Report name** — the QuickBooks report to export, from Account List Detail through Vendor Balance Detail.
- **Display columns by** — only for reports that can spread values across columns (for example Balance Sheet, Cash Flow, Profit and Loss Summary, Sales by Customer, Trial Balance and the Transaction List reports). Choose Total, Month, Quarter, Year, or a dimension such as Customers or Classes. Transaction List reports offer their own set, including Account, Transaction Type and Payment Method.

### Step 2. Advanced settings

- **Where** — a filter written in QuickBooks query syntax. For example, `AccountType='Accounts Receivable'` on the Account entity keeps only receivable accounts. Handy for keeping inactive records or test customers out of your model.
- **Order** — sort the export by a field, ascending by default or with `DESC` for the reverse.
- **Split by** — break nested lines into their own rows. Type `Line` on Invoice, Bill or SalesReceipt and each product or expense line becomes a separate row, which is the grain Power BI needs for sales-by-item or spend-by-account visuals.

**QuickBooks Reports** has three extra steps before its own Advanced settings:

- **Report period** (step) — **Report period** offers QuickBooks' own presets: Today, This Month, Last Month, This Fiscal Quarter, This Fiscal Year, Last Fiscal Year, the "-to-date" variants and more. Because they are relative, the window moves forward by itself on every run. Pick Custom to set **Start date** and **End date**; both accept a fixed date or a macro such as `{{365daysago}}` for a rolling year. AP Aging Summary and AR Aging Summary use a single **Report date** instead, which is the as-of date for the aging.
- **Columns** (step) — **Columns** appears for detail reports (Account List Detail, AP Aging Detail, AR Aging Detail, Customer Balance Detail, General Ledger Detail, Journal Report, Profit and Loss Detail, Vendor Balance Detail and the Transaction List reports). Pick the fields you want; leave it empty and QuickBooks returns its default set. Columns grouped under multicurrency or location tracking only return data if those features are switched on in your company.
- **Advanced settings** (step) — **Report structure** decides the shape of summary and detail reports. QuickBooks-style format mirrors the QuickBooks screen; Values-by-row format (summary reports) or Flat structure (detail reports) gives one value or one transaction per row, which is what Power BI handles best. **Query parameters** pass extra QuickBooks report options as key-value pairs, for example `accounting_method` set to `Accrual`.

### Step 3. Blend and transform data

- **Column management** — drop the reference columns nobody reads, rename QuickBooks' API names into labels your finance team recognizes, reorder fields, and set types so amounts arrive as numbers and dates as dates.
- **Join** — Coupler.io's Join attaches Customer details to Invoice rows, or Vendor details to Bill rows, on the shared ID before the data lands — or load each as its own table and relate them in Power BI's model view instead.
- **Append** — stack the same entity from several QuickBooks companies into one table, so a group with three legal entities gets one Invoice table with a company column.
- **Aggregate** — roll invoice lines up to monthly revenue per customer or per item, so Power BI starts from a small summary table rather than every line.
- **Custom SQL** — write a query over the flow's data for anything the other transformations don't cover, such as calculating days-to-pay from invoice and payment dates.

### Step 4. Power BI destination

The destination step has one sub-step, **Basic settings**, and no Power BI sign-in. Click **Generate link** and Coupler.io creates the **Integration URL** for this flow.

In Power BI Desktop, choose **Get data** → **Web**, paste the Integration URL into the From Web dialog, and click **OK**. The QuickBooks records arrive as a table in Power Query. Give the query a name that says which flow it came from, such as "QB Invoice lines" or "QB Profit and Loss Detail", because a finance model usually ends up with six or seven of them. Then, in model view, relate the Invoice and Payment tables to Customer on the customer ID, relate Bill to Vendor, and connect every transaction date to a shared date table so fiscal-year measures work across all of them.

Started from a Coupler.io dashboard template instead? Click **Download template**, open the file in Power BI Desktop, paste the Integration URL shown below the button, and click **Load**.

Treat the Integration URL like a password: anyone holding it can read your QuickBooks figures. If it leaks, regenerate it in the flow and point the Power BI source at the new link.

There is nothing else to set. Coupler.io works out the columns and their types from the QuickBooks data, so no mapping is needed.

### Step 5. Schedule

Choose how often Coupler.io refreshes the data: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. For most bookkeeping, a daily run before the working day starts is plenty; during month-end close, hourly keeps the Trial Balance current while adjustments are still being posted.

Each run replaces the data behind the Integration URL with a fresh full copy of the entity or report. Power BI picks it up when the report refreshes: click **Refresh** in Power BI Desktop, or publish to the Power BI Service and schedule a refresh on the semantic model about half an hour after Coupler.io's run. For multi-year comparisons, set the QuickBooks Reports period to Last Fiscal Year plus the current one, or a Custom range starting at `{{365daysago}}` or earlier. Keep in mind this destination holds the latest copy, not an archive: if you need snapshots of a balance at past dates, export them as separate flows.

### Bonus: or just ask the AI Agent to build it for you

Prefer to describe the result rather than click through the setup? Open the Coupler.io AI Agent in the right sidebar of the Coupler.io app and write what you need:

> *"Export QuickBooks invoices with each line split into its own row, join the customer name and email, and also set up a monthly Profit and Loss Summary for this fiscal year. Get both ready to load into Power BI."*

The Agent picks the entity and report, sets **Split by**, adds the Join, and chooses a sensible **Report period**. If something is ambiguous, such as which fiscal year you mean, it asks.

A few steps stay with you:

1. **Sign in to QuickBooks** — the Intuit login and company consent are yours to give.
2. **Choose the company** — if your login covers several QuickBooks companies, you pick which one feeds the flow.
3. **Paste the Integration URL** — in Power BI Desktop, under **Get data** → **Web**.
4. **Set the Power BI refresh** — click Refresh or schedule it in the Power BI Service.

## What data can you export from QuickBooks to Power BI with Coupler.io?

The QuickBooks connector offers 33 data entities and QuickBooks Reports adds 28 reports. Each flow carries one entity or one report. Coupler.io's Join and Append combine them before the data reaches Power BI, for example invoices with customer details, or the same report from several companies — or load each as a separate table and link them in Power BI's model view.

- **Account** — your chart of accounts with type, detail type, current balance and active status. The dimension every financial visual groups by.
- **Attachable** — receipts, contracts and other files attached to transactions, with the record each one belongs to. Useful for checking which expenses lack backup.
- **Bill** — supplier bills with due dates, open balances and, split by line, the expense account or item on each line.
- **BillPayment** — payments made against bills, with the payment account and the bills each payment settled.
- **Budget** — the budgets set up in QuickBooks, by account and period. The "plan" side of any budget-versus-actual page.
- **Class** — your classes, used to tag transactions by business line, location or project.
- **CompanyInfo** — company name, address, fiscal year start and country. Handy as a lookup when you combine several companies.
- **CreditMemo** — credits issued to customers, with amounts and remaining credit. Needed for accurate net revenue.
- **Customer** — every customer and sub-customer with contact details, terms, balance and parent relationship.
- **Department** — departments or locations, the second tagging dimension next to classes.
- **Deposit** — bank deposits and the payments or receipts each one bundles, for matching cash to the bank feed.
- **Employee** — employees with hire dates and status. Pairs with TimeActivity for labor reporting.
- **Estimate** — quotes sent to customers, with status and amounts. Shows how much of the pipeline turns into invoices.
- **ExchangeRate** — currency rates stored in QuickBooks, for restating foreign-currency transactions.
- **Invoice** — customer invoices with dates, due dates, totals and balance due; split by line for item-level sales.
- **Item** — products and services with type, prices, cost and quantity on hand.
- **JournalEntry** — manual journal entries and their debit and credit lines, for auditing adjustments.
- **JournalCode** — journal codes used by companies whose local accounting rules require them.
- **Payment** — customer payments received and the invoices they were applied to. The basis for days-to-pay analysis.
- **PaymentMethod** — the payment methods defined in QuickBooks, such as card, check or bank transfer.
- **Preferences** — company-level settings like accounting method and enabled features. Mostly useful for auditing a setup.
- **Purchase** — expenses paid directly by card, check or cash, with the account and payee.
- **PurchaseOrder** — purchase orders to suppliers, with status and expected amounts.
- **RefundReceipt** — refunds paid back to customers, so refunds don't hide inside revenue figures.
- **SalesReceipt** — sales paid on the spot, with items and amounts. Combine with Invoice for total sales.
- **TaxAgency** — tax agencies your company files with.
- **TaxCode** — tax codes applied to transactions.
- **TaxRate** — the individual tax rates behind each tax code.
- **Term** — payment terms such as Net 30, used to calculate expected payment dates.
- **TimeActivity** — time entries by employee or vendor, with customer, service item, hours and billable status.
- **Transfer** — money moved between your own accounts, so transfers can be excluded from income and spending.
- **Vendor** — suppliers with contact details, terms and open balance.
- **VendorCredit** — credits received from suppliers, which reduce what you owe.

> **Note:** The reports below are available in the **QuickBooks Reports** source in Coupler.io, separate from the QuickBooks source above. Pick QuickBooks Reports when you create the flow, then choose the report in **Report name**.

- **Account List Detail** — the chart of accounts as a report, with Account No., Type, Detail Type, Description and Balance.
- **AP Aging Detail** — each unpaid bill with Supplier, Due Date, Past Due days and Open Balance.
- **AP Aging Summary** — what you owe per supplier, bucketed by how overdue it is, as of the **Report date**.
- **AR Aging Detail** — each open invoice with Customer, Due Date, Past Due days and Open Balance.
- **AR Aging Summary** — receivables per customer in aging buckets, as of the **Report date**.
- **Balance Sheet** — assets, liabilities and equity at the end of the period, optionally by month or by class.
- **Cash Flow** — the statement of cash flows, split into operating, investing and financing activity.
- **Customer Balance** — the outstanding balance per customer.
- **Customer Balance Detail** — the invoices, payments and credits behind each customer's balance.
- **Customer Income** — income and expenses attributed to each customer, for per-client profitability.
- **Expenses by Vendor** — total spend per supplier for the period.
- **General Ledger Detail** — every posting by account, with Date, Transaction Type, Name, Split, Amount and running Balance.
- **Inventory Valuation Summary** — quantity on hand, average cost and asset value per inventory item.
- **Journal Report** — every transaction as debits and credits by account, the audit trail in report form.
- **Profit and Loss Summary** — income, cost of goods sold and expenses, with columns by month, quarter, class or department.
- **Profit and Loss Detail** — the transactions behind each P&L line, with Date, Name, Memo/Description, Split and Amount.
- **Sales by Class Summary** — revenue per class.
- **Sales by Customer** — revenue per customer, matching the QuickBooks sales report.
- **Sales by Department** — revenue per department or location.
- **Sales by Product** — quantity and revenue per product or service.
- **Tax Summary** — tax collected and paid per tax code for the period.
- **Transaction List** — all transactions of the period with type, account, split account and amount.
- **Transaction List by Customer** — the same list grouped by customer.
- **Transaction List by Vendor** — the same list grouped by supplier, including PO status.
- **Transaction List with Splits** — each transaction with all its split lines, including product, quantity and rate.
- **Trial Balance** — debit and credit balance per account, the starting point for month-end checks.
- **Vendor Balance** — the outstanding balance per supplier.
- **Vendor Balance Detail** — the bills, payments and credits behind each supplier balance.

## Why export QuickBooks to Power BI — real life cases

These are the finance reports teams actually build once QuickBooks data refreshes into Power BI on its own.

- **Monthly P&L matrix with year-over-year comparison**

  Export Profit and Loss Summary with **Display columns by** set to Month and the Values-by-row format, so each account and month is one row. A Power BI matrix with accounts on rows and months on columns reproduces the familiar statement, and a DAX measure using SAMEPERIODLASTYEAR on a proper date table adds the prior-year column and the variance. Drill-down from account group to account replaces flipping between QuickBooks report tabs.

- **Collections dashboard with alerts on overdue receivables**

  Load AR Aging Detail with Customer, Due Date, Past Due and Open Balance, then build KPI cards for total overdue and average days past due, plus a bar chart by customer. Pin the overdue card to a Power BI dashboard and set a data alert so the credit controller gets notified when it crosses a threshold. A second table from the Payment entity shows who is paying faster or slower than their Term.

- **Budget versus actual with what-if scenarios**

  Coupler.io's Join lines up the Budget entity with Profit and Loss Detail on account before loading — or relate the two tables in Power BI's model on account instead. A DAX measure computes variance, conditional formatting flags lines over budget, and a what-if parameter lets the owner test "what if revenue grows 8% instead of 5%" right in the report.

- **Supplier spend broken down with a decomposition tree**

  Combine Bill and Purchase split by line with the Vendor and Account tables. Power BI's decomposition tree then lets a controller start from total spend and break it down by vendor, then account, then class, picking the next level based on where the biggest numbers sit. Expenses by Vendor from QuickBooks Reports serves as a check that the totals match QuickBooks.

- **Consolidated view across several QuickBooks companies**

  Groups with more than one QuickBooks company can use Coupler.io's Append to stack the same Invoice or Profit and Loss Summary flow from each company into one table, tagged by company. Row-level security then limits each country or entity manager to their own company, and the whole set is shared as a Power BI app so nobody needs QuickBooks access to see results.

- **Billable time and client profitability**

  TimeActivity holds hours by employee, customer and service item, along with billable status. Relate it to Customer and Invoice, and a drill-through page per customer shows hours logged, hours billed and revenue side by side. Customer Income from QuickBooks Reports adds the expense side for a real profit-per-client figure.

- **Cash position for the owner's phone**

  Balance Sheet with **Display columns by** set to Month and Cash Flow give the bank balance trend and where the cash went. Build a small page of KPI cards and a line chart, then design a mobile layout for it in Power BI Desktop so the owner checks cash from the Power BI mobile app in the morning instead of logging in to QuickBooks.

- **Month-end close pack as a paginated report**

  Trial Balance, General Ledger Detail and Journal Report are the documents accountants review at close. Load them into one semantic model, build a paginated report that prints cleanly page by page, and set an email subscription in the Power BI Service so the close pack lands in the partner's inbox on the first working day of each month.

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
- [Connect Shopify to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md)
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
