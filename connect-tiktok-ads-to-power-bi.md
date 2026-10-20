---
title: Connect TikTok Ads to Power BI with Coupler.io
slug: connect-tiktok-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md
source: TikTok Ads
destination: Power BI
meta_title: Connect TikTok Ads to Power BI with Coupler.io
meta_description: Load TikTok Ads campaign, ad, audience, GMV Max, DSA, and Playable Ad reports into Power BI on a schedule and compare creatives in one model.
---

![TikTok Ads](https://static.coupler.io/importers/tiktok.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect TikTok Ads to Power BI with Coupler.io

Power BI has no built-in TikTok Ads connector, so teams usually download spreadsheets from TikTok Ads Manager or try to call the Marketing API from Power Query, which means handling access tokens, paging, and the limits on which metrics and dimensions can be requested together. Creatives on TikTok also wear out within days, so a weekly CSV is out of date before anyone opens it. Coupler.io pulls the report you configure from TikTok on a schedule and publishes it at a URL that Power BI reads directly, so the creative and spend pages stay current without manual downloads.

## How to export TikTok Ads data to Power BI

Coupler.io covers performance at the advertiser, campaign, ad group, and ad level, audience breakdowns, Dynamic Showcase Ads, Playable Ads, GMV Max for TikTok Shop, and plain lists of campaigns and ads. You connect your TikTok For Business login once and then work through these steps.

### Step 1. Basic settings

- **Report type** — the data you want in Power BI. Each option fixes the grain of the table and which of the later fields you'll see. All 18 are listed further down.
- **Ad accounts** — pick the advertiser accounts to include. If an account is missing from the list, reconnect TikTok under Connections in Coupler.io.
- **Stores** — shown only for **GMV Max campaign report**. Choose the TikTok Shop stores whose GMV Max campaigns you want; the list depends on the ad accounts you picked.

### Step 2. Report period

- **Start date** — where the data starts. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the window rolls forward on its own. For a full trailing year, use the macro `{{365daysago}}`. Not shown for **Campaigns** and **Ads**, which are lists rather than reports.
- **End date** — where the window ends. Keep it relative to today and nobody has to update the flow. Also hidden for **Campaigns** and **Ads**.
- **Split data by period** — **Daily**, **Hourly**, or **Totals only** for the basic reports and the **GMV Max campaign report**. Audience and Dynamic Showcase Ads reports offer **Daily** and **Totals only**. Playable Ad reports have no split.

### Step 3. Metrics and dimensions

- **Metrics** — the columns in your report, grouped into attribute metrics (names, budgets, objectives, statuses) and performance metrics such as **Cost**, **Impressions**, **Reach**, **Frequency**, **Conversions**, **Cost per result**, **2-second video views**, **6-second video views**, **Video views at 100%**, and **Purchase ROAS (website)**. Shown for every report type except **Campaigns** and **Ads**.
- **Audience dimensions** — for the four audience reports. Group rows by **Gender**, **Age**, **Country code**, **Province**, **DMA region**, **Language**, **Platform**, **Placement**, **Interest category** (down to **Interest category fourth level**), and more.
- **DSA dimensions** — for the Dynamic Showcase Ads reports: **Catalog id**, **Product set id**, **Product id**, or **Sku id**.
- **Targeting dimension** — for the basic reports. Optionally split by **Country code** or **Ad type**.
- **Asset dimension** — also for the basic reports. Optionally group by an asset such as **Video material id**, **Image id**, **Post id**, or **Page id**.
- **Search dimension** — for the basic reports. Choose **Search terms** to see which TikTok searches triggered your ads.

### Step 4. Advanced settings

- **Advanced filters** — a property and value pair that limits the fetched rows, for example a single campaign or objective. Available for every report type except **Ads** and **GMV Max campaign report**.

### Step 5. Blend and transform data

- **Column management** — remove the dozens of conversion columns you don't track, rename labels like **Clicks (destination)** to fit your report's naming, and set types so Power BI sums cost and averages rates correctly.
- **Join** — match TikTok ad rows to the **Ads** list or to Shopify orders on a shared ID, so creative details or real revenue sit on the same row as spend.
- **Append** — stack Meta, Snapchat, or Pinterest campaign rows under TikTok to make one cross-platform social spend table.
- **Aggregate** — roll hourly or daily data up to campaign-week totals before it leaves Coupler.io, which helps when the model mainly needs trends.
- **Custom SQL** — for logic the other steps can't express, such as a calculated hook rate per creative, write the query and Coupler.io loads its result.

### Step 6. Power BI destination

You won't be asked to sign in to Power BI. The destination step has one sub-step, **Basic settings**:

1. Click **Generate link** to create the flow's **Integration URL**.
2. In Power BI Desktop, go to **Get data** → **Web**, the "Get data from Web" option the app points you to.
3. Paste the Integration URL and click **OK**.
4. The TikTok report loads as a table. In Power Query, name it after its grain, such as *TikTok Ad Daily* or *TikTok Audience Age Gender*, so the field list stays readable once more flows are added.

A common TikTok model has a daily ad-level fact table and a separate **Ads** or **Campaigns** list as a lookup. Relate them on the ad or campaign ID, mark your own date table, and connect it to the date column of the performance table. Coupler.io's Join can also flatten the two into one table before load if you'd rather keep the model to a single table.

You never define columns by hand: Coupler.io works out names and types from the TikTok response. For a flow started from a Coupler.io dashboard template, click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown beneath the button, and click **Load**.

Anyone holding the Integration URL can read the data because it embeds an access token, so keep it private. If it gets out, regenerate it in the flow and update the Power BI source to the new URL.

### Step 7. Schedule

Choose a refresh interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. There is no append or replace setting for Power BI. Each run overwrites what sits behind the Integration URL with a full fresh copy. Power BI reads it at its own refresh: click **Refresh** in Power BI Desktop, or publish to the Power BI Service and schedule a semantic model refresh shortly after Coupler.io finishes.

During a launch, an hourly Coupler.io run paired with several Power BI Service refreshes a day lets the team pause underperforming creatives the same afternoon. Remember that each run is a complete replacement rather than an addition to an archive, so the start date controls how much history Power BI has. Pick *this year* or *last year* if you need long trends.

### Bonus: or just ask the AI Agent to build it for you

Open the AI Agent in the right sidebar of the Coupler.io app and describe the report:

> *"Set up a daily TikTok Ads ad-level report for the last 60 days with ad name, campaign name, cost, impressions, 2-second and 6-second video views, video views at 100%, conversions, and cost per conversion, and prepare it to load into Power BI."*

The Agent selects the report type, metrics, dimensions, and dates, and asks when there's a genuine choice to make. What stays with you:

1. **Connect TikTok For Business** — it's your login, so you authorise it.
2. **Pick the ad accounts** (and stores for GMV Max) — you decide what's in scope.
3. **Add the Integration URL to Power BI** — through **Get data** → **Web** in Power BI Desktop.
4. **Set up the refresh** — in Power BI Desktop or on the semantic model in the Power BI Service.

## What data can you export from TikTok Ads to Power BI with Coupler.io?

There are 18 report types. Each flow handles one, so a typical setup pairs a daily ad report with an audience report and the **Ads** list. Coupler.io's Join merges them into a single table before delivery, or you can load each one separately and relate them in Power BI's model view.

- **Basic campaign report** — spend, reach, clicks, and conversions per campaign, with budget and automation type alongside. The headline table for most TikTok pages.
- **Basic ad group report** — the same metrics per ad group, where targeting and bidding are set, so you can compare audiences.
- **Basic ad report (Upgraded Smart+ by ad)** — results per ad, with Upgraded Smart+ ads rolled up to the ad (asset group) level. Filtering by ad ID isn't supported for this one.
- **Basic ad report (Upgraded Smart+ by creative)** — results per ad, but Upgraded Smart+ ads are broken down to individual creatives. Best for creative testing.
- **GMV Max campaign report** — TikTok Shop GMV Max results: **Cost**, **Net cost**, **Orders (SKU)**, **Cost per order**, **Gross revenue**, and **ROI** for the stores you choose.
- **Campaigns** — a list of your campaigns and their settings, with no metrics or dates. Works as a lookup table.
- **Ads** — a list of your ads and their attributes, the natural partner for the ad-level reports.
- **Basic advertiser report** — totals per ad account, for agency overviews and monthly summaries.
- **Audience campaign report** — campaign performance split by the audience dimensions you select, such as age, gender, or placement.
- **Audience advertiser report** — account-level performance by audience, to see who your TikTok spend actually reaches.
- **Audience ad report** — audience breakdowns per ad, showing which creative works for which group.
- **Audience ad group report** — audience breakdowns per ad group, to check whether targeting delivers to the people you meant to reach.
- **Playable Ad report** — results for each playable ad, with **Playable name** and **Playable status**.
- **Playable Ad report by country** — playable ad results broken out per market, useful for app marketers launching market by market.
- **Dynamic Showcase Ads (DSA) campaign report** — catalog ad results per campaign, split by catalog, product set, product, or SKU.
- **Dynamic Showcase Ads (DSA) report** — catalog ad results per ad, so you can see which product feeds and ads sell.
- **Dynamic Showcase Ads (DSA) advertiser report** — catalog ad totals per account.
- **Dynamic Showcase Ads (DSA) ad group report** — catalog ad results per ad group, for comparing product sets and audiences.

## Why export TikTok Ads to Power BI — real life cases

Below are eight Power BI reports built on TikTok Ads data, each matched to how TikTok campaigns are run day to day.

- **Creative hook and hold funnel**

  Use **Basic ad report (Upgraded Smart+ by creative)** with **Impressions**, **2-second video views**, **6-second video views**, and **Video views at 25%** through **Video views at 100%**. A funnel visual per ad shows where viewers drop off, and DAX measures turn the counts into a hook rate and a completion rate. A slicer on ad name lets the creative team compare two edits side by side before choosing which one to scale.

- **TikTok Shop GMV Max profit view**

  The **GMV Max campaign report** for your **Stores** feeds KPI cards for **Gross revenue**, **Net cost**, **Orders (SKU)**, and **ROI**, and a combo chart of daily cost against revenue. A what-if parameter for target ROI draws a reference line, making it obvious which campaigns fall below the break-even point the finance team agreed.

- **Age and gender performance matrix**

  Use the **Audience campaign report** with **Age** and **Gender** as audience dimensions. A matrix with age bands on rows and gender on columns, coloured by **Cost per conversion**, shows where conversions are cheapest. Add **Platform** as a small multiples field and you can compare iOS and Android side by side without a separate page for each.

- **Interest category drill-down**

  Add **Interest category**, **Interest category second level**, **Interest category third level**, and **Interest category fourth level** to the **Audience ad group report** and build a hierarchy from them in Power BI. A decomposition tree then breaks spend from the top category down to the most specific level, so a broad "Beauty" audience reveals whether skincare or makeup is driving conversions.

- **Dayparting with bookmarks**

  Set **Split data by period** to **Hourly** on the **Basic ad group report**. A heatmap matrix of hour by weekday shows when **CPM** spikes and when **CTR (destination)** peaks. Bookmarks switch the same matrix between cost, click-through, and conversion views with a single button, which keeps the page simple enough for media buyers adjusting delivery schedules.

- **Catalog ads by SKU against store revenue**

  Pull the **Dynamic Showcase Ads (DSA) report** with **Product id** and **Sku id**. Coupler.io's Join attaches the SKU's price and margin from your store, and a scatter chart plots ad cost against attributed conversions for each product. SKUs that cost a lot and sell little are easy to spot and exclude from the product set.

- **App cohort ROAS and playable ads by market**

  Mobile app teams track **Day 0 purchase ROAS (TikTok)**, **Day 2 purchase ROAS (TikTok)**, **Calendar day 6 purchase ROAS (TikTok)**, and **Day 13 purchase ROAS (TikTok)** on a line chart to see how returns build after install. A second page maps the **Playable Ad report by country** on a filled map. Data alerts on the dashboard tiles notify the user acquisition lead when Day 0 ROAS falls below target.

- **Agency workspace with row-level security**

  Agencies select every client account in **Ad accounts** and pull the **Basic advertiser report** and **Basic campaign report**, including **Paid likes**, **Paid shares**, and **Paid follows** for engagement-focused clients. Row-level security keyed on **Account name** lets each client see only their own data in a shared Power BI app, and the report is pinned in a Microsoft Teams channel for weekly check-ins.

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
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
