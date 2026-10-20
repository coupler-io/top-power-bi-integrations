---
title: Connect YouTube Analytics to Power BI with Coupler.io
slug: connect-youtube-analytics-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md
source: YouTube
destination: Power BI
meta_title: Connect YouTube Analytics to Power BI with Coupler.io
meta_description: Bring YouTube channel, video, subscriber and traffic source statistics, or a custom metrics report, into Power BI on a schedule for channel reviews.
---

![YouTube Analytics](https://static.coupler.io/importers/youtube.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect YouTube Analytics to Power BI with Coupler.io

YouTube Studio is good at showing one channel at a time and poor at anything else. Power BI has no ready connector for YouTube Analytics, Studio's exports are one-off files, and calling the YouTube Analytics API from Power Query means handling Google OAuth, quotas, and paging yourself. Coupler.io connects to the channels your Google account can see, pulls channel, video, subscriber, and traffic statistics on a schedule, and gives Power BI a link that always serves the latest version of the report.

## How to export YouTube Analytics data to Power BI

Coupler.io covers channel statistics, video statistics, subscriber movement, traffic sources, and a custom report built from YouTube's own metrics and dimensions. Here are the setup screens in order.

### Step 1. Basic settings

- **Channels** — sign in with the Google account that owns or manages the channels and select the ones to include. Several channels in one flow give you a single table for a whole network.
- **Report type** — the shape of the data. Choose a ready-made report such as *Video detailed statistics* (the default) or *Custom* to build your own. All seven are described further down.

### Step 2. Report period

- **Start date** — the first day covered. Pick a relative value (*days ago*, *this month*, *last month*, *this year*, *last year*) and the window rolls forward with each run.
- **End date** — the last day covered. *Today* keeps every refresh current.
- **Split data by period** — *Day* or *Month*. Shown for the channel, subscriber, and video reports; *Traffic sources* and *Custom* don't use it.

### Step 3. Metrics and dimensions

- **Metrics** — for the *Custom* report only. YouTube's metrics come in groups: Performance (*Views*, *Watch time (minutes)*, *Average view duration*, *Average view percentage*), Interactions (*Likes*, *Comments*, *Shares*, *Subscribers gained*, *Subscribers lost*), plus Cards, Playlist, Audience retention, Annotations, and Premium.
- **Dimensions** — also *Custom* only. Break the numbers down by *Age*, *Gender*, *Country*, *City*, *Device type*, *Video*, *Playlist*, *Live or on demand*, *Insight traffic source type*, *Day*, *Month*, and others.
- **Additional dimensions** — extra breakdowns for *Channel statistics by content and subscriber type*, *Channel subscribers*, *Video statistics by content and subscriber type*, and *Traffic sources*. Options depend on the report and include *Creator content type*, *Subscribed status*, *Video*, and *Live or on demand*.

### Step 4. Sorting options

- **Sorting column** — which column the rows are ordered by on arrival, for example views descending so a Power BI table opens with the top video first.

### Step 5. Advanced settings

- **Video ID** — YouTube only returns audience retention for one specific video, so paste that video's ID here when your *Custom* report uses retention metrics.
- **Insight traffic source type** — required whenever you add the *Insight traffic source detail* dimension. Choose one of ADVERTISING, EXT_URL, RELATED_VIDEO, SUBSCRIBER, YT_CHANNEL, YT_OTHER_PAGE, or YT_SEARCH to see the exact search terms, videos, or sites behind that source.

### Step 6. Blend and transform data

- **Column management** — remove columns no chart uses, rename metric keys for a non-technical audience, reorder fields, and adjust types.
- **Join** — link two YouTube flows on a shared key, such as video statistics and traffic sources on *Video*, so each video row carries its traffic mix.
- **Append** — stack YouTube under other video or social sources with matching columns for one cross-platform content table.
- **Aggregate** — sum views and watch time per channel, week, or content type before the data reaches Power BI.
- **Custom SQL** — write a query for calculations the other tools don't handle, and Coupler.io loads the output.

### Step 7. Power BI destination

You won't sign in to Power BI here; the step has only **Basic settings**. Click **Generate link** to create the **Integration URL**. In Power BI Desktop, pick **Get data** → **Web**, paste that URL into the From Web dialog, and click **OK**. YouTube's report comes in as a table ready for Power Query. A common setup is one query per report type, for example "YouTube videos" and "YouTube traffic sources", both related on the video column to a small video table, with the day column marked against a date table.

Columns and data types come from the YouTube data itself, so there's no mapping to do. The Integration URL includes an access token: keep it as private as a password. If you regenerate it in the flow, change the Web source in Power BI to the new URL.

If the flow started from a Coupler.io dashboard template, click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown below the button, and click **Load**.

### Step 8. Schedule

Run the flow every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. YouTube Analytics figures settle with a delay of a day or two, so daily is the sensible choice for most channels. Every run overwrites the data behind the Integration URL with a full current copy, since Power BI flows have no append mode. Power BI fetches it on refresh: hit Refresh in Desktop, or publish to the Power BI Service and schedule a refresh on the semantic model after Coupler.io finishes. For year-over-year comparisons, set the start date wide, for instance `{{365daysago}}` or further back with *last year*. Power BI always gets the latest copy rather than an accumulating archive, which is fine here because YouTube keeps channel history.

### Bonus: or just ask the AI Agent to build it for you

The Coupler.io AI Agent is in the right sidebar of the app. Tell it what the report should show:

> *"Give me daily Video detailed statistics for all my channels since the start of last year, plus Traffic sources broken down by video, and load both into Power BI."*

It selects the report types, period, split, and additional dimensions and creates the flow. Left for you:

1. **Connect YouTube** — sign in with your own Google account.
2. **Select the channels** — you decide which channels belong in the report.
3. **Set metrics, dimensions, and filters for a Custom report** — including the video or traffic source type if you need them.
4. **Paste the Integration URL into Power BI Desktop** — through Get data → Web, then set up the refresh in Power BI.

## What data can you export from YouTube Analytics to Power BI with Coupler.io?

Seven report types are available, and each flow returns one. Coupler.io's Join merges two of them on a shared key such as *Video* before loading, or you can bring them in separately and relate them in Power BI's model view.

- **Channel detailed statistics** — the channel's core numbers by day or month: views, watch time, average view duration, likes, comments, shares, and subscribers gained and lost.
- **Channel statistics by content and subscriber type** — the same channel figures split by content type (videos, Shorts, live) and by subscribed versus not subscribed viewers.
- **Channel subscribers** — subscriber movement over time, optionally broken down by content type or by video, to see which uploads gained or lost followers.
- **Video detailed statistics** — per-video performance for the chosen period, the default report and the base for most video rankings.
- **Video statistics by content and subscriber type** — video-level numbers by format and subscriber status, showing how loyal viewers and newcomers react to each upload.
- **Traffic sources** — where views come from: search, suggested videos, external sites, channel pages, playlists, and ads, optionally per video.
- **Custom** — your own combination of YouTube metrics and dimensions, including audience retention, cards, playlists, demographics, devices, geography, and YouTube Premium watch time.

## Why export YouTube Analytics to Power BI — real life cases

These are the Power BI reports video, marketing, and partnerships teams put together on top of the YouTube tables.

- **Channel review with time intelligence**

  Load **Channel detailed statistics** split by *Month*, relate it to a date table, and write DAX measures with SAMEPERIODLASTYEAR for views and watch time. A line and clustered column chart shows this year against last, so the monthly review answers "are we ahead?" in one visual.

- **From channel to a single video in two clicks**

  Build an overview page on **Video detailed statistics** and add a drill-through page keyed on the video title. Right-click any video in a bar chart to open its detail page with likes, comments, shares, and average view percentage, without leaving the report.

- **Shorts or long-form?**

  **Channel statistics by content and subscriber type** splits results by *Creator content type*. A ribbon chart by month shows which format is gaining share of views, and a matrix with subscribed versus not subscribed columns shows which format reaches new viewers rather than existing fans.

- **What drives subscribers**

  Point Power BI's key influencers visual at subscribers gained from a *Custom* report with dimensions such as *Device type*, *Country*, *Live or on demand*, and *Creator content type*. It ranks which conditions most raise subscriber gains, a useful starting point for the next content plan.

- **Search terms that bring viewers in**

  Set **Insight traffic source type** to YT_SEARCH and add *Insight traffic source detail* in a *Custom* report. A table of search terms with views and watch time, plus a slicer for channel, tells the team which topics people search for to find them, ready to feed titles and descriptions.

- **Sponsor recap with a what-if rate**

  Join **Video detailed statistics** with a sponsor list in Coupler.io, then add a what-if parameter for the agreed rate per thousand views. A KPI card shows the value delivered for the sponsored videos, and a paginated report turns the same data into a printable recap for the brand.

- **Agency reporting across client channels**

  Select every client channel in **Channels**, load one table, and apply row-level security on the channel column. Each client signs in to the same published Power BI app and sees only their channel, so the agency maintains one report rather than a dozen.

- **Retention curve for a flagship video**

  Use a *Custom* report with *Audience watch ratio* and *Relative retention performance* against *Elapsed video time ratio*, with the video's ID in **Video ID**. A line chart of the curve shows exactly where viewers drop off, which is the feedback editors want before cutting the next one.

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
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
