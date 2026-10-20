---
title: Connect Facebook Page Insights to Power BI with Coupler.io
slug: connect-facebook-page-insights-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md
source: Facebook Page Insights
destination: Power BI
meta_title: Connect Facebook Page Insights to Power BI with Coupler.io
meta_description: Bring Facebook Page performance, follower, post, reel, and video insights into Power BI on a schedule and trend organic reach month over month.
---

![Facebook Page Insights](https://static.coupler.io/importers/facebook.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Facebook Page Insights to Power BI with Coupler.io

The Facebook connector that Power BI Desktop once included was retired years ago, and Meta Business Suite exports organic Page data as one-off spreadsheets with column names that change whenever Meta renames a metric. Building a Graph API call in Power Query means managing Page access tokens and following pagination links, which most social teams would rather not maintain. Coupler.io fetches your Page, post, reel, and video insights on a schedule and gives Power BI a URL to read, so the organic social report refreshes along with the rest of your workspace.

## How to export Facebook Page Insights data to Power BI

Coupler.io covers Page-level performance, followers and their locations, Page profile details, and lifetime and daily results for posts, reels, and videos. You sign in with the Facebook account that manages your Pages and then go through these screens. Which fields appear depends on the report type you pick.

### Step 1. Basic settings

- **Pages** — tick the Facebook Pages you manage and want to report on. Several Pages can go into one flow, which suits brands with regional or product Pages.
- **Report type** — the kind of data the flow returns, from Page totals to per-post and per-video results. All six options are described below.

### Step 2. Report period

- **Start date** — for **Page: performance insights**, **Page: followers daily snapshot**, and **Post: video daily performance**. The relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) keeps the window moving by itself. When you need a fixed rolling year, enter `{{365daysago}}`.
- **End date** — same three report types. Leave it relative to today and each run includes the latest day.
- **Created after** — for **Post: posts lifetime performance** and **Post: reels lifetime performance**. Only posts or reels published after this date are included.
- **Created before** — the matching upper limit for those two report types.
- **Split data by period** — for **Page: performance insights** only. Choose **Daily** or **Monthly**.

### Step 3. Metrics

Shown for **Page: performance insights**, **Post: posts lifetime performance**, **Post: reels lifetime performance**, and **Post: video daily performance**.

- **Metrics** — the list changes with the report type. Page reports offer **Page views**, **New followers**, **New unique unfollows**, **Content views - total**, **Unique content viewers**, and the video and ad break counts. Post reports offer **Post views - total**, **Unique post viewers**, **Clicks by type**, **Reactions by type (like, love, wow, etc.)**, **Shares**, and video retention. Reels add **Views**, **Average view time (ms)**, and **Follows**.

### Step 4. Metrics and dimensions

- **Metrics and dimensions** — for **Page: general overview**, a multi-select of Page profile fields such as **About**, **Category**, **Website URL**, **Single line address**, **Opening hours**, **Overall star rating**, **Rating count**, **Followers**, and **Page likes**. For **Page: followers daily snapshot**, the same field title is a single choice between **Unique followers by city** and **Unique followers by country**.

### Step 5. Advanced filters

Shown only for **Post: video daily performance**.

- **Created after** — include only videos published after this date.
- **Created before** — include only videos published before this date.

### Step 6. Advanced settings

- **Post columns** — for **Post: posts lifetime performance**. Add post attributes such as **Message**, **Created time**, **Permalink URL**, **Status type**, **Attachments**, or **Promotion status**. Leave it empty to get all of them; the post ID and updated time are always included.
- **Data behavior** — for **Page: general overview**, **Post: posts lifetime performance**, and **Post: reels lifetime performance**. **Latest data** keeps only current values, while **Track changes over time** saves a daily snapshot so you can see how lifetime numbers grow.

### Step 7. Blend and transform data

- **Column management** — keep the reaction and view columns your community team reads and drop the rest, rename **Post views - paid vs. organic** to a shorter label, and set created time as a date-time column.
- **Join** — connect post rows to Facebook Ads data on the promoted post, so organic and paid results for the same content appear side by side.
- **Append** — put several Pages, or Instagram and LinkedIn page data, into one long social table with shared column names for a cross-network view.
- **Aggregate** — turn daily Page metrics into monthly totals for the board report, or count posts per week to compare publishing volume with reach.
- **Custom SQL** — for anything else, like an engagement rate per post calculated before load, write the query and Coupler.io delivers the output.

### Step 8. Power BI destination

Power BI needs no login or authorisation here. The destination's only sub-step is **Basic settings**:

1. Click **Generate link** and copy the **Integration URL** Coupler.io gives you.
2. Open Power BI Desktop and choose **Get data** → **Web** ("Get data from Web" in the in-app instructions).
3. Paste the URL in the From Web dialog and click **OK**.
4. The Page data loads as a table. Name the query to match the report type, for example *FB Page Daily* for performance insights or *FB Posts* for posts lifetime performance.

A Page analytics model usually has two tables: daily Page metrics on one side and posts on the other. Mark a date table, relate it to the daily table's date column and to the posts' **Created time** (converted to a date in Power Query), and add a small Pages table so one slicer filters both. Coupler.io's Join can attach Page-level context to each post before the data arrives, if you prefer a single table.

You don't need to define fields or types: Coupler.io infers them from the Facebook data. If the flow came from a Coupler.io dashboard template, click **Download template**, open it in Power BI Desktop, paste the **Integration URL** displayed under the button, and click **Load**.

The Integration URL includes an access token, so share it only with people who should see the data. Regenerating it in the flow invalidates the old one; repoint the Power BI source to the new URL afterwards.

