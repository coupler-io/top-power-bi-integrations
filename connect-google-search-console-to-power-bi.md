---
title: Connect Google Search Console to Power BI with Coupler.io
slug: connect-google-search-console-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-search-console-to-power-bi.md
source: Google Search Console
destination: Power BI
meta_title: Connect Google Search Console to Power BI with Coupler.io
meta_description: Load Search Console search results, Discover, Google News and URL index data into Power BI on a schedule, so SEO reporting keeps itself current.
---

![Google Search Console](https://static.coupler.io/importers/google-search-console.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Google Search Console to Power BI with Coupler.io

Search Console's Performance tab caps exports at 1,000 rows and keeps queries, pages, and countries in separate tabs, which is a poor fit for a Power BI model. Power BI has no native Search Console connector, so teams either paste exports into a workbook each week or write Power Query calls to the Search Console API and deal with Google sign-in and row limits themselves. Coupler.io connects to your Search Console properties, pulls search, Discover, Google News, and index data on a schedule, and gives Power BI one link per flow that serves the freshest copy.

## How to export Google Search Console data to Power BI

Coupler.io covers Google Search performance, performance by search appearance, Discover, Google News, and URL index status. These are the screens and what goes in each field.

### Step 1. Basic settings

- **Sites** — the Search Console properties to pull from. Select as many as you have access to; an agency can cover its whole roster in one flow. **URLs index performance** doesn't use this field.
- **Report type** — which dataset the flow returns, one of five. The following steps show only the fields that apply to your choice.
- **Inspection URLs** — appears only for **URLs index performance**. Enter the URLs to check, one per line.

### Step 2. Report period

Used by every report except **URLs index performance**.

- **Start date** — the start of the analysis window. Relative options (*days ago*, *this month*, *last month*, *this year*, *last year*) move with the calendar, so a weekly report never needs editing. For a trailing year of search data, use `{{365daysago}}`.
- **End date** — the end of the window, *today* by default.

### Step 3. Dimensions

- **Dimensions** — how each row is broken down. **Search results performance** offers Date, Country, Device, Page, and Query; **Discover performance** offers Date, Country, and Page; **Google News performance** offers Date, Country, Device, and Page. Date is preselected, which keeps a trend line possible. **Search results performance by appearance** has no dimension choice.
- **Filters by dimensions** — rules that narrow the rows, built from a dimension, an operator, and an expression. Filter on Country, Device, Page, Query, or Search appearance, even ones you didn't pick above, with Contains, Equals, Does not contain, Does not equal, Including Regex, or Excluding Regex. Not used by the by-appearance report.

### Step 4. Advanced settings

- **Search results type** — which Google surface the figures come from: Web (default), News, Image, or Video, with Discover and Google News added for **Search results performance by appearance**. Only the two search results reports use it.
- **Aggregate data by** — how Google groups the numbers. Auto lets Search Console decide, Page groups by canonical URL, and Property combines everything for the property. **Discover performance** and **Google News performance** offer News showcase panel instead of Property, which needs a filter of Search appearance equals NEWS_SHOWCASE.
- **Data state** — Final (default) returns settled figures that are a few days old but won't change. All includes the latest data, up to about 24 hours old, which may still be revised.

### Step 5. Blend and transform data

- **Column management** — keep the Search Console columns your report uses, rename them for readers outside SEO, and check their types.
- **Join** — link Search Console pages to landing pages from an analytics source so ranking and on-site behaviour share one row.
- **Append** — stack separate flows, such as branded and non-branded queries, or sites pulled one by one, into a single table.
- **Aggregate** — sum clicks and impressions by page or month before the data reaches Power BI.
- **Custom SQL** — compute your own measures, such as impression-weighted position across a page group.

### Step 6. Power BI destination

This step has **Basic settings** only and no Power BI authorization. Click **Generate link** and Coupler.io produces the **Integration URL**. In Power BI Desktop, select **Get data** → **Web**, paste the Integration URL into the From Web dialog, and click **OK**. The Search Console data lands as a table you can shape in Power Query. Call the query something like "GSC queries by page", mark the Date column against your date table, and if you also load an analytics table, relate the two on Page.

Coupler.io sets the columns and types from Search Console's data, so there's nothing to map. Because the Integration URL holds an access token, keep it private like a password; after regenerating it in the flow, repoint Power BI's Web source to the new link.

For flows created from a Coupler.io dashboard template: click **Download template**, open it in Power BI Desktop, paste the **Integration URL** shown under the button, and click **Load**.

### Step 7. Schedule

Pick an interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. With Data state set to Final, a daily run is plenty, since settled figures change once a day at most. Each run replaces what sits behind the Integration URL with a fresh full copy; there's no append choice for Power BI. Power BI reads it on refresh, either with Refresh in Desktop or a scheduled refresh on the semantic model in the Power BI Service, timed to follow Coupler.io's run. Search Console keeps about 16 months of performance data, and this destination holds the latest copy rather than an archive, so a start date beyond that window won't bring back older months.

### Bonus: or just ask the AI Agent to build it for you

Open the Coupler.io AI Agent in the right sidebar of the app and describe the SEO report:

> *"Pull Search results performance for our main site by Date, Page, and Query for the last 365 days, final data only, and load it into Power BI."*

The Agent chooses the report type, dimensions, period, and advanced settings and builds the flow. What stays manual:

1. **Connect Google Search Console** — sign in with your own Google account.
2. **Select the sites** — choose the properties the report should cover.
3. **Set up any Filters by dimensions** — in the wizard, such as excluding branded queries.
4. **Paste the Integration URL into Power BI Desktop and set the refresh** — Get data → Web, then a refresh in Desktop or the Power BI Service.

## What data can you export from Google Search Console to Power BI with Coupler.io?

Five report types are available, one per flow. Coupler.io's Join and Append combine several of them, or a Search Console flow with analytics data, before they land; alternatively, load each as its own table and relate them in Power BI's model view.

- **Search results performance** — clicks, impressions, CTR, and average position from Google Search, by Date, Country, Device, Page, and Query. The core of any SEO report.
- **Search results performance by appearance** — the same metrics split by how results appeared, such as rich results or video, to see whether structured data pays off.
- **Discover performance** — traffic from the Google Discover feed by Date, Country, and Page, for publishers who depend on recommendations.
- **Google News performance** — clicks and impressions inside Google News by Date, Country, Device, and Page, showing which stories got picked up.
- **URLs index performance** — index status for the URLs you list, a check on whether critical pages are still in Google.

## Why export Google Search Console to Power BI — real life cases

Below are the Power BI reports SEO, content, and agency teams build once Search Console data refreshes on its own.

- **Queries close to page one**

  From **Search results performance** with Query and Page, build a scatter chart of average position against impressions and filter it to positions 8 to 20. A slicer for Device narrows it further. The queries with lots of impressions just off page one are the cheapest wins, and the list updates every day.

- **A correct average position**

  Averaging position across rows misleads, because a query with ten impressions counts as much as one with ten thousand. Write a DAX measure that weights position by impressions and use it in a matrix by page group, with conditional formatting on the change since last month.

- **Branded versus non-branded, side by side**

  Run two flows with **Filters by dimensions** on Query (one Contains your brand name, one Does not contain it) and Append them in Coupler.io with a label column. Bookmarks in Power BI flip one report page between the branded, non-branded, and combined views.

- **Search traffic by market**

  Use Country and Device from **Search results performance** on a filled map, with a decomposition tree beside it splitting clicks by country, device, and page. Regional teams can see where mobile underperforms desktop and which pages carry each market.

- **Rankings next to on-site results**

  Join Search Console pages with landing-page data from an analytics source in Coupler.io, or load both and relate them on Page in the model. A combo chart shows clicks as columns and conversions as a line, so content teams see which ranking pages earn results, not only traffic.

- **Did the structured data work?**

  **Search results performance by appearance** shows clicks and CTR for each appearance type. A line chart by month with an X-axis constant line on the release date, plus a card showing CTR before and after, gives developers a clear answer on whether the markup change paid off.

- **Publisher dashboard for Discover and News**

  Load **Discover performance** and **Google News performance** as separate tables on a shared date table and Page dimension. A Power BI app gives editors one place to see which articles were picked up where, with an email subscription sending the morning summary to the newsroom.

- **Alerts on revenue-critical pages**

  List key URLs in **Inspection URLs** for **URLs index performance** and show their status in a table on a Power BI dashboard. Pin a count of pages not indexed as a tile and set a data alert on it, so the SEO lead hears about a deindexed product page the day it happens.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
- [Connect Google Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md)
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
