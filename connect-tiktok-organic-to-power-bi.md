---
title: Connect TikTok Organic to Power BI with Coupler.io
slug: connect-tiktok-organic-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md
source: TikTok Organic
destination: Power BI
meta_title: Connect TikTok Organic to Power BI with Coupler.io
meta_description: Load TikTok profile insights, audience genders, countries, hourly activity and video stats into Power BI on a schedule, with no API scripts.
---

![TikTok Organic](https://static.coupler.io/importers/tiktok.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect TikTok Organic to Power BI with Coupler.io

TikTok's own analytics tab is built for creators scrolling on a phone, not for a team report. There is no native TikTok connector in Power BI, the in-app export is a manual download, and profile data only reaches back about 60 days, so a report that relies on someone remembering to export goes stale fast. Coupler.io signs in to your TikTok account, pulls profile, audience, and video statistics on a timer, and gives Power BI a single link per flow that always returns the latest data.

## How to export TikTok Organic data to Power BI

TikTok Organic has two kinds of data: profile and audience reports for the account, and video lists with per-video results. Setup is short, and here is every field you will see.

### Step 1. Basic settings

- **Report type** — the one choice on this screen. Six reports are available: four about the profile and its audience, two about your videos. The profile you get data for is the TikTok account you connected.

### Step 2. Report period

Only the four profile reports use dates. **Video list top countries** and **Video list insights** skip this step.

- **Start date** — where the window begins. TikTok doesn't return data older than 60 days, so 60 *days ago* is the furthest you can go and is also the default. Shorter relative picks such as *this month* or *last month* work fine; *this year* and *last year* ask for more than TikTok keeps.
- **End date** — where the window ends. It defaults to yesterday, the last full day TikTok has numbers for.

### Step 3. Blend and transform data

- **Column management** — keep the TikTok columns your visuals need, give them readable names, and fix types before loading.
- **Join** — attach **Video list top countries** to **Video list insights** by video so each video row carries its top markets.
- **Append** — put TikTok profile rows under rows from other short-form channels with matching columns, for one cross-platform table.
- **Aggregate** — total daily profile rows into weeks before they reach Power BI.
- **Custom SQL** — run your own query over the data, for example to rank videos by engagement per view.

### Step 4. Power BI destination

The destination step has just **Basic settings** and no Power BI login. Click **Generate link** to get the **Integration URL** for this TikTok flow. Open Power BI Desktop, choose **Get data** → **Web**, paste the address into the From Web dialog, and click **OK**. The TikTok report shows up as a table for Power Query. If you run both a profile flow and a video flow, load them as separate queries, for example "TikTok profile" and "TikTok videos", and mark the profile table's date column against your date table.

Coupler.io works out the columns and types from TikTok's data, so you don't map anything. Treat the Integration URL as a secret, since it carries an access token. Regenerating it in the flow means pointing Power BI's Web source at the new link.

Working from a Coupler.io dashboard template? Click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** displayed under the button, and click **Load**.

### Step 5. Schedule

Set the flow to run every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. For TikTok, a daily morning run lines up with the yesterday end date. Each run replaces the data behind the Integration URL with a full new copy; Power BI flows don't append. Power BI loads it when the report refreshes, either with Refresh in Desktop or through a scheduled refresh on the semantic model in the Power BI Service, set to run after Coupler.io. Be clear on one limit: TikTok keeps about 60 days, and this destination holds only the latest copy, not an archive. Older days fall out of the Power BI report as the window moves.

### Bonus: or just ask the AI Agent to build it for you

You can also open the Coupler.io AI Agent in the right sidebar of the app and describe the TikTok report you want:

> *"Export TikTok profile insights for the last 60 days, and separately my video list insights, and load both into Power BI."*

The Agent picks the report types and dates and sets up the flow. You still do these yourself:

1. **Connect TikTok** — the login to your TikTok account is yours to give.
2. **Confirm the connected account** — the profile you sign in with is the one reported on.
3. **Paste the Integration URL into Power BI Desktop** — via Get data → Web.
4. **Schedule the Power BI refresh** — in Desktop or the Power BI Service.

## What data can you export from TikTok Organic to Power BI with Coupler.io?

Six report types are available, one per flow. Coupler.io's Join links the two video reports by video before loading, or you can bring each in as a table and connect them in Power BI's model view.

- **Profile audience genders** — the gender split of your audience over the period, the figure brand partners ask for first.
- **Profile audience countries** — where your viewers are, ready for a map and for decisions about caption languages.
- **Profile audience hourly activity** — when your audience is active, hour by hour, which turns posting time into a data question.
- **Profile insights** — the account's daily performance, the trend line at the top of a TikTok report.
- **Video list top countries** — the countries each video was watched in most, to spot clips that travelled beyond your home market.
- **Video list insights** — one row per video with its results, the table for ranking what worked.

## Why export TikTok Organic to Power BI — real life cases

Here's what social and creator teams build in Power BI once TikTok data refreshes without anyone exporting it.

- **A posting-time heatmap**

  Load **Profile audience hourly activity** and lay it out as a matrix with hours across and days down, with background colour scaled to activity. The darkest cells are the posting slots, and because the matrix refreshes daily, the team sees when the audience's habits shift instead of relying on last quarter's guess.

- **Daily account check on a phone**

  Build a mobile layout for the **Profile insights** report page with a few KPI cards and a line chart. Pin the cards to a dashboard in the Power BI Service and set data alerts on the tiles, so the social lead gets a notification when a day comes in well under the usual level.

- **Media kit that updates itself**

  Combine **Profile audience genders** and **Profile audience countries** on one report page with a donut chart and a filled map. Export it to PDF before a brand call, or share the link; the figures are always from the last run, so the pitch deck never quotes stale demographics.

- **Which videos travel abroad**

  Join **Video list top countries** to **Video list insights** in Coupler.io, then use a scatter chart in Power BI with views on one axis and the share of non-home-market views on the other. Clips in the top corner are the ones worth subtitling or reposting for other markets.

- **Video review in the team's Teams channel**

  A table visual on **Video list insights** sorted by views, with a slicer for publish week, is added as a tab in the content team's Microsoft Teams channel. The weekly review happens right there, and nobody has to open TikTok or pass a spreadsheet around.

- **TikTok next to other short-form channels**

  Use Coupler.io's Append to stack TikTok profile numbers under similar rows from other video platforms with the same column names. In Power BI a clustered column chart with a platform slicer compares channels on one page, which helps decide where the next batch of videos should go first.

- **Being honest about the 60-day window**

  Because TikTok stops at about 60 days and this destination keeps the latest copy, build the report around rolling comparisons: DAX measures for the last 7 days against the 7 before, and a 28-day trend on a line chart. The report stays accurate within what TikTok provides, rather than implying a long history it doesn't hold.

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
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
