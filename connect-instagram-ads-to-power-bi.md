---
title: Connect Instagram Ads to Power BI with Coupler.io
slug: connect-instagram-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-ads-to-power-bi.md
source: Instagram Ads
destination: Power BI
meta_title: Connect Instagram Ads to Power BI with Coupler.io
meta_description: Send Instagram Ads insights, campaigns, ad sets, and ads to Power BI on a schedule and see which placements and videos earn follows and sales.
---

![Instagram Ads](https://static.coupler.io/importers/instagram-ads.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Instagram Ads to Power BI with Coupler.io

Instagram campaigns are run from Meta's Ads Manager, and Power BI can't read that data directly. There's no built-in connector, and getting the numbers per placement means either exporting CSVs filtered to Instagram or calling the Marketing API from Power Query and handling paging and token renewal yourself. Coupler.io pulls the insights and the campaign structure for you and hands Power BI one Integration URL, so the social team reports on stories, reels, and feed results without the manual export step.

## How to export Instagram Ads data to Power BI

There are two kinds of data: performance insights you can split by placement, audience, and device, and the lists of campaigns, ad sets, and ads that give those numbers names. Sign in with the Facebook account that manages your ads, then work through four short screens. Everything after the first screen applies to insights only.

### Step 1. Basic settings

- **Ad accounts** — the Meta ad accounts behind your Instagram campaigns. Select one or several, and with several, every row is tagged with the account it came from.
- **Report type** — choose between performance insights and the three structure lists. The four options are described further down the page.

### Step 2. Report period

Shown for **Reports and Insights** only.

- **Start date** — set the opening day with the relative picker: *days ago*, *this month*, *last month*, *this year*, or *last year*. A relative value moves forward every run. For a rolling year, `{{365daysago}}` does it.
- **End date** — usually *today*, so the newest delivery is always in the report.
- **Split data by period** — Daily (the default), Weekly, Monthly, or Totals only. Use Weekly if the report is a Monday recap, or Daily if you want trend lines.

### Step 3. Metrics and dimensions

Shown for **Reports and Insights** only.

- **Metrics and dimensions** — the columns of the insights table. Alongside spend, reach, impressions, and clicks, Instagram teams usually add Instagram profile visits, Instagram profile follows, Video ThruPlay, the Video plays at 25% to 100% set, Purchase ROAS, and conversion actions such as `Conversions: Post Saves` or `Conversions: Post Shares`.
- **Breakdowns** — choose Publisher platform, Platform position to separate Instagram feed, stories, reels, and explore. Other options include Age, Gender, Country, Region, and Impression device.

### Step 4. Advanced settings

Shown for **Reports and Insights** only.

- **Attribution settings** — Use ad account settings to match Ads Manager, Use ad group settings, or Specify manually.
- **Action attribution windows** — available once attribution is set manually. Choose the click and view windows conversions should count under.
- **Filters** — a Graph API filter for skipping objects you don't want, such as campaigns whose names mark them as tests.

### Step 5. Blend and transform data

- **Column management** — shorten long metric labels such as `Video plays at 100%` so the Power BI field list stays readable, and set numeric types on the ranking and ROAS columns.
- **Join** — add the ad set's optimization goal from **List of Ad sets** to each insights row, so visuals can group results by what Meta was optimizing for.
- **Append** — combine Instagram insights with TikTok Ads or YouTube-heavy Google Ads results into one short-form video spend table.
- **Aggregate** — total daily placement rows into weekly figures for an executive page that doesn't need day-level detail.
- **Custom SQL** — compute cost per profile follow, or map Platform position values to the friendly names your team uses.

### Step 6. Power BI destination

You don't log in to Power BI from Coupler.io. The step has a single **Basic settings** sub-step. In Power BI Desktop, pick **Get data** → **Web**. In Coupler.io, click **Generate link**, copy the resulting **Integration URL**, paste it into the From Web dialog, and press **OK**. The table shows up in Power Query. Call it something like `IG Insights by placement`. If you also load **List of Ads**, relate it to the insights on the ad ID so ad names and statuses show up in slicers.

Coupler.io reads the column types from the data, so you don't define a schema. Keep the Integration URL private, since it contains an access token. After regenerating it in the flow, update the Power BI data source to the new address.

For flows created from a Coupler.io dashboard template, click **Download template**, open it in Power BI Desktop, paste the **Integration URL** shown below the button, and click **Load**.

### Step 7. Schedule

Coupler.io can refresh every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. For Instagram campaigns, a daily run catches yesterday's stories and reels delivery, and an hourly run helps during a launch week. Each run replaces the dataset behind the Integration URL with a full new copy. There's no append option for Power BI. The report updates when Power BI refreshes: press Refresh in Desktop, or publish to the Power BI Service and schedule a refresh on the semantic model that starts after Coupler.io's run.

Meta's insights go back roughly 37 months. To report on more than a quarter, set an early **Start date**. Power BI only ever receives the latest copy, not an archive, so history Meta no longer serves won't stay in the report.

### Bonus: or just ask the AI Agent to build it for you

Open the Coupler.io AI Agent in the right sidebar of the Coupler.io app and tell it what the report should show:

> *"Build an Instagram Ads report for this month, split daily, with spend, reach, Instagram profile visits, Instagram profile follows, Video ThruPlay, and purchase ROAS, broken down by publisher platform and platform position. Get it ready to load into Power BI."*

It selects **Reports and Insights**, adds the metrics and breakdown, and sets up the destination. It checks with you only where the request has a real gap.

Your part is short:

1. **Sign in with Facebook** — Meta access is granted by you, not the Agent.
2. **Choose the ad accounts** — the ones your Instagram campaigns run from.
3. **Paste the Integration URL** — into Power BI Desktop through Get data → Web.
4. **Set up the refresh in Power BI** — manually in Desktop or on a schedule in the Service.

## What data can you export from Instagram Ads to Power BI with Coupler.io?

Four report types are available, one per flow. Coupler.io's Join can attach the campaign, ad set, or ad details to the insights before loading, which leaves Power BI with one table. You can also load all four separately and connect them in the model view.

- **Reports and Insights** — Instagram delivery and results by day, week, or month: spend, reach, profile visits and follows, video plays, ROAS, and conversion actions, split by placement, audience, or device.
- **List of Campaigns** — campaign objective, status, budget, and schedule, used to label insights rows with their goal.
- **List of Ad sets** — optimization goal, status, and remaining budget for each ad set, which helps explain why two similar audiences delivered differently.
- **List of Ads** — every ad with its status, bid, and flagged issues, linked to its ad set and campaign.

## Why export Instagram Ads to Power BI — real life cases

These are the Power BI pages Instagram advertisers rely on most once the data refreshes itself.

- **Placement comparison: feed, stories, reels**

  Break insights down by Publisher platform, Platform position and plot cost per result per placement on a clustered bar chart. Bookmarks switch the chart between reach, ThruPlays, and purchases, so the creative team sees where vertical video beats square video before briefing the next shoot.

- **Follower growth from paid**

  Put Instagram profile follows and Instagram profile visits on a line and clustered column chart with Amount spend, and add a DAX measure for cost per follow. A KPI card set against a monthly follower target shows whether paid is doing its share of account growth, a question Ads Manager has no built-in view for.

- **Video retention funnel**

  A funnel visual built on 2-second continuous video plays, then Video plays at 25%, 50%, 75%, and 100%, shows where viewers drop off for each ad. A slicer on campaign and a tooltip with Video average play time help editors see whether a hook is too slow or the ending is losing people.

- **Audience matrix with key influencers**

  Pull insights with the Age, Gender breakdown and run the key influencers visual on purchases. It points to the age and gender segments most associated with conversions, and a matrix next to it shows the cost per purchase for each cell. Media planners use it to set up the next round of audience tests.

- **Campaign hierarchy with drill-down**

  Load **List of Campaigns**, **List of Ad sets**, and **List of Ads** as dimension tables and relate them to insights on their IDs. A matrix with drill-down goes from campaign to ad set to ad, and a drill-through page per ad set shows its optimization goal next to its daily delivery.

- **Monday recap by email**

  Set **Split data by period** to Weekly and build a one-page summary of spend, reach, Quality ranking, and Engagement rate ranking per ad. A Power BI email subscription sends it to stakeholders every Monday, so the weekly social update no longer needs someone to make slides.

- **Phone-first report for the social team**

  Social managers are rarely at a desk. A mobile layout with the day's spend, profile follows, and top ads by Purchase ROAS, plus a slicer on Impression device, gives them the morning check on their phone. Data alerts on the spend tile flag a campaign that is overspending before lunch.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
- [Connect Google Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md)
- [Connect Google Search Console to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-search-console-to-power-bi.md)
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
