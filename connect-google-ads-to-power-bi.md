---
title: Connect Google Ads to Power BI with Coupler.io
slug: connect-google-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md
source: Google Ads
destination: Power BI
meta_title: Connect Google Ads to Power BI with Coupler.io
meta_description: Load Google Ads campaign, keyword, search query, shopping, and geo reports into Power BI on a schedule, plus custom GAQL. Refreshed with no scripts.
---

![Google Ads](https://static.coupler.io/importers/googleadv.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Google Ads to Power BI with Coupler.io

Google Ads has no native connector in Power BI. The usual options are scheduled report emails, a spreadsheet someone keeps up to date, or a custom Power Query script that calls the Google Ads API with a developer token and an OAuth flow that Power BI wasn't built to handle. Each new manager account or report type multiplies the effort. Coupler.io connects with your Google login, runs the report you pick across every account you select, and serves the result to Power BI as a web table that stays current.

## How to export Google Ads data to Power BI

The wizard covers prebuilt performance reports (campaigns, ad groups, ads, keywords, search terms, audiences, geography, shopping, video), the account's structural objects and reference constants, and a free-form GAQL query when none of those fit. There are only two source screens before you blend the data and connect Power BI.

### Step 1. Basic settings

- **Ad accounts** — choose the client accounts to report on. Only active accounts your Google login can access appear in the list. Pick several, and the results come back together in one table.
- **Report type** — the report to export. It's grouped in the app as Frequently Used, Reports, Core Components, Campaign Settings, Strategies and Constants, and Miscellaneous, and every option is listed further down.
- **Custom GAQL** — appears only when **Report type** is **Custom GAQL**. Paste a Google Ads Query Language statement here. Google's Query Builder helps you put one together field by field.

### Step 2. Report period

Shown for the performance reports only. Core components, settings, constants, and **Custom GAQL** skip this step (in a GAQL query, you set the dates inside the query itself).

- **Start date** — defaults to 60 days ago. The relative date picker also offers *days ago*, *this month*, *last month*, *this year*, and *last year*, which keeps the window rolling without edits. For a full trailing year, type `{{365daysago}}`.
- **End date** — defaults to today, so the latest complete day is always included.
- **Split data by periods** — choose one or more of Date, Day of week, Month, Quarter, Week, and Year. Picking both Date and Day of week lets one table serve trend lines and a weekday heatmap. Keywords performance doesn't offer Day of week, and Click performance comes split by Date only.

### Step 3. Blend and transform data

- **Column management** — rename Google's API field names to friendly labels, convert cost columns to the right numeric type, and drop the columns your report pages never use.
- **Join** — combine Campaign performance with the **Campaign budgets** list, or with CRM revenue keyed on campaign, so each row already carries its budget or pipeline value.
- **Append** — put several Google Ads flows under one another, or add Microsoft Ads and Facebook Ads rows, for a single paid-search and paid-social spend table.
- **Aggregate** — collapse Date rows to Month to keep a long Search query performance history small enough for a fast model.
- **Custom SQL** — classify search terms into brand and non-brand with a CASE expression, or compute cost per conversion before the data reaches Power BI.

### Step 4. Power BI destination

This step has one sub-step, **Basic settings**, and nothing to authorize. In Power BI Desktop, go to **Get data** → **Web**, which the app's instructions call "Get data from Web". Back in Coupler.io, click **Generate link** to produce the **Integration URL**. Paste the URL into the dialog and click **OK**. The report loads as one table you can shape in Power Query. Name the query after the report type, for example `GAds Campaign performance`, so a model with several flows stays readable, and connect its Date column to your calendar table.

Coupler.io takes the column names and types from the data, so there's no schema to describe. The Integration URL includes an access token, so share it only as you would a password. If you regenerate it, point the Power BI source at the new link.

Started from a Coupler.io dashboard template instead? Click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown under the button, and click **Load**.

### Step 5. Schedule

Choose a refresh interval in Coupler.io: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Google Ads reporting data settles over a few hours as conversions are attributed, so a daily early-morning run suits most accounts. Every run replaces the data behind the Integration URL with a complete new copy of the date range. There's no append mode for this destination. Power BI shows the new numbers on its next refresh, either manually in Desktop or on a schedule you set for the published semantic model in the Power BI Service, timed shortly after Coupler.io finishes.

For long history, set a wide **Start date** and let **Split data by periods** at Month keep the row count down. This destination holds the latest copy rather than an archive, so a range you shorten later disappears from the report on the next run.

### Bonus: or just ask the AI Agent to build it for you

Prefer to describe it? Open the Coupler.io AI Agent in the right sidebar of the Coupler.io app and write what you need:

> *"Set up Google Ads search query performance for all my accounts since the start of last year, split by month, so I can review search terms. Prepare it for loading into Power BI."*

The Agent picks **Search query performance**, sets the dates and the period split, and prepares the Power BI destination. If something in the request is unclear, such as which accounts to include, it asks before building.

What stays in your hands:

1. **Connect Google Ads** — the sign-in is always done by you.
2. **Choose the ad accounts** — you decide which accounts the flow covers.
3. **Paste the Integration URL** — into Power BI Desktop, under Get data → Web.
4. **Schedule the Power BI refresh** — in Desktop, or on the semantic model in the Service.

## What data can you export from Google Ads to Power BI with Coupler.io?

Google Ads offers 53 report types in Coupler.io. Each flow carries one of them, so a full model is usually a handful of flows. Coupler.io's Join and Append can combine them into one table before Power BI loads it. Alternatively, keep them as separate tables and relate them in Power BI's model view.

Frequently used:

- **Campaign performance** — daily results per campaign: impressions, clicks, cost, conversions, and value. Most Google Ads dashboards start here.
- **Keywords performance** — results per keyword and match type, for bid reviews and Quality Score checks.
- **Ad group performance** — the same metrics one level down, for spotting which themes within a campaign carry it.
- **Ad performance** — results per ad, so text and format tests can be compared directly.
- **Asset group performance** — Performance Max results by asset group, the closest view inside those campaigns.
- **Custom GAQL** — any query you write in Google Ads Query Language, for fields or resources the prebuilt reports leave out.

Reports:

- **Account performance** — totals per account, suited to manager-level roll-ups.
- **Ad group audience performance** — how audience segments attached to ad groups perform.
- **Ad group performance by country** — ad group results split by country.
- **Ad group performance by state** — ad group results split by state.
- **Ad group performance by region** — ad group results split by region.
- **Ad group performance with install conversion** — ad group results including app install conversions.
- **Age range performance** — results by age bracket.
- **Campaign audience performance** — audience segment results at the campaign level.
- **Campaign performance by ad network type** — campaign results split across Search, Display, YouTube, and partner networks.
- **Campaign performance by country** — campaign results by country.
- **Campaign performance by state** — campaign results by state.
- **Campaign performance by region** — campaign results by region.
- **Campaign performance with conversion actions name** — campaign results broken out per named conversion action.
- **Campaign performance with unique user stats** — campaign reach and frequency based on unique users.
- **Click performance** — click-level records, available per day.
- **Display keyword performance** — keywords targeted on the Display Network and their results.
- **Display topics performance** — topic targeting results on Display.
- **Expanded landing page** — results by final landing page URL after redirects.
- **Gender performance** — results by gender.
- **Keywordless query** — queries matched by dynamic search ads instead of keywords.
- **Landing page** — results by the landing page set on your ads.
- **Placement performance** — where Display and video ads appeared and how each placement did.
- **Responsive search ad performance** — results for responsive search ads.
- **Search query performance** — the actual searches that triggered your ads, the source for negative keyword work.
- **Shopping performance** — product-level Shopping results.
- **User location performance** — results by where users physically were.
- **Video campaign performance with conversions** — video campaign results with conversion columns.

Core components, settings, and constants:

- **Accounts** — the accounts themselves, with their settings.
- **Ad groups** — every ad group with its status and campaign.
- **Ads** — every ad with its type and status.
- **Campaigns** — campaign settings, a natural dimension table for any performance report.
- **Campaign budgets** — budget amounts and delivery method for each campaign budget.
- **Campaign criterion** — targeting criteria set at campaign level.
- **Campaign labels** — the labels attached to campaigns, handy for custom groupings in slicers.
- **Accessible bidding strategies** — bidding strategies the account can use, including shared ones from a manager.
- **Bidding strategies** — portfolio bidding strategies and their targets.
- **Carrier constant** — reference list of mobile carriers.
- **Language constant** — reference list of targetable languages.
- **Mobile app category constant** — reference list of app categories.
- **Mobile device constant** — reference list of mobile devices.
- **Operating system version constant** — reference list of OS versions.
- **Topic constant** — reference list of Display topics.
- **Ad group criterion** — keywords, audiences, and other criteria set on ad groups.
- **Call details** — calls from call ads and assets, with duration and status.
- **Labels** — all labels defined in the account.
- **User interest** — the interest categories available for targeting.
- **User list** — your remarketing and customer match lists with their sizes.

## Why export Google Ads to Power BI — real life cases

Here's what search and shopping teams build once Google Ads lands in their Power BI model every morning.

- **Year-over-year trend page with time intelligence**

  Use **Campaign performance** split by Date with a Start date of *last year*, and relate it to a marked date table. DAX measures built on SAMEPERIODLASTYEAR and DATESYTD then put this year's cost and conversions next to last year's on a line chart. The seasonal comparison that took an afternoon in spreadsheets updates itself.

- **Weekday heatmap for bid scheduling**

  Split **Campaign performance** by Day of week as well as Date. A matrix with Day of week on rows, campaign on columns, and background color on conversion rate shows which days earn their budget. Bookmarks switch the same matrix between cost per conversion and conversion volume, so the PPC manager can review both views quickly.

- **Search term mining with a decomposition tree**

  Load **Search query performance** monthly and add a Custom SQL step that tags brand versus non-brand queries. A decomposition tree splits cost by campaign, then by tag, then by query, and shows the expensive non-converting terms that belong on the negative list. A slicer on the minimum cost threshold keeps the long tail out of view.

- **Shopping product review for merchandisers**

  **Shopping performance** at product level, joined in Coupler.io with your margin table, feeds a scatter chart of revenue against cost per product. A drill-through page shows each product's daily trend. Merchandisers can see which items to push or pull from campaigns without asking the PPC team.

- **Geographic split with a filled map**

  **Campaign performance by region** and **User location performance** fill a map visual coloured by cost per conversion. Paired with a region slicer, it shows where a national budget is underperforming and supports a case for location bid adjustments or a separate regional campaign.

- **Budget pacing with a what-if parameter**

  Join **Campaign performance** with **Campaign budgets** and add a what-if parameter for a budget change percentage. A DAX measure estimates spend at the new level, and a KPI card shows month-to-date spend against budget. Account leads can test "what if we raise this campaign by 20%" in the meeting rather than after it.

- **Switchable metric dashboard with field parameters**

  Build one visual layout on **Ad group performance** and drive it with a field parameter covering cost, clicks, conversions, and conversion value. One slicer changes every chart on the page, so a single report page replaces four almost identical ones, and the report stays small enough to maintain.

- **Agency roll-up shared in Teams**

  Select every client account in one **Account performance** flow, publish the report, and embed the page in each client team's Microsoft Teams channel. Row-level security on the account name keeps every client to their own numbers, and the mobile layout lets an account director check the morning numbers on a phone.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
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
