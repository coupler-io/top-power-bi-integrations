---
title: Connect LinkedIn Public Data to Power BI with Coupler.io
slug: connect-linkedin-public-data-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-linkedin-public-data-to-power-bi.md
source: LinkedIn Company Pages
destination: Power BI
meta_title: Connect LinkedIn Public Data to Power BI with Coupler.io
meta_description: Send LinkedIn Company Page performance, follower gains, demographics, post and video stats to Power BI on a schedule, ready for page reporting.
---

![LinkedIn Public Data](https://static.coupler.io/importers/linkedin.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect LinkedIn Public Data to Power BI with Coupler.io

Power BI has no built-in connector for LinkedIn Company Pages. The usual workaround is downloading the page analytics files from LinkedIn's admin view every month, or asking someone to write Power Query calls against LinkedIn's API and keep the OAuth tokens alive. Both break the moment the person who set them up goes on holiday. Coupler.io connects to your Company Pages with your LinkedIn login, turns page, follower, post, and video statistics into tidy tables, and hands Power BI one web address per flow that always serves the latest numbers.

## How to export LinkedIn Public Data data to Power BI

A flow covers one report type for as many Company Pages as you manage: page performance, follower growth and demographics, audience demographics, or post and video results. These are the screens you go through and what each field expects.

### Step 1. Basic settings

- **Pages** — the Company Pages to pull from, listed from the LinkedIn account you signed in with. Select several and the rows arrive together with the page identified on each one, which suits a group with regional pages or an agency looking after a few brands.
- **Report type** — what the flow returns. The current choices are grouped under Page, Audience, and Post, nine in total, and each one is explained further down. The next steps only show the fields your choice needs.

### Step 2. Report period

This step appears for **Page: performance insights**, **Page: cumulative posts performance**, **Page: follower gains trend**, **Page: audience demographics insights**, and **Post: video performance**. Lifetime and snapshot reports skip it.

- **Start date** — the first day included. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the window moves forward without anyone opening the flow again. For **Page: cumulative posts performance** the dates are optional, but once you set an end date a start date is needed too.
- **End date** — the last day included; *today* keeps every run current.
- **Split data by period** — the time grain of the rows: Daily, Weekly, Monthly, or Totals only. It isn't shown for **Page: audience demographics insights**, and not every report offers all four: **Page: follower gains trend** stops at Monthly, and **Post: video performance** offers Daily and Weekly only.

### Step 3. Dimensions

- **Dimension** — shown for the two demographics reports. **Page: audience demographics insights** can be cut by Seniority, Country, Industry, Company size, Job function, or Geo (market area). **Page: follower demographics insights** offers the same set plus Association type. One dimension per flow, so a second cut means a second flow.

### Step 4. Metrics

- **Metrics** — used only by **Post: video performance**. Pick any mix of Views, Viewers, Time viewed (ms), and Time viewed for views (ms).

### Step 5. Blend and transform data

- **Column management** — hide columns no visual will use, rename LinkedIn's field names into something a comms lead recognises, and set types, for example making the millisecond watch-time columns plain numbers.
- **Join** — merge two LinkedIn flows on page and date so follower gains and page performance sit on the same row before Power BI sees them.
- **Append** — stack several LinkedIn flows, or put LinkedIn rows under other social sources with matching columns, for one social table.
- **Aggregate** — roll daily page rows into weekly or monthly totals per page when the report only needs that grain.
- **Custom SQL** — write a query for anything the visual tools don't cover, such as seconds watched per viewer from the two time columns.

### Step 6. Power BI destination

There is no Power BI sign-in on this step; it only has **Basic settings**. Click **Generate link** and Coupler.io creates the **Integration URL** for this LinkedIn flow. In Power BI Desktop, go to **Get data** → **Web**, paste the Integration URL into the From Web dialog, and click **OK**. The report arrives as a table you can tidy in Power Query. Name the query after the report type, for example "LinkedIn follower gains", because most LinkedIn models end up with three or four of these side by side, each related to a shared page table and a date table.

Coupler.io sets the columns and their types from the LinkedIn data, so there is nothing to map. The Integration URL contains an access token, so share it the way you would share a password. If you regenerate it in the flow, update the Web source in Power BI to the new address.

Starting from a Coupler.io dashboard template instead? Click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown below the button, and click **Load**.

### Step 7. Schedule

Choose how often the flow runs: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Company Page statistics move slowly, so a daily run early in the morning is enough for most teams. Each run swaps the data behind the Integration URL for a complete fresh copy; there is no append option for Power BI. Power BI picks up the new copy on its next refresh: press Refresh in Desktop, or publish the report to the Power BI Service and schedule a refresh on the semantic model an hour after Coupler.io's run. To build long trends, set a wide start date, such as `{{365daysago}}`. Keep in mind this destination always holds the latest copy LinkedIn returns, not a growing archive, so anything LinkedIn stops reporting drops out of Power BI too.

### Bonus: or just ask the AI Agent to build it for you

The Coupler.io AI Agent sits in the right sidebar of the Coupler.io app. Describe the LinkedIn report you have in mind and it fills in the report type, period, grain, and dimension for you:

> *"Get weekly follower gains for our Company Pages over the last 12 months, plus follower demographics by Industry, and load both into Power BI."*

It asks when a choice really needs you and otherwise just sets things up. A few steps stay in your hands:

1. **Connect LinkedIn** — you sign in with your own LinkedIn account.
2. **Select the Company Pages** — you decide which pages are in scope.
3. **Paste the Integration URL into Power BI Desktop** — through Get data → Web.
4. **Set the refresh in Power BI** — in Desktop or as a scheduled refresh in the Power BI Service.

## What data can you export from LinkedIn Public Data to Power BI with Coupler.io?

There are nine current report types. Each flow returns one, so a full LinkedIn model usually runs three or four flows. Coupler.io's Join can combine them on page and date before they land, or you can load each as its own table and relate them in Power BI's model view instead.

- **Page: lifetime overview** — all-time totals for each Company Page in a single row, the right size for a KPI card at the top of a report.
- **Page: performance insights** — how each page does over time, by day, week, or month. The table behind most trend lines in a LinkedIn report.
- **Page: cumulative posts performance** — the combined results of everything a page published in the chosen period, which tells you whether the content output as a whole is moving the page.
- **Page: follower gains trend** — new followers per day, week, or month for each page, the plainest measure of growth.
- **Page: follower demographics insights** — the people who follow the page, split by one of Association type, Country, Geo (market area), Job function, Industry, Seniority, or Company size.
- **Page: audience demographics insights** — the wider audience the page reached in the period, split by Seniority, Country, Industry, Company size, Job function, or Geo (market area). Compare it with the follower table to see who you reach versus who stays.
- **Post: individual posts lifetime performance** — one row per post with its lifetime results, the table for ranking what was published.
- **Post: video performance** — Views, Viewers, Time viewed (ms), and Time viewed for views (ms) for page videos, by day or week.
- **Post: social reactions & comments count** — reaction and comment counts per post, a direct read on which updates got people talking.

## Why export LinkedIn Public Data to Power BI — real life cases

What follows are Power BI reports that comms, employer branding, and agency teams build once the LinkedIn tables refresh themselves.

- **Page growth scorecard for the leadership app**

  Put **Page: lifetime overview** in KPI cards and **Page: follower gains trend** in a line chart, publish the report as a Power BI app, and leadership gets one place to check page growth. An email subscription on the report page sends the monthly snapshot to inboxes the morning after Coupler.io's run, so nobody pastes screenshots into slides.

- **Finding which slice of the audience actually grew**

  Load **Page: follower demographics insights** split by Seniority or Industry and point a decomposition tree at the follower count. Managers can split by page, then by industry, then by seniority, and see where new followers really came from rather than guessing from the headline number.

- **One report, many demographic lenses**

  Run a flow per **Dimension** you care about (Country, Job function, Company size) and Append them in Coupler.io with a column naming the dimension. In Power BI, a field parameter lets the reader switch the axis of a single bar chart between those cuts, so one page replaces four nearly identical ones.

- **Reach versus following, market by market**

  Relate **Page: audience demographics insights** by Geo (market area) to the follower demographics table by country and draw both on a filled map with a slicer toggling between them. Regions with high reach but few followers stand out as places where content travels but doesn't convert into a lasting audience.

- **Post leaderboard for the content meeting**

  A matrix built on **Post: individual posts lifetime performance**, joined in Coupler.io to **Post: social reactions & comments count**, ranks every post with conditional formatting on reactions and comments. Add a drill-through page to open one post's figures, and the weekly content review starts from the same table every time.

- **Video watch quality, not just views**

  From **Post: video performance**, write a DAX measure that divides Time viewed for views (ms) by Views to get average watch time per view, and plot it next to Views in a combo chart by week. A video with modest views but long watch time is a clue for the next script, which a raw view count hides.

- **Separate client pages behind one report**

  An agency selects every client's Company Page in **Pages**, loads the result into one model, and applies row-level security on the page name. Each client opens the same published report and only sees their own page, so there's one report to maintain instead of one per account.

- **Posting output against page momentum**

  Put **Page: cumulative posts performance** and **Page: performance insights** on a shared date table and use small multiples, one panel per page. With bookmarks for "last quarter" and "this year", the team can show whether months with heavier posting actually moved page results, page by page.

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
- [Connect Microsoft Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-microsoft-ads-to-power-bi.md)
- [Connect QuickBooks to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-quickbooks-to-power-bi.md)
- [Connect Shopify to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md)
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