### Step 9. Schedule

Choose the refresh interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. With Power BI there is no append or replace option. Every run swaps the dataset behind the Integration URL for a new full copy, and Power BI loads it when the report refreshes, either through **Refresh** in Power BI Desktop or through a scheduled refresh on the semantic model in the Power BI Service, set for after Coupler.io's run.

Organic Page metrics are finalised by the next morning, so a daily run before work starts is usually enough. Remember that this destination only keeps the latest copy, not a history log. For long trends, use a wide window on the date-based reports (for example starting *last year*). For lifetime post and reel totals, switch **Data behavior** to **Track changes over time** so daily snapshots build up in the data itself.

### Bonus: or just ask the AI Agent to build it for you

The Coupler.io AI Agent is in the right sidebar of the app. Describe the report you want:

> *"Export Facebook post lifetime performance for posts created this year, with message, created time, permalink, post views, unique viewers, reactions, comments, and shares, and set it up so I can load it into Power BI."*

The Agent picks the report type, metrics, post columns, and dates, and asks whenever more than one setting would fit. You still do the following yourself:

1. **Log in to Facebook** — the connection is made with your own account.
2. **Select the Pages** — you choose which Pages to report on.
3. **Connect Power BI to the Integration URL** — via **Get data** → **Web** in Power BI Desktop.
4. **Set the Power BI refresh** — on the published semantic model.

## What data can you export from Facebook Page Insights to Power BI with Coupler.io?

There are six report types. Each flow returns one, so a typical organic social report uses two or three flows, for example daily Page metrics plus post and reel performance. Coupler.io's Join and Append combine them into one table before Power BI loads it, or you can keep them as separate tables and relate them in Power BI's model view.

- **Page: general overview** — the Page's profile and status: category, contact details, opening hours, ratings, follower and like counts. Useful when you manage many location Pages and need to check them all in one place.
- **Page: performance insights** — daily or monthly Page totals for views, followers gained and lost, content views, post engagement, reactions by type, and video views. The basis for any Page growth trend.
- **Page: followers daily snapshot** — your followers broken down by city or by country, day by day, to see where the audience is located and how it shifts.
- **Post: posts lifetime performance** — one row per post with views, clicks, reactions, comments, shares, and video metrics accumulated over the post's lifetime, plus the post attributes you select.
- **Post: reels lifetime performance** — reel-level views, unique views, view time, audience retention, follows gained, and reactions, so reels can be judged separately from regular posts.
- **Post: video daily performance** — day-by-day results for each video post: views, organic and paid split, view time, and ad break earnings, to see how long a video keeps attracting viewers.

## Why export Facebook Page Insights to Power BI — real life cases

These eight Power BI reports come from social, community, and brand teams who need to see organic Facebook performance alongside their other data.

- **Page growth scorecard with year-over-year comparison**

  Pull **Page: performance insights** split **Daily** for a window starting *last year*. KPI cards show **New followers**, **New unique unfollows**, and **Lifetime followers**, and DAX measures with SAMEPERIODLASTYEAR place each month next to the same month last year. Net follower growth gets its own card with a target line, so leadership can see whether the Page is growing faster or slower than a year ago.

- **What makes a post spread: key influencers**

  Load **Post: posts lifetime performance** with **Status type**, **Attachments**, and **Created time** as post columns. Run Power BI's key influencers visual on **Shares** and it shows which factors raise shares, for example video posts or posts published in the morning. The content calendar then rests on your own data rather than general best-practice lists.

- **Reaction mix and sentiment over time**

  Use **Reactions - Like & Care**, **Reactions - Love**, **Reactions - Haha**, **Reactions - Wow**, **Reactions - Sad**, and **Reactions - Anger** from the post report in a 100% stacked column chart by week. A rising share of anger reactions often appears before a support issue becomes visible elsewhere. A drill-through page lists the specific posts behind any week, with **Permalink URL** set as a web URL so each post opens in one click.

- **Reels versus regular posts**

  Place **Post: reels lifetime performance** next to the posts table and compare **Views**, **Average view time (ms)**, and **Follows** on a scatter chart, with a DAX measure converting milliseconds to seconds. Reels that hold attention and bring in followers sit in the top-right corner. That gives the video team a concrete reason to make more of a format, or to stop producing it.

- **Video monetization tracker**

  **Post: video daily performance** with **Ad break earnings**, **Ad break impressions**, and **Ad break CPM** feeds a combo chart: earnings as columns, CPM as a line, one point per day. Publishers can see how long a video keeps earning after it goes live, and can set a data alert on the earnings tile so a sudden drop sends an email.

- **Follower map by city and country**

  Choose **Page: followers daily snapshot** with **Unique followers by country** in one flow and **Unique followers by city** in another. A filled map shows country totals, and a slicer on date shows how the audience shifts after a campaign or a regional launch. Retail and events teams use this to decide where to run local promotions.

- **Location Page portfolio with ratings**

  Franchises and multi-location brands pull **Page: general overview** for every location Page, with **Overall star rating**, **Rating count**, **Followers**, **Single line address**, and **Opening hours**, and set **Data behavior** to **Track changes over time**. A map visual plots each location by rating, and a line chart tracks how ratings move. Row-level security keyed on Page lets each regional manager see only their own locations in a Power BI app.

- **Paid versus organic content views**

  Combine **Content views - paid vs. organic** and **Page likes by source (organic, paid)** from Page insights with Facebook Ads spend that Coupler.io joins on date. A line and stacked column chart shows how much of the Page's reach is paid for each month, and a DAX measure estimates the cost per organic follower. Bookmarks switch the page between a reach view and a follower view during the monthly marketing review.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
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
