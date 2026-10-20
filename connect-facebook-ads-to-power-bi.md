---
title: Connect Facebook Ads to Power BI with Coupler.io
slug: connect-facebook-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md
source: Facebook Ads (Meta Ads)
destination: Power BI
meta_title: Connect Facebook Ads to Power BI with Coupler.io
meta_description: Bring Facebook Ads insights, campaigns, ad creatives, sponsored leads, activity logs, and invoices into Power BI on a schedule, with no CSV exports.
---

![Facebook Ads](https://static.coupler.io/importers/facebook.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Facebook Ads to Power BI with Coupler.io

Power BI Desktop ships without a built-in connector for Meta's advertising insights, so most teams live with one of two workarounds: download a CSV from Ads Manager every Monday, or write Graph API calls in Power Query and then nurse expiring tokens, paged responses, and async report jobs. Neither survives a second ad account or a new breakdown. Coupler.io handles the Meta side for you: pick the report, the accounts, and the date window, and Power BI gets a ready-made table behind a single web link that refreshes on your schedule.

## How to export Facebook Ads data to Power BI

The wizard covers performance insights, the account structure (campaigns, ad sets, ads, creatives), lead form submissions, the account change log, and Business Manager invoices. You sign in to Meta once, then move through the screens below. Several of them only show up for certain report types, and each note says which.

### Step 1. Basic settings

- **Report type** — choose what this flow pulls. The rest of the wizard rearranges itself around the answer, so pick this first. All nine options are described further down.
- **Ad accounts** — select one or many. When you choose several, Coupler.io returns them in one table with the account name on every row, which suits agencies and brands running regional accounts. Shown for every report type except leads and invoices.
- **Businesses** — appears only for **List of Business invoices**. Choose the Business Manager accounts whose bills you want.
- **Pages** — appears only for **List of Sponsored Leads**. Choose the Facebook Pages that own your lead forms.

### Step 2. Report period

Shown for **Reports and Insights**, **List of Sponsored Leads**, and **List of Activities**.

- **Start date** — the first day of data. The relative picker gives you *days ago*, *this month*, *last month*, *this year*, and *last year*, so the window moves forward by itself with every run. For a rolling twelve months, type `{{365daysago}}`.
- **End date** — the last day of data. Leaving it on *today* keeps every refresh current.
- **Split data by period** — insights only. Choose Daily, Weekly, Monthly, or Totals only. Daily is the default and the right choice if Power BI will plot trends or use time intelligence.

### Step 3. Date filters

Shown only for **List of Business invoices**. All four fields are optional.

- **Billing start date** — the first billed month to include. If you leave it blank, you get the last six months.
- **Billing end date** — the last billed month. Blank means up to today.
- **Issue start date** — filter by the date Meta issued the invoice instead. Once set, the billing period filter is ignored.
- **Issue end date** — the latest issue date to keep.

### Step 4. Metrics and dimensions

Shown only for **Reports and Insights**.

- **Metrics and dimensions** — the columns of your table. The defaults (Account name, Campaign id, Campaign name, Amount spend, Reach, Frequency, Impressions, Clicks, CPC, CPM, CTR) are a sensible start. Add ROAS, video plays, ranking fields, or any of the `Conversions:` actions your pixel tracks, including custom events.
- **Breakdowns** — split rows by audience or delivery: Age, Gender, Country, Region, Publisher platform, Platform position, Impression device, and creative asset breakdowns such as Body asset or Video asset.

### Step 5. Advanced settings

- **Attribution settings** — insights only. Choose Use ad account settings to match what Ads Manager shows, Use ad group settings, or Specify manually.
- **Action attribution windows** — only when attribution is set manually. Pick windows such as 7 days click or 1 day view, so every account reports conversions on the same terms.
- **Filters** — insights only. A Graph API filter expression that drops test campaigns or paused objects before the data reaches Power BI.
- **Lead generation forms** — leads only. Choose specific forms, or leave it empty to collect every form on the selected Pages.
- **Ad creative columns** — only for **List of Ads with ad creatives**. Choose creative attributes such as Title, Body, Call to action type, Thumbnail URL, or Instagram permalink URL. Leave it blank to get all of them; the core ad fields are always included.
- **Updated since** — only for **List of Ads with ad creatives**. Returns only the ads changed after a given date.

### Step 6. Blend and transform data

- **Column management** — rename `Amount spend` to `Spend` and other fields to the names your measures expect, hide the columns nobody reads, reorder, and set data types before Power BI sees anything.
- **Join** — attach campaign metadata or CRM deal data to insights rows on a shared key, so Power BI receives one wide table.
- **Append** — stack Facebook Ads rows with Google Ads, TikTok Ads, or LinkedIn Ads results to build one cross-channel spend table.
- **Aggregate** — roll daily rows up to weeks or months when the report only needs that grain and you want a lighter model.
- **Custom SQL** — write a query for the logic the other transformations can't express, such as a cost-per-purchase column or a CASE that maps campaign names to funnel stages.

### Step 7. Power BI destination

The only sub-step is **Basic settings**, and you don't sign in to anything. Open Power BI Desktop, choose **Get data** → **Web**, then return to Coupler.io and click **Generate link**. Copy the **Integration URL**, paste it into the From Web dialog, and click **OK**. The insights arrive as a table. Rename the query to something like `Meta Insights`, mark the date column on a proper date table, and if you run a second flow for **List of Campaigns**, relate the two on Campaign id in the model view.

There's no schema or column mapping to fill in, because Coupler.io works out the columns and their types from the data. Treat the Integration URL as you would a password, since it carries an access token. If you regenerate it in the flow, update the Power BI source to the new URL.

If you started from a Coupler.io dashboard template, the path is shorter: click **Download template**, open the file in Power BI Desktop, paste the Integration URL shown below the button, and click **Load**.

### Step 8. Schedule

Choose how often Coupler.io pulls from Meta: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. There is no append or replace choice here. Each run swaps the data behind the Integration URL for a fresh, complete copy of the window you set. Power BI picks it up when the report refreshes, either with Refresh in Desktop or through a scheduled refresh on the semantic model after you publish to the Power BI Service. Set that refresh about half an hour after Coupler.io's run so it doesn't read the old copy.

Meta keeps insights for about 37 months. To keep a long history in the report, set a wide **Start date**. This destination holds only the latest copy, not an archive, so anything Meta drops will eventually leave the report too.

### Bonus: or just ask the AI Agent to build it for you

You can also describe the flow to the Coupler.io AI Agent. It sits in the right sidebar of the Coupler.io app and can fill in the wizard from a plain request:

> *"Create a flow with daily Facebook Ads insights for the last 90 days: campaign name, ad set name, amount spend, impressions, link clicks, purchases, and purchase ROAS, broken down by publisher platform. Then get it ready to load into Power BI."*

The Agent chooses **Reports and Insights**, picks the metrics and breakdown, sets the dates, and prepares the Power BI destination. It asks a question only when the request leaves something open.

A few steps stay with you:

1. **Sign in to Meta** — the Facebook login is yours to complete.
2. **Choose the ad accounts or Pages** — and, for lead flows, the **Lead generation forms**, which you select in the wizard.
3. **Paste the Integration URL into Power BI Desktop** — through Get data → Web.
4. **Set the refresh in Power BI** — click Refresh in Desktop, or add a scheduled refresh in the Service.

## What data can you export from Facebook Ads to Power BI with Coupler.io?

There are nine report types, and each flow exports one of them. That's why a typical setup runs one insights flow next to a few structural lists. Coupler.io's Join can merge them into a single table before they land. You can also load each as its own table and relate them in Power BI's model view.

- **Reports and Insights** — delivery and results over time: spend, reach, frequency, clicks, cost metrics, video completion, ROAS, and every conversion action, split by the breakdowns you choose. It's the fact table behind almost every Meta report.
- **List of Campaigns** — one row per campaign, with objective, status, budget, and schedule. It works as a dimension table that labels each insights row with its purpose.
- **List of Ad sets** — targeting, optimization goal, status, and remaining budget for each ad set, so you can group results by how delivery was set up.
- **List of Ads** — every ad with its status, bid amount, and any issues Meta has flagged, linked to its ad set and campaign.
- **List of Ad creatives** — the creative library: format, text, links, and media, including assets that never went live.
- **List of Ads with ad creatives** — ads paired with the creatives they actually ran. Use it to rank headlines and thumbnails by results without building a join yourself.
- **List of Sponsored Leads** — lead form submissions with each person's answers, the time they submitted, and the ad and campaign behind them.
- **List of Activities** — the account's audit log: who paused, edited, or rebudgeted what, and when.
- **List of Business invoices** — monthly invoices for each bill-to entity, with billed amounts and the campaigns included in each one.

## Why export Facebook Ads to Power BI — real life cases

These are the Meta reports people build once the data refreshes without anyone touching it.

- **Daily pacing board with alerts**

  Build a page of KPI cards for Amount spend, CPM, and Frequency against a monthly budget held in a small DAX measure. Pin the cards to a Power BI dashboard and add data alerts to the tiles, so the media buyer gets a notification when daily spend passes the planned pace or Frequency goes above 3. No one needs to open Ads Manager to notice that a campaign is overspending.

- **ROAS combo chart that matches finance's definition**

  Plot Amount spend as columns and Purchase ROAS as a line on a combo chart, split by campaign. Then add a DAX cost-per-purchase measure that divides Amount spend by `Conversions: Purchases (FB pixel)`, counted under a fixed **Action attribution windows** setting. Because every account uses the same window, the numbers stay comparable from one account to the next, and the platform ROAS and your own measure sit on the same visual.

- **Audience heatmap by age, gender, and region**

  Pull insights with the Age, Gender breakdown in one flow and Country, Region in another. A matrix with conditional formatting on CTR and cost per purchase shows the weak cells right away, and a filled map shows where spend goes compared with where buyers are. Slicers for campaign and month let the planner check whether a pattern holds.

- **Creative gallery with drill-through**

  Load **List of Ads with ad creatives** with Title, Body, Call to action type, and Thumbnail URL, and set Thumbnail URL to the Image URL data category. A table visual then shows each ad's thumbnail next to its CTR and spend. Right-click any row to drill through to a page with that creative's daily trend and placement split from Publisher platform, Platform position.

- **Annotating performance swings with the change log**

  Relate **List of Activities** to the insights table through a shared date table. A line chart of cost per result, with a tooltip page listing that day's activities, shows the budget edit or audience change behind a jump. Agencies that share an account with the client use it to settle "who changed what" from the record.

- **Lead-quality report for the sales team**

  **List of Sponsored Leads** combined with CRM outcomes through Coupler.io's Join shows which forms and campaigns produce leads that turn into opportunities, not just submissions. Publish the page in a Power BI app for the sales team and set an email subscription that sends the week's lead counts by campaign every Monday morning.

- **Invoice reconciliation for finance**

  Put **List of Business invoices** next to monthly spend aggregated from insights, and a variance measure shows any gap between what Meta billed and what the campaigns report. A paginated report formatted for print gives the controller a month-end document with one section per bill-to entity, ready to file with the accruals.

- **Client portal with row-level security**

  Load several ad accounts into one flow and set up row-level security roles on Account name. Each client, or each regional manager, sees only their own rows in the same published report. The mobile layout keeps the top KPIs readable on a phone, so clients check results without asking for a screenshot.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

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
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
