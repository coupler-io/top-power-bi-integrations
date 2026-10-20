---
title: Connect Google Analytics to Power BI with Coupler.io
slug: connect-google-analytics-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md
source: Google Analytics 4 (GA4)
destination: Power BI
meta_title: Connect Google Analytics to Power BI with Coupler.io
meta_description: Send GA4 sessions, channels, landing pages, key events, and ecommerce revenue to Power BI on a schedule, ready for DAX and time intelligence.
---

![Google Analytics](https://static.coupler.io/importers/google-analytics.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Google Analytics to Power BI with Coupler.io

GA4 is good at answering one question at a time inside its own interface, and much worse at feeding a model you control. Explorations expire, standard reports refuse half the dimension combinations you want, and getting the numbers into Power BI tends to end in a CSV download or a Power Query script calling the Data API with tokens you have to keep alive. Coupler.io takes that plumbing off your plate: it queries your GA4 properties on a schedule, flattens the response into a clean table, and hands Power BI a URL to read it from.

## How to export Google Analytics data to Power BI

The wizard has three GA4 screens: which properties, which dates, which fields. Everything you can report on, from acquisition channels to item revenue, comes out of the third screen, and the categories further down this page show how those fields group together.

### Step 1. Basic settings

- **Properties** — tick the GA4 properties to read from. They're listed by account after you sign in with Google, and you can select a whole account group in one click. Choose several and Coupler.io returns them in one table with the property identified on each row, so a portfolio of sites lands in Power BI as a single query.

### Step 2. Report period

- **Start date** — the first day the report covers. It defaults to 60 days back. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the window slides forward on its own. For a rolling twelve months, type `{{365daysago}}`.
- **End date** — the last day included. It defaults to today, which keeps every refresh current without anyone touching the flow.

### Step 3. Metrics and dimensions

- **Metrics** — up to 10 numeric fields per flow. You start with Views, Total users, New users, Event count, and Key events, then swap in whatever your report needs: Sessions, Engagement rate, Purchase revenue, Ads cost, Return on ad spend, and so on. The list is loaded from your selected properties, so custom metrics show up too.
- **Dimensions** — up to 9 fields to slice those metrics by. Date is preselected; add Session source / medium, Landing page, Country, Device category, Item name, or any other GA4 dimension. Every dimension you add makes the table longer, so pick the grain your visuals will actually use.

GA4's API rejects some metric and dimension pairings. If you need more than 10 metrics, or two families of fields that won't combine, build a second flow and bring them together in the next step.

### Step 4. Blend and transform data

- **Column management** — rename API-style labels into the names your report authors expect, drop the fields nobody charts, reorder columns, and fix data types before Power BI ever sees them.
- **Join** — line up two GA4 flows on Date plus a shared dimension, so a second set of 10 metrics sits beside the first in one table. It also works for attaching ad spend or CRM revenue from another source.
- **Append** — stack properties, or GA4 data with traffic from another analytics tool, under one set of column names.
- **Aggregate** — collapse Date rows to week or month when the report never needs daily detail, which keeps the Power BI model lighter.
- **Custom SQL** — compute ratios, bucket landing pages into site sections, or filter out internal traffic with a query, and Coupler.io loads the result.

### Step 5. Power BI destination

There's no Power BI sign-in here. The destination's only sub-step is **Basic settings**, and it gives you a link rather than asking for credentials.

1. Click **Generate link**. Coupler.io creates the **Integration URL** for this flow.
2. In Power BI Desktop, go to **Get data** → **Web**, paste the Integration URL into the From Web dialog, and click **OK**.
3. Your GA4 table opens in Power Query. Name the query after what it holds, for example `GA4 Acquisition` or `GA4 Items`, so a second flow later doesn't end up called "Query2".
4. Set the Date column to the Date type, then relate it to a date table in model view. That's what lets SAMEPERIODLASTYEAR and the other time intelligence functions work on your sessions and revenue.

Coupler.io defines the columns and types from the data GA4 returns, so there's no column mapping to do. The Integration URL includes an access token, so share it the way you would a password. If you regenerate it in the flow, update the Web source in Power BI to the new address.

Started from a Coupler.io dashboard template instead? Click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown under the button, and click **Load**.

### Step 6. Schedule

Choose how often Coupler.io re-queries GA4: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Daily is usually enough for GA4, because the previous day's data finishes processing on Google's side overnight. Each run replaces what sits behind the Integration URL with a fresh full pull for your date window. Nothing is appended, so there are no duplicate days to clean up.

Power BI picks up the new data when the report refreshes. In Desktop, click **Refresh**. After you publish to the Power BI Service, set a scheduled refresh on the semantic model for a time after Coupler.io's run, for example Coupler.io at 06:00 and Power BI at 06:30. For year-over-year visuals, set the Start date wide enough to cover both years. Power BI keeps only the latest copy that Coupler.io delivers, not an archive of past runs.

### Bonus: or just ask the AI Agent to build it for you

Not sure which of the hundreds of GA4 fields to pick? Open the Coupler.io AI Agent in the right sidebar of the app and describe the report you need:

> *"From my main GA4 property, get sessions, engaged sessions, engagement rate, key events, and purchase revenue by date, session default channel group, and landing page for the last 365 days, and load it into Power BI."*

The Agent knows which metrics and dimensions GA4 accepts together, chooses them, sets the date range, and prepares the Power BI destination. If your request mixes fields GA4 won't combine, it tells you and suggests splitting the request into two flows.

What's still yours to do:

1. **Sign in with Google** so Coupler.io can read your GA4 data.
2. **Choose the properties** the report should cover.
3. **Paste the Integration URL** into Power BI Desktop under Get data → Web.
4. **Schedule the refresh** on the semantic model in the Power BI Service.

## What data can you export from Google Analytics to Power BI with Coupler.io?

GA4 in Coupler.io has no report-type selector. Every flow is built from the same **Metrics** and **Dimensions** fields. The nine categories below are an editorial grouping we put together to make that list easier to scan. They are not options in the app. Each one names real metric and dimension labels you'll find in the wizard. One flow holds up to 10 metrics and 9 dimensions, so most teams run a flow per category. Coupler.io's Join and Append combine those flows before they land, or you can load each as its own table and relate them on Date in Power BI's model view.

- **Traffic acquisition and channels** — where visits come from: Session source / medium, Session default channel group, Session campaign, Session primary channel group, and the First user versions of each for first-touch views, measured with Sessions, Engaged sessions, and Engagement rate. Organic google search clicks, impressions, and average position appear here too when Search Console is linked to the property.
- **Paid media and Google Ads** — Google Ads campaign, Google Ads ad group name, Google Ads keyword text, and Google Ads query, plus the CM360, DV360, and SA360 dimensions, measured with Ads cost, Ads clicks, Ads impressions, Ads cost per click, Cost per key event, and Return on ad spend. This is how ad spend gets into the same table as on-site behavior.
- **Audience and users** — who visits and how often they come back: Total users, New users, Active users, 1-day, 7-day, and 28-day active users, DAU / MAU, and Sessions per user, split by New / returning, Age, Gender, Interests, Audience name, and Language.
- **Geography and technology** — Continent, Country, Region, and City, together with Device category, Browser, Operating system, Screen resolution, and Platform. Use it to find the markets and devices where engagement drops.
- **Pages, content, and engagement** — Page path, Page title, Landing page, Full page URL, and Content group, measured with Views, Views per session, Average session duration, Bounce rate, User engagement, and Scrolled users. Event-level detail (Event name, Link URL, Outbound, File name, Video title, Percent scrolled) shows which elements on a page people actually use.
- **Key events and conversions** — Key events, Session key event rate, User key event rate, Event count, and Event value, filtered with Is key event and Event name, so you can see the conversion actions you marked in GA4 broken down by any dimension above.
- **Ecommerce and items** — Purchase revenue, Transactions, Ecommerce purchases, Add-to-carts, Checkouts, Cart-to-view rate, Purchase-to-view rate, Item revenue, Items viewed, Refund amount, and First time purchasers, by Item name, Item category, Item brand, Item variant, Transaction ID, Order coupon, or Shipping tier. Covers the funnel from product view to purchase.
- **Time and cohorts** — Date, Date + hour (YYYYMMDDHH), Week, Month, Year month, ISO week of the year, Day of week name, First session date, Cohort, and Nth day. These set the grain of every other category and support retention curves.
- **App, monetization, and stability** — Total ad revenue, Publisher ad impressions, Publisher ad clicks, ARPU, Crash-affected users, and Crash-free users rate, by Ad unit, Ad format, Ad source, and App version, for properties that include app data streams.

## Why export Google Analytics to Power BI — real life cases

Here are eight reports teams build once their GA4 tables refresh in Power BI on their own:

- **Channel performance with a real year-over-year line**

  Load Sessions, Engaged sessions, Key events, and Purchase revenue by Date and Session default channel group, relate Date to a proper date table, and write measures with SAMEPERIODLASTYEAR. A combo chart puts this year's revenue in bars and last year's in a line, per channel, which GA4's own comparison view can't keep up across a full year.

- **Landing page triage with a decomposition tree**

  Pull Bounce rate, Engagement rate, Sessions, and Key events by Landing page, Device category, and Session source / medium. A decomposition tree splits a drop in key events step by step, from landing page to device to source, until one cause stands out, such as a mobile layout problem on a single campaign page.

- **Paid spend against on-site outcomes**

  Coupler.io's Join lines up Ads cost, Ads clicks, and Return on ad spend by Google Ads campaign with the Key events and Purchase revenue GA4 attributes to the same campaign. A DAX measure for cost per key event feeds a matrix with conditional formatting, so campaigns spending heavily for few outcomes are highlighted in red without anyone exporting from two tools.

- **Market map for regional teams**

  Sessions, Total users, and Purchase revenue by Country, Region, and City drive a filled map with a slicer for Session default channel group. Add row-level security on Country and each regional manager sees only their own territory when the report is shared through a Power BI app.

- **Ecommerce funnel from item view to purchase**

  Items viewed, Add-to-carts, Checkouts, and Ecommerce purchases by Item category and Item name build a funnel visual, with Cart-to-view rate and Purchase-to-view rate shown on KPI cards. A drill-through page per Item name shows its revenue trend and Item refund amount, so merchandisers can tell a weak product page from a product people keep returning.

- **What makes a session convert**

  Feed the key influencers visual with Session key event rate as the target and Device category, New / returning, Session default channel group, Language, and Day of week name as explanatory fields. It ranks the factors that change conversion odds most, which gives a CRO team a list of tests to run.

- **Content performance with field parameters**

  Content teams want Views one minute and User engagement or Scrolled users the next. A field parameter lets readers switch the metric on a single bar chart of Page title or Content group, and bookmarks save named views like "Blog this month" and "Docs last quarter" for the weekly review.

- **Traffic alerts on a phone**

  Pin a card of yesterday's Sessions and Key events to a Power BI dashboard and set a data alert on the tile, so the on-call marketer gets an email when sessions fall below a threshold, for example after a tracking tag breaks. Design a mobile layout for the report and the same numbers are readable in the Power BI mobile app, or in a Teams channel tab where the web team already works.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
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
