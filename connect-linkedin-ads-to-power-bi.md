---
title: Connect LinkedIn Ads to Power BI with Coupler.io
slug: connect-linkedin-ads-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-linkedin-ads-to-power-bi.md
source: LinkedIn Ads
destination: Power BI
meta_title: Connect LinkedIn Ads to Power BI with Coupler.io
meta_description: Load LinkedIn Ads analytics by company, job title, and seniority, plus sponsored leads, campaigns, and creatives into Power BI for B2B reporting.
---

![LinkedIn Ads](https://static.coupler.io/importers/linkedin.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect LinkedIn Ads to Power BI with Coupler.io

LinkedIn's Campaign Manager is good at showing one campaign at a time and much less useful when you want to see results by seniority across every account for the last two quarters. Power BI doesn't have a native LinkedIn Ads connector. The usual alternatives are demographic CSV exports pulled by hand, or Power Query calls to the Marketing API with its OAuth handshake and the 18-metric cap on every analytics request. Coupler.io handles the API side and gives Power BI a single URL to read, so B2B marketers can slice spend by company, job function, and industry in their own model.

## How to export LinkedIn Ads data to Power BI

The source covers ad analytics (by one dimension, several, or by member demographics), sponsored lead form submissions, and the account's building blocks: users, campaign groups, campaigns, creatives, video ads, shares, and conversion definitions. Connect your LinkedIn profile that has access to Campaign Manager, then work through these screens.

### Step 1. Basic settings

- **Ad accounts** — one or more Campaign Manager accounts. If you combine accounts, the rows land in one table, with each row tied to its account.
- **Report type** — the dataset this flow brings in. The 11 choices fall into three reports, one lead source, and seven account objects, all covered below.

### Step 2. Report period

- **Start date** — shown for every report type except **Shares (creative sponsored content)** and **Conversions**. For the analytics reports, it opens the reporting window. For **Sponsored leads**, it's the earliest submission to include. Use the relative picker (*days ago*, *this month*, *last month*, *this year*, *last year*) so the window keeps moving, or `{{365daysago}}` for a rolling year.
- **End date** — shown for the three analytics reports and **Sponsored leads**. Leaving it at *today* keeps each run current.
- **Split data by period** — analytics reports only. Totals only, Daily (the default), Monthly, or Yearly.

### Step 3. Metrics and dimensions

Shown for the three analytics reports. This step has to be set in the wizard by hand.

- **Metrics** — up to 18 per report, which is LinkedIn's limit. The defaults are Impressions, Reach, Clicks, Amount spend in USD, Conversions, and Reactions. Others include Leads, Lead form opens, Engagements, Video completions, Document completions, Conversion value in local currency, and Amount spend in local currency. If you need more than 18, run a second flow with the same dimension and join the two.
- **Dimension** — for **Ad analytics by single dimension** and **Campaign performance by member dimension**. Choose what the rows are grouped by. The single-dimension report offers Account, Campaign, Campaign group, Creative, Placement, Impression device type, Conversion, and the member options. The member report is limited to Member company, Member company size, Member country, Member DMA®, Member industry, Member job function, Member job title, Member region, and Member seniority.
- **Dimensions** — for **Ad analytics by multiple dimensions**. Combine several, for example Campaign with Creative and Campaign objective type, or Placement name with Impression device type.

### Step 4. Advanced settings

- **Advanced filters** — analytics reports only. Key-value pairs that narrow the data by a property, such as one campaign group. This one is also set in the wizard.
- **Lead generation forms** — **Sponsored leads** only. Pick specific forms, or leave it on all forms.

### Step 5. Blend and transform data

- **Column management** — trim the "Viral" metric variants you don't report on, rename `Amount spend in USD` to match your other channels, and set currency columns to decimals.
- **Join** — combine two analytics flows that share the same dimension to get past the 18-metric limit, or add CRM account data to Member company rows.
- **Append** — stack LinkedIn rows under Google Ads and Microsoft Ads data so a B2B paid media table covers search and social together.
- **Aggregate** — total daily campaign rows by campaign group and month for a leadership page.
- **Custom SQL** — bucket Member seniority values into decision-maker tiers, or calculate cost per lead before loading.

### Step 6. Power BI destination

There's nothing to authorize. The step's only sub-step is **Basic settings**. Start in Power BI Desktop with **Get data** → **Web**. In Coupler.io, click **Generate link** to get the **Integration URL**, paste it into Power BI's From Web dialog, and confirm with **OK**. The analytics table opens in Power Query. Give the query a name that includes the dimension, such as `LinkedIn by Member job function`, because B2B models tend to collect several of these flows. Link each one to your date table, and if you load **Campaigns** or **Campaign groups**, relate them on the campaign ID.

Column names and types come from the data itself, so there's no mapping step. The Integration URL contains an access token, so don't post it anywhere public. If you regenerate it in Coupler.io, point the Power BI source to the new link.

If the flow began from a Coupler.io dashboard template, click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** shown below the button, and choose **Load**.

### Step 7. Schedule

Choose an interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. LinkedIn campaigns usually have modest daily volume, so a daily run is enough for analytics. For **Sponsored leads**, an hourly run gets new form fills to sales sooner. Each run replaces the data behind the Integration URL with a complete fresh copy. Power BI has no append or replace setting here. Power BI shows the update after its own refresh, either with Refresh in Desktop or through a scheduled refresh on the semantic model in the Power BI Service, timed to follow Coupler.io's run.

For long-range B2B reporting, set the **Start date** wide enough to cover your sales cycle. This destination keeps only the current copy, not an archive. A history beyond what LinkedIn still returns needs another storage layer.

### Bonus: or just ask the AI Agent to build it for you

The Coupler.io AI Agent is in the right sidebar of the Coupler.io app. Tell it what you want:

> *"Create a LinkedIn Ads flow for campaign performance by member seniority since the start of this year, split monthly, for all my ad accounts, and get it ready to load into Power BI."*

The Agent selects **Campaign performance by member dimension**, sets the dates and the monthly split, and prepares the Power BI destination. It asks about anything the request leaves open.

LinkedIn leaves a few more steps to you than most sources:

1. **Connect LinkedIn** — you complete the sign-in yourself.
2. **Choose the ad accounts** — the Campaign Manager accounts in scope.
3. **Pick metrics, the dimension, and any advanced filters in the wizard** — these fields are set by hand.
4. **Paste the Integration URL into Power BI Desktop** — under Get data → Web, then set the refresh in Desktop or in the Power BI Service.

## What data can you export from LinkedIn Ads to Power BI with Coupler.io?

There are 11 report types, and each flow exports one. B2B teams usually run a few analytics flows with different dimensions, plus the campaign lists. Coupler.io's Join can merge the analytics with campaign details before the data reaches Power BI. Or load each as its own table and connect them in the model view.

- **Ad analytics by single dimension** — LinkedIn metrics grouped by one dimension, from Campaign or Creative to Member company or Member job title. It's the most flexible report.
- **Ad analytics by multiple dimensions** — the same metrics grouped by a combination of delivery dimensions, such as campaign by creative by placement.
- **Campaign performance by member dimension** — campaign results split by a professional attribute of the people reached: company, company size, industry, job function, job title, seniority, country, region, or DMA.
- **Sponsored leads** — lead gen form submissions with answers and timestamps, ready to pass to sales.
- **Ad account users** — the people with access to each ad account and their roles.
- **Campaign groups** — groups with their status, budget, and schedule, often used for quarterly or product-line planning.
- **Campaigns** — campaign settings: objective, format, targeting, bidding, and budget.
- **Creatives** — each creative with its status and the content it references.
- **Video ads** — video creative details for video campaigns.
- **Shares (creative sponsored content)** — the posts used as sponsored content, so engagement numbers can be read alongside the actual copy.
- **Conversions** — the conversion definitions set up in the account, with their type and attribution windows.

## Why export LinkedIn Ads to Power BI — real life cases

B2B teams use these LinkedIn reports to see who they reached, not just how many people.

- **Seniority and job function breakdown with a decomposition tree**

  Run **Campaign performance by member dimension** once with Member seniority and once with Member job function. A decomposition tree splits Amount spend in USD, then Leads, by campaign group, then campaign, then seniority. It shows whether budget is reaching directors and VPs or going to entry-level profiles that will never sign a contract.

- **Account-based marketing coverage**

  Pull **Ad analytics by single dimension** on Member company and relate it in the model to your target account list from the CRM. A table with conditional formatting shows which named accounts saw ads, how many impressions each got, and which got none. It's what an ABM review meeting needs, and it updates every morning.

- **Getting past the 18-metric limit**

  Two flows with the Campaign dimension, one with delivery and cost metrics, one with conversions and video metrics, joined in Coupler.io on campaign and date, give Power BI a single campaign table with 36 measures to work with. Field parameters let viewers choose which metrics a chart shows without crowding the page.

- **Lead alerts posted to Teams**

  Load **Sponsored leads** hourly into a report embedded in the sales team's Microsoft Teams channel. A card showing today's lead count, pinned to a dashboard with a data alert, notifies the SDR lead when submissions arrive so follow-up can happen the same afternoon.

- **Document and video engagement funnels**

  For thought-leadership campaigns, chart Document 25% completions through Document completions, or Video starts through Video completions, as a funnel per creative. Relating **Creatives** and **Shares (creative sponsored content)** to the analytics adds the post copy to a tooltip, so content leads can see which topics people actually read to the end.

- **Regional pipeline view with a filled map**

  Use Member country or Member region as the dimension and color a filled map by cost per lead. Mark a date table and add a SAMEPERIODLASTYEAR measure, and regional managers get this year's lead cost by territory next to last year's in one visual.

- **Budget governance with row-level security**

  Model **Campaign groups** and **Campaigns** as dimension tables and set row-level security by campaign group, so each product or regional team sees only its own spend in the shared report. A separate page built from **Ad account users** lists who has access to each account, which helps during quarterly access reviews.

- **Monthly board pack as a paginated report**

  Set **Split data by period** to Monthly and compare Amount spend in local currency against Conversion value in local currency in a ROAS measure. A paginated report formatted for print gives the CMO a fixed monthly layout of LinkedIn spend, leads, and return by campaign group.

## Other Power BI integrations

Every other source you can land in Power BI with the same no-code setup:

- [Connect Facebook Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-ads-to-power-bi.md)
- [Connect Facebook Page Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-facebook-page-insights-to-power-bi.md)
- [Connect Google Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-ads-to-power-bi.md)
- [Connect Google Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-analytics-to-power-bi.md)
- [Connect Google Search Console to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-google-search-console-to-power-bi.md)
- [Connect Instagram Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-ads-to-power-bi.md)
- [Connect Instagram Insights to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-instagram-insights-to-power-bi.md)
- [Connect LinkedIn Public Data to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-linkedin-public-data-to-power-bi.md)
- [Connect Microsoft Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-microsoft-ads-to-power-bi.md)
- [Connect QuickBooks to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-quickbooks-to-power-bi.md)
- [Connect Shopify to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md)
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
