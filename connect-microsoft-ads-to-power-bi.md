---
title: Connect Microsoft Ads to Power BI with Coupler.io
slug: connect-microsoft-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-microsoft-ads-to-power-bi.md
source: Microsoft Ads (Bing Ads)
destination: Power BI
meta_title: Connect Microsoft Ads to Power BI with Coupler.io
meta_description: Send Microsoft Ads campaign, keyword, search query, geographic, and product reports to Power BI on a schedule, ready for DAX and refresh.
---

![Microsoft Ads](https://static.coupler.io/importers/microsoft.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Microsoft Ads to Power BI with Coupler.io

Microsoft Advertising lives inside the Microsoft ecosystem, yet getting its numbers into Power BI is still a chore. The Reports page hands you CSV downloads one report at a time, and the Reporting API expects you to submit a job, poll until it finishes, unzip the result, and keep an OAuth token alive, all of which is painful to rebuild in Power Query. Coupler.io handles that part. You pick a report, choose the columns, and Coupler.io gives you a URL that Power BI reads like any web source, refreshed on the schedule you set.

## How to export Microsoft Ads data to Power BI

Coupler.io covers campaign, ad group, ad, keyword, and account performance, search queries, ad extensions, product and hotel ads, audiences and locations, budgets, and change history. You sign in with your Microsoft Advertising account once, then move through the wizard below.

### Step 1. Basic settings

- **Ad accounts** — pick one account or many. When you select several, the rows arrive in one dataset with the account identified, which suits agencies and multi-brand advertisers who report on all of them together.
- **Report type** — decides what each row represents: a campaign, a keyword, a search term, a location, a product partition, and so on. The full list of 45 is further down the page. **Custom report** is the open-ended option where you assemble the metrics and dimensions yourself.

### Step 2. Report period

- **Report period** — a ready-made window such as *Last 7 days*, *Last 30 days*, *This month*, *Last month*, *Last 6 months*, *This year*, or *Last year*, or **Custom** if you want to set the dates yourself. The **Search campaign change history report** has a shorter list of presets, and the **Negative keyword conflict report** skips this field because it has no date range.
- **Start date** — appears only when **Report period** is set to **Custom**. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the window moves forward by itself. For a rolling twelve months, the macro `{{365daysago}}` does the job.
- **End date** — also only for **Custom**. A date in the future is capped at today, so leaving it relative to today keeps every run current.
- **Split data by period** — the time grain: **Summary**, **Hourly**, **Daily**, **Weekly from Sunday**, **Weekly from Monday**, **Monthly**, or **Yearly**. Daily is the default and the one to keep if Power BI will draw trend lines. Not shown for the budget summary, change history, and negative keyword conflict reports.

### Step 3. Metrics and dimensions

Shown only for **Custom report**.

- **Metrics** — choose the numbers to pull, from **Impressions**, **Clicks**, **Spend**, and **Conversions** through **Return on ad spend**, **Impression share percent**, **Top impression share lost to rank percent**, **Low quality clicks**, and the video completion metrics.
- **Dimensions** — choose how those numbers are grouped, for example **Account name** and **Campaign name**.

### Step 4. Dimensions

Shown for every report type except **Custom report**.

- **Dimensions** — pick the attributes you want on each row, such as **Campaign name**, **Keyword**, **Device type**, **Network**, **Quality score**, or **City**. You don't pick metrics here: every metric the report supports comes along automatically.

### Step 5. Advanced settings

- **Advanced filters** — enter a property and a value to narrow the rows before they are fetched, for instance a single campaign or one status. Not available for the **Budget summary report** or the **Product match count report**.

### Step 6. Blend and transform data

- **Column management** — drop the columns your report pages never use, turn labels like `AdGroupName` into something a stakeholder recognises, and fix data types so Power BI treats spend as currency and dates as dates.
- **Join** — attach CRM deals or e-commerce orders to Microsoft Ads rows on a shared key, so cost and revenue share a row before the data reaches Power BI.
- **Append** — stack Google Ads and other search or social accounts under your Microsoft Ads rows to get one paid media table with matching columns.
- **Aggregate** — collapse hourly or daily rows into the weekly or monthly totals a board pack needs, which also keeps the Power BI model small.
- **Custom SQL** — write a query against the combined data when the built-in steps are not enough, and Coupler.io loads the query result.

### Step 7. Power BI destination

The destination has one sub-step, **Basic settings**, and Power BI doesn't need to be authorised in Coupler.io at all.

1. Click **Generate link**. Coupler.io creates the **Integration URL** for this flow.
2. In Power BI Desktop, choose **Get data** → **Web** (the in-app hint reads "Get data from Web").
3. Paste the Integration URL into the From Web dialog and click **OK**.
4. The report loads as a table. Open Power Query, rename the query to something like *MSAds Keywords Daily*, and check the column types.

Running more than one Microsoft Ads flow, say a campaign report and a keyword report? Load each as its own query, add a date table and mark it as the model's date table, then relate both report tables to it on their date column. A small campaign table keyed on **Campaign id** lets slicers filter keywords and campaigns together. If you would rather avoid the extra relationships, Coupler.io's Join can merge the two flows before they land.

Coupler.io decides the columns and their types from the report itself, so there is nothing to map. If you started from a Coupler.io dashboard template instead, click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown below the button, and click **Load**.

The Integration URL contains an access token, so share it as carefully as a password. You can regenerate it in the flow if it leaks; afterwards, point the Power BI data source at the new URL.

### Step 8. Schedule

Set how often Coupler.io refreshes the report: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Each run replaces the data behind the Integration URL with a full fresh copy, so there is no append or replace choice to make. Power BI picks up the new copy when the report refreshes. In Power BI Desktop that means clicking **Refresh**; in the Power BI Service, publish the report and schedule a refresh on the semantic model about half an hour after Coupler.io's run.

Microsoft Advertising keeps assigning conversions to clicks for days after they happen, so a rolling *Last 30 days* window that is rewritten every morning gives you settled figures without manual reruns. Keep in mind that this destination holds the latest copy only, not an archive. If you want two years of keyword data in Power BI, set the period wide enough to include them, for example **Custom** starting *last year*.

### Bonus: or just ask the AI Agent to build it for you

Coupler.io's AI Agent sits in the right sidebar of the app. Tell it what you need in plain words and it configures the flow:

> *"Build a daily Microsoft Ads keyword report for the last 90 days with Keyword, Campaign name, Device type, Quality score, impressions, clicks, spend, and conversions, and get it ready to load into Power BI."*

The Agent chooses the report type, the dimensions, the period, and the split, and it asks you about anything it can't infer. A few things are still yours to do:

1. **Sign in to Microsoft Advertising** — the connection uses your own login.
2. **Choose the ad accounts** — you decide which accounts belong in the report.
3. **Paste the Integration URL** — in Power BI Desktop, under **Get data** → **Web**.
4. **Schedule the Power BI refresh** — on the semantic model in the Power BI Service.

## What data can you export from Microsoft Ads to Power BI with Coupler.io?

There are 45 report types. A flow pulls one of them, so most advertisers run a performance report next to a keyword or search query report. Coupler.io's Join and Append can combine those flows before they arrive, so Power BI gets one finished table, or you can load each one as a separate table and relate them in Power BI's model view.

- **Campaign performance report** — spend, clicks, conversions, and revenue per campaign over time. The usual starting point for a paid search overview page.
- **Keyword performance report** — results per keyword together with **Quality score**, **Expected CTR**, **Ad relevance**, and **Landing page experience**, so you can see why a keyword costs what it does.
- **Ad group performance report** — the same results one level down, useful when campaigns hold many themes and the averages hide the weak groups.
- **Ad performance report** — each ad's title, description, type, and status beside its results, for comparing copy variants.
- **Ad extension by ad report** — how sitelinks, callouts, and other extensions perform on each ad they appear with.
- **Ad extension by keyword report** — extension performance per keyword, showing which searches extensions actually help.
- **Ad extension detail report** — results per individual extension item, so you can retire the ones nobody clicks.
- **Call detail report** — calls generated by call extensions, for advertisers who measure leads by phone.
- **Budget summary report** — spend per campaign against its budget, in the account's **Currency code**, for keeping an eye on pacing.
- **Search campaign change history report** — who changed what and when, with **Changed by**, **Attribute changed**, **Old value**, and **New value**.
- **DSA auto target performance report** — results for each dynamic search ads auto target.
- **DSA category performance report** — dynamic search ads results by the website categories Microsoft matched.
- **DSA search query performance report** — the actual searches that triggered dynamic search ads and what they delivered.
- **Hotel dimension performance report** — hotel ads results by hotel attributes.
- **Hotel dimension performance report with share performance statistics** — the same view with impression share columns added, showing how much of the available demand you won.
- **Hotel group performance report** — hotel ads results by the hotel groups you bid on.
- **Hotel group performance report with share performance statistics** — hotel group results together with share and lost-share figures.
- **Account performance report** — totals per ad account, handy for a top-line page across many accounts.
- **Account performance report with share performance statistics** — account totals plus impression share, to see whether budget or rank is holding you back.
- **Ad dynamic text performance report** — results for ads that use dynamic text, broken down by the text served.
- **Ad group performance report with share performance statistics** — ad group results along with share metrics, pinpointing the groups that miss impressions.
- **Asset performance report** — results per asset, such as headlines and images in responsive ads.
- **Asset group performance report** — performance for each asset group in Performance Max campaigns.
- **Audience performance report** — results for the audiences attached to your campaigns and ad groups.
- **Campaign performance report with share performance statistics** — campaign results together with impression share and lost-share columns.
- **Conversion performance report** — conversions by **Goal** and goal type, so each tracked action can be reported separately.
- **Destination URL performance report** — results by landing page URL, for judging which pages turn clicks into conversions.
- **Goals and funnels report** — conversion goal and funnel step results across campaigns.
- **Publisher usage performance report** — where on the Microsoft network and partner sites your ads appeared and how each placement did.
- **Search query performance report** — the searches people typed, with **Delivered match type** and the keyword that matched. The main source for new keywords and negatives.
- **Share of voice report** — impression share by keyword, with quality signals beside it, for gauging competitive pressure.
- **Product dimension performance report** — Shopping results by product attributes such as brand or category.
- **Product dimension performance report with share performance statistics** — the same product view with share metrics added.
- **Product match count report** — how many products each product group matches, useful when checking feed coverage.
- **Product partition performance report** — Shopping results per **Product group** and **Country of sale**.
- **Product partition performance report with share performance statistics** — product group results plus impression share.
- **Product partition unit performance report** — results at the most granular product partition level.
- **Product search query performance report** — the searches that triggered Shopping ads.
- **Age gender audience report** — results split by age band and gender.
- **Geographic performance report** — results by **Country**, **State**, **Metro area**, **City**, and **Postal code**.
- **Negative keyword conflict report** — negative keywords that block your own active keywords, listed so you can fix them.
- **Professional demographics audience report** — results by company, industry, and job function, a LinkedIn-sourced view specific to Microsoft Advertising.
- **User location performance report** — results by where the searcher physically was, as opposed to the location they searched for.
- **Custom report** — your own combination of metrics and dimensions, for the cases the ready-made reports don't cover.

## Why export Microsoft Ads to Power BI — real life cases

Here are eight Power BI reports that search marketers and their managers build from Microsoft Ads data.

- **Keyword health matrix with drill-through**

  Load the **Keyword performance report** and put **Keyword** on the rows of a Power BI matrix, with **Quality score**, **Expected CTR**, **Ad relevance**, and **Landing page experience** as columns using icon-based conditional formatting. Account managers spot the red cells at a glance. Right-clicking a keyword opens a drill-through page that shows its daily spend and **Historical quality score**, so a fall in score can be linked to a landing page release.

- **Lost impression share in a decomposition tree**

  Pull the **Campaign performance report with share performance statistics** and analyse **Impression share percent**, **Impression lost to budget percent**, and **Impression lost to rank agg percent** with a decomposition tree, splitting by campaign, then **Device type**, then **Network**. It answers a recurring question in budget meetings: would more money help, or does the ad need better quality and bids first?

- **Search term mining page**

  The **Search query performance report** with **Delivered match type** and **Bid match type** becomes a table filtered to the Top N searches by spend, with slicers for campaign and match type. Searches with clicks but no conversions go straight onto a negatives list. A second visual shows the **Negative keyword conflict report**, so the team also catches the negatives that are blocking keywords they want to keep.

- **Regional performance on a filled map**

  The **Geographic performance report** split by **State** and **Metro area** feeds a filled map coloured by cost per conversion, with a slicer for **Campaign type**. Regional managers zoom into their own area, and **Postal code** is there when a store-level catchment view is needed. Row-level security keyed on region means each manager sees only their own territory when the report is shared through a Power BI app.

- **Spend trend annotated with account changes**

  Put daily spend from the **Campaign performance report** on a line chart, and place the **Search campaign change history report** in a table next to it, synced to the same date slicer. Clicking a spike filters the table to that day's **Changed by**, **Attribute changed**, **Old value**, and **New value** entries. A tooltip page on the line chart lists the changes on hover, so nobody has to dig through the Microsoft Advertising history screen.

- **Shopping profitability scatter chart**

  Load the **Product partition performance report** and plot each **Product group** on a scatter chart with spend on the x-axis and **Return on ad spend** on the y-axis, sized by **Revenue** and filtered by **Country of sale**. Product groups in the high-spend, low-return corner become the agenda for the next feed and bid review. Coupler.io's Join can attach margin data from your store first, so the chart shows profit rather than revenue.

- **Hour-of-day and device heatmap**

  Set **Split data by period** to **Hourly** on the **Ad group performance report** and include **Device type**. A matrix with hours on columns and weekdays on rows, coloured by conversion rate, shows when mobile clicks convert and when they don't, which tells you where to set ad schedule bid adjustments. Give the page a mobile layout so the PPC lead can check it on a phone.

- **Blended paid search report for clients**

  Coupler.io's Append combines Microsoft Ads and Google Ads campaign rows into one table, and DAX measures calculate blended CPA and each engine's share of spend. A date table with SAMEPERIODLASTYEAR adds year-over-year comparison. Agencies publish a paginated report version for the monthly client PDF and set up email subscriptions, so the report arrives on the first of the month without anyone exporting it.

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
- [Connect QuickBooks to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-quickbooks-to-power-bi.md)
- [Connect Shopify to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md)
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
