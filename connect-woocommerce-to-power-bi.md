---
title: Connect WooCommerce to Power BI with Coupler.io
slug: connect-woocommerce-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md
source: WooCommerce
destination: Power BI
meta_title: Connect WooCommerce to Power BI with Coupler.io
meta_description: Move WooCommerce orders, line items, products, coupons, reviews, tax rates, and sales reports into Power BI on a schedule, with no plugin or API code.
---

![WooCommerce](https://static.coupler.io/importers/woocommerce.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect WooCommerce to Power BI with Coupler.io

WooCommerce stores everything in your WordPress database, but its analytics screens show only a slice of it, and Power BI has no WooCommerce connector of its own. That leaves three options: export CSVs from the admin, install an export plugin and hope it survives the next update, or call the REST API from Power Query and handle pagination and authentication keys yourself. Coupler.io connects with your store's API keys, reads the entity you choose, and publishes it at a URL Power BI loads like any web source. Orders, catalog data, coupons, reviews, tax setup, and WooCommerce's built-in reports all arrive as tables on a schedule.

## How to export WooCommerce data to Power BI

There are three screens on the WooCommerce side: choose the entity, narrow it down, pick the columns. Which filters appear depends on the entity, as noted next to each field.

### Step 1. Basic settings

- **Data entity** — the WooCommerce dataset this flow exports. There are 23 options, from Orders and Products to tax rates and the seven built-in "Report:" summaries, all described further down. Changing it clears the filters that no longer apply.

### Step 2. Advanced settings

- **After date** — export only records dated after this moment. Applies to Coupons, Products, Product reviews, Orders, and Orders with line items. The picker supports relative dates such as *days ago*, *this month*, *last month*, *this year*, and *last year*, so the window keeps moving without edits. `{{365daysago}}` gives you a rolling year.
- **Before date** — the matching upper limit, for the same five entities. Leave it empty to include everything up to the latest record.
- **Search** — a text filter that returns exact matches only. Type a customer's surname to get their orders, or a product title to find orders that include it. Available for nine entities: Orders, Orders with line items, Customers, Coupons, Products, Product reviews, Product categories, Product tags, and Product shipping classes.
- **Status** — for Orders and Orders with line items only: Any (the default), Pending, Processing, On-hold, Completed, Cancelled, Refunded, Failed, or Trash.
- **Period** — for "Report: Sales report" and "Report: Top sellers report" only: Today (the default), This Week, This Month, Last Month, This Year, or Custom.
- **Start date** — appears when Period is set to Custom and marks the first day of the report.
- **End date** — also appears only with a Custom Period and closes the range.

### Step 3. Columns

- **Columns** — leave it empty to get every column the entity returns, or list the ones you want, one per line, in the order they should appear. A short list keeps wide entities like Orders easier to work with in Power Query.

### Step 4. Blend and transform data

- **Column management** — rename WooCommerce's snake_case field names into readable headers, hide the WordPress metadata you'll never chart, and set prices and totals to numeric types.
- **Join** — bring Product categories onto Orders with line items, or Countries onto Orders, so names replace IDs and codes before the data reaches Power BI.
- **Append** — combine several WooCommerce stores, or WooCommerce with another shop platform, into one orders table.
- **Aggregate** — summarize line items into revenue and quantity per product per day, which is all most dashboards need.
- **Custom SQL** — exclude Trash and Failed orders, compute net revenue after refunds, or pull a value out of a nested field with a query.

### Step 5. Power BI destination

Coupler.io never asks for Power BI credentials. The destination step consists of **Basic settings**, where you create a link for Power BI to read.

1. Click **Generate link**. The **Integration URL** for this WooCommerce flow appears.
2. In Power BI Desktop, select **Get data** → **Web**, paste the URL into the From Web dialog, and click **OK**.
3. Rename the query in Power Query to match the entity, for example `Woo Orders`, `Woo Line Items`, or `Woo Tax Rates`. Once you have several WooCommerce flows, names like these keep the field list easy to scan.
4. In model view, connect line items and orders on the order ID, connect products to line items on the product ID, and connect the order creation date to your date table.

Coupler.io decides the columns and their types based on what WooCommerce returns, so there's no mapping screen. The Integration URL includes an access token: share it the way you'd share your WooCommerce API keys. If you regenerate it in the flow, update the Web source in Power BI to the new URL.

If you started from a Coupler.io dashboard template, click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** displayed under the button, and click **Load**.

### Step 6. Schedule

Set how often Coupler.io reads your store: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. A busy store usually refreshes orders hourly and reference data like Tax rates or Shipping zones weekly, since those rarely change. Each run replaces the copy behind the Integration URL with a full fresh export. An order that moves from Processing to Completed, or gets refunded, simply has its new status in the next copy, with no leftover rows.

Power BI sees the new copy when the report refreshes: click **Refresh** in Power BI Desktop, or schedule a refresh of the semantic model in the Power BI Service for a few minutes after Coupler.io finishes. For long-term trends, use a wide After date or leave it empty. The Integration URL holds only the most recent export, so your history is whatever the filters let through.

### Bonus: or just ask the AI Agent to build it for you

Prefer to describe the result instead of filling in the fields? Open the Coupler.io AI Agent in the right sidebar of the app and write something like:

> *"Export WooCommerce orders with line items from this year, only Processing and Completed, keep order ID, date, status, product name, quantity, and line total, and load it into Power BI."*

The Agent picks the entity, applies the date and status filters, sets the columns, and prepares the Power BI link. If your request fits more than one entity, for example orders versus a summary report, it asks which you mean.

Your part of the setup:

1. **Connect WooCommerce** with your store URL and API keys.
2. **Select the store** if you connected more than one.
3. **Paste the Integration URL** into Power BI Desktop through Get data → Web.
4. **Schedule the semantic model refresh** in the Power BI Service.

## What data can you export from WooCommerce to Power BI with Coupler.io?

The **Data entity** list has 23 options: transactional data, catalog and customer records, store configuration, and seven ready-made summary reports. One flow loads one entity. Coupler.io's Join merges related entities, such as orders with countries or line items with categories, before they land, or you can load each as its own table and relate them in Power BI's model view.

- **Continents** — continent codes and names with the countries in each, for grouping sales into regions.
- **Countries** — country codes, names, and their states or provinces. Joins to billing and shipping codes on orders so maps show readable names.
- **Coupons** — every discount code with its type, amount, usage count and limits, expiry date, and product restrictions.
- **Currencies** — the currencies your store supports, with codes and symbols, for multi-currency reporting.
- **Customers** — registered customers with contact details, billing and shipping addresses, and registration date.
- **Orders** — one row per order: status, dates, totals, discount and shipping amounts, tax, payment method, and billing and shipping addresses.
- **Orders with line items** — one row per product in each order, with quantity, price, and line total, plus the parent order's details. The table to use for revenue by product.
- **Product attributes** — the global attributes like size, color, or material that define your variations.
- **Product categories** — the category tree with names, parent categories, and product counts.
- **Product reviews** — customer reviews with rating, review text, reviewer, product, verification flag, and date.
- **Product shipping classes** — the shipping classes products belong to, for linking catalog items to shipping cost rules.
- **Product tags** — tags with product counts, useful as a flexible grouping field in slicers.
- **Products** — the catalog: name, SKU, type, status, regular and sale prices, stock status and quantity, categories, and tags.
- **Report: Coupon totals** — coupon counts grouped by coupon type.
- **Report: Customer totals** — customer counts split into paying and non-paying.
- **Report: Order totals** — order counts per status, for a quick pipeline snapshot.
- **Report: Product totals** — product counts by product type.
- **Report: Review totals** — review counts per star rating.
- **Report: Sales report** — WooCommerce's sales summary for the chosen Period: gross and net sales, orders, items sold, tax, shipping, refunds, and discounts.
- **Report: Top sellers report** — your best-selling products for the chosen Period, ranked by quantity.
- **Shipping zones** — the zones your store ships to, in the order WooCommerce matches them.
- **Tax classes** — standard and custom tax classes like reduced or zero rate.
- **Tax rates** — every configured rate with country, state, postcode, city, rate, priority, and whether it applies to shipping.

## Why export WooCommerce to Power BI — real life cases

WooCommerce owners mostly use Power BI for the questions the admin can't answer in one screen. Eight common ones:

- **Order pipeline that ops checks every morning**

  Load Orders with Status set to Any and use a stacked bar of order count by status per day, with slicers for payment method and shipping country. Pin the On-hold and Processing counts to a dashboard and set a data alert, so someone gets an email when On-hold orders pile up past a threshold instead of finding out from customer complaints.

- **Where gross sales go**

  Report: Sales report with Period set to This Month gives gross sales, discounts, refunds, shipping, and tax for the month. A waterfall chart steps from gross sales down to net sales, so it's clear whether coupons or refunds took the bigger share. Switch the flow to Last Month and the same visual becomes the month-end summary.

- **Category mix in a treemap**

  Coupler.io's Join attaches Product categories to Orders with line items before load. A treemap sized by line total and grouped by parent category shows which parts of the catalog bring in revenue, and a report page tooltip on each tile shows that category's top products and units sold when you hover.

- **Did the coupon pay for itself?**

  Relate Coupons to Orders through the coupon code, then compare discount given with order value per code. A scatter chart with usage count on one axis and average order value on the other separates codes that drive larger baskets from codes that just discount orders people would have placed anyway. Expiry dates from Coupons show which codes are still active.

- **Sales by region**

  Join Countries and Continents onto Orders in Coupler.io so codes become country and state names. A filled map of net sales by state, with a continent slicer, shows where demand is growing. Small multiples of monthly orders by country let you compare markets on the same scale.

- **Product reviews next to sales**

  Product reviews gives rating and date per product. Relate it to Products and Orders with line items, and a matrix shows average rating, review count, and units sold side by side. A low-rated product that keeps selling well needs a better listing, and a well-rated product with low sales needs more visibility.

- **Tax setup audit**

  Load Tax rates, Tax classes, and Shipping zones as reference tables, then relate tax collected on Orders to the rate that should apply by country and state. A paginated report lists any jurisdiction where collected tax doesn't match the configured rate, which your accountant can review before each filing.

- **Customer value tiers**

  From Customers and Orders, DAX calculated columns give each customer an order count, lifetime revenue, and days since last order. Grouping these into tiers gives segments like "repeat, recent" and "one-time, lapsed". A gauge compares paying with non-paying customers using Report: Customer totals, and the report is shared with the retention team through a Power BI app.

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
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
