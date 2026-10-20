---
title: Connect Instagram Insights to Power BI with Coupler.io
slug: connect-instagram-insights-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-insights-to-power-bi.md
source: Instagram Insights
destination: Power BI
meta_title: Connect Instagram Insights to Power BI with Coupler.io
meta_description: Get Instagram profile reach, follower demographics, post, story, and comment insights into Power BI on a schedule and keep snapshots of each.
---

![Instagram Insights](https://static.coupler.io/importers/instagram-ads.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Instagram Insights to Power BI with Coupler.io

Instagram's own analytics are designed for the phone: a few weeks of history, story numbers that disappear a day after the story ends, and no built-in Power BI connector. Getting the data out yourself means a Business account linked to a Facebook Page, Graph API tokens, and a separate call for each metric, profile, and media item. Coupler.io handles that part. It fetches profile, post, story, and audience insights on your schedule and serves them at a URL Power BI reads like any web source, so the Instagram report updates without anyone opening the app.

## How to export Instagram Insights data to Power BI

Coupler.io covers profile performance and reach, follower and audience demographics, post totals and comments, story results, and profile interactions. You connect the Instagram professional account once, through the Facebook login it's linked to, and then complete the steps below. Several fields only appear for particular report types, as noted.

### Step 1. Basic settings

- **Profiles** — choose the Instagram professional accounts to include. Selecting more than one keeps brand or regional accounts in the same table.
- **Report type** — what each row represents: a profile day, a post, a story, an audience segment, or a comment. All 15 report types are covered below.

### Step 2. Report period

- **Start date** — for the date-based profile reports: **Profile: performance insights**, **Profile: daily reach**, **Profile: performance by media type**, **Profile: performance by follow type**, **Profile: links taps by contact button type**, and **Profile: reach by media product type and follow type**. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the range updates itself.
- **End date** — the same six reports. It defaults to yesterday, which is the last complete day Instagram reports on.
- **Timeframe** — for **Profile: new audience demography insights** only. Choose **This month** or **This week**.
- **Published after** — for **Post: performance totals**, **Post: profile interactions breakdown**, and **Post: comment details review**. Limits the export to posts published after this date. If you need a fixed rolling year here, `{{365daysago}}` works.
- **Published before** — the matching upper limit for the same three post reports.
- **Split by** — for **Profile: performance insights** and **Profile: performance by media type**. Pick **Totals only**, **Daily**, or **Monthly (31st day not included)**.

### Step 3. Metrics and dimensions

- **Metrics** — for **Profile: new audience demography insights**. Choose **Engaged audience demographics**, **Reached audience demographics**, or both.
- **Dimension** — for **Profile: new audience demography insights** and **Profile: followers demography insights**. Group the audience by **Age**, **Gender**, **Age, Gender**, **City**, or **Country**.

### Step 4. Advanced settings

- **Show promoted posts ad ids in separate rows** — for **Post: performance totals**. When it's on (the default), a post that was boosted gets a separate row for each ad ID, which makes matching against ad data straightforward.
- **Data behavior** — for the account overview, demography, post, comment, and story reports. **Latest data** keeps current values only, while **Track changes over time** saves a daily snapshot so you can trend numbers that Instagram otherwise only reports as a current total.

### Step 5. Blend and transform data

- **Column management** — keep the columns your social team actually reviews, give the API field names readable labels, and set timestamps to date-time so Power BI can group them into hours and days.
- **Join** — match **Post: performance totals** to Instagram Ads or Facebook Ads results on the ad ID, so boosted posts show both organic and paid numbers in one row.
- **Append** — combine several Instagram profiles, or Instagram with Facebook Page and TikTok organic data, into one social table with consistent columns.
- **Aggregate** — group daily reach into weekly or monthly totals, or count comments per post before loading, so the Power BI model stays small.
- **Custom SQL** — for custom logic, such as an engagement rate per post or a ranking within each profile, write the query and Coupler.io loads the result.

### Step 6. Power BI destination

There's no Power BI login to set up in Coupler.io. The step has a single sub-step, **Basic settings**:

1. Click **Generate link** to get the **Integration URL**.
2. In Power BI Desktop, open **Get data** → **Web** (the in-app note says "Get data from Web").
3. Paste the URL and click **OK**.
4. The Instagram report arrives as a table. Give the query a name that says what it holds, such as *IG Daily Reach* or *IG Posts*, since Instagram reports quickly add up to several queries.

Profile-level daily reports and post-level reports have different grains, so keep them as separate tables. Mark a date table, relate it to the date column of the daily profile tables and to the post publish date, and use a small Profiles table so one slicer filters everything. Coupler.io's Join can attach the comment details to each post before load, which saves you one relationship in the model.

Coupler.io sets column names and types from the Instagram data, so there's nothing to configure by hand. If you started from a Coupler.io dashboard template, click **Download template**, open it in Power BI Desktop, paste the **Integration URL** shown below the button, and click **Load**.

The Integration URL contains an access token and should be kept private. If it's exposed, regenerate it in the flow and change the Power BI source to the new URL.

### Step 7. Schedule

Pick how often Coupler.io runs: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Power BI flows have no append or replace setting, because each run replaces what's behind the Integration URL with a new full copy. Power BI collects it when the report refreshes, via **Refresh** in Power BI Desktop or a scheduled refresh on the semantic model in the Power BI Service, timed after Coupler.io's run.

Story reports only cover the last 24 hours and **Profile: followers daily breakdown (last 30 days)** only covers a month, so for those, schedule at least a daily run and set **Data behavior** to **Track changes over time**. That way the daily snapshots build up in the data, because this destination holds the latest copy, not an archive. For the date-based profile reports, a window starting *this year* gives Power BI a year-to-date trend, within the history Instagram makes available.

### Bonus: or just ask the AI Agent to build it for you

Find the AI Agent in the right sidebar of the Coupler.io app and tell it what you need:

> *"Create an Instagram report of post performance totals for posts published in the last 90 days, keep a daily snapshot so I can see how numbers grow, and get it ready to load into Power BI."*

The Agent sets the report type, the publish dates, and the data behavior, and asks before deciding anything that could go either way. You handle these yourself:

1. **Authorise Instagram** — through the Facebook login your professional account is linked to.
2. **Choose the profiles** — you decide which accounts are included.
3. **Load the Integration URL in Power BI** — Power BI Desktop, **Get data** → **Web**.
4. **Schedule the refresh** — in the Power BI Service for the published report.

## What data can you export from Instagram Insights to Power BI with Coupler.io?

There are 15 report types. Each flow pulls one, so most Instagram reports in Power BI combine a daily profile flow, a posts flow, and a demographics flow. Coupler.io's Join and Append can merge flows into one table before load, or you can keep them separate and relate them in Power BI's model view.

- **Post: performance totals** — one row per post with its accumulated results, plus separate rows for promoted versions if you want them. The core table for any content report.
- **Profile: account overview** — the profile's current headline figures in one row per account. With snapshots enabled, it becomes a growth history.
- **Profile: performance insights** — profile-level results for the date range, as totals or split daily or monthly.
- **Profile: followers daily breakdown (last 30 days)** — follower changes day by day over the past month.
- **Profile: followers demography insights** — who follows you, by age, gender, city, or country.
- **Post: comment details review** — the comments on your posts, ready for community management and moderation reporting.
- **Post: profile interactions breakdown** — the profile visits, follows, and other actions each post led to.
- **Story: performance totals (last 24 hours)** — results for stories from the past day, before they expire from Instagram.
- **Story: navigation by action type (last 24 hours)** — how viewers moved through your stories: forward, back, to the next account, or out.
- **Story: profile interactions breakdown (last 24 hours)** — what viewers did on your profile after watching a story.
- **Profile: daily reach** — how many accounts saw your content each day, the main metric for awareness trends.
- **Profile: performance by media type** — profile results split by posts, reels, and stories.
- **Profile: performance by follow type** — results split between followers and non-followers, showing how far content reaches beyond your audience.
- **Profile: links taps by contact button type** — taps on the email, call, directions, and other contact buttons on your profile.
- **Profile: reach by media product type and follow type** — reach by format and by follower status together, to see which format brings in new viewers.

## Why export Instagram Insights to Power BI — real life cases

Here are eight Power BI reports that brand, social, and community teams build from Instagram Insights.

- **Daily reach trend with alerts**

  Load **Profile: daily reach** for a window starting *this month* and plot it on a line chart with a seven-day rolling average calculated in DAX. Pin the chart to a Power BI dashboard and add a data alert to the tile so the social lead gets an email when reach falls below the usual range, instead of noticing at the end of the month.

- **Format comparison with field parameters**

  Combine **Profile: performance by media type** and **Profile: reach by media product type and follow type** on one page. A field parameter lets the viewer switch the chart's measure between reach, interactions, and other metrics without separate visuals. Split by follower and non-follower, it shows whether reels bring in new viewers while regular posts mainly reach existing followers.

- **Audience profile: followers versus new reach**

  Load **Profile: followers demography insights** and **Profile: new audience demography insights** with **Reached audience demographics** and **Engaged audience demographics**, using **Age, Gender** and **Country** as dimensions. A back-to-back bar chart shows the age and gender profile, and a filled map shows countries. Comparing the two shows whether new viewers resemble your current followers or a different group.

- **Post leaderboard with drill-through to comments**

  **Post: performance totals** filtered with **Published after** ranks posts in a table with Top N filtering and conditional formatting. Right-clicking a post opens a drill-through page built on **Post: comment details review**, where the team can read what people said. With **Show promoted posts ad ids in separate rows** turned on, Coupler.io's Join can attach the ad spend, so boosted and organic posts are compared fairly.

- **Community management app**

  Put **Post: comment details review** into a Power BI app for the community team, with a text search slicer and a mobile layout for checking on the go. A paginated report version gives the legal or customer care team a printable list of comments for a chosen week.

- **Story funnel that survives the 24-hour limit**

  Schedule **Story: performance totals (last 24 hours)** and **Story: navigation by action type (last 24 hours)** to run daily with **Data behavior** set to **Track changes over time**, so each day's stories stay in the data. A ribbon chart shows how forward taps, back taps, and exits change week to week, and identifies which story sequences keep viewers watching.

- **From content to contact: a decomposition tree**

  Combine **Post: profile interactions breakdown**, **Story: profile interactions breakdown (last 24 hours)**, and **Profile: links taps by contact button type**. A decomposition tree breaks profile actions down by source (post or story) and then by contact button, which shows a local business whether its stories or its posts drive calls and direction requests. A KPI card tracks the total number of contact taps against the monthly goal.

- **Brand portfolio with row-level security**

  Groups with several brand accounts select them all in **Profiles** and pull **Profile: account overview** with **Track changes over time** for daily follower history. Row-level security keyed on profile gives each brand manager their own view in a shared Power BI app, and email subscriptions send every manager a Monday summary of the previous week.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
- [Connect Google Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md)
- [Connect Google Search Console to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-search-console-to-power-bi.md)
- [Connect Instagram Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-ads-to-power-bi.md)
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
