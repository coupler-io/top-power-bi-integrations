---
title: Connect Shopify to Power BI with Coupler.io
slug: connect-shopify-to-power-bi
canonical: https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-shopify-to-power-bi.md
source: Shopify
destination: Power BI
meta_title: Connect Shopify to Power BI with Coupler.io
meta_description: Load Shopify orders, line items, refunds, fulfillments, inventory, and customers into Power BI on a schedule and model margin and stock in one place.
---

![Shopify](https://static.coupler.io/importers/shopify.svg) → ![Power BI](https://static.coupler.io/importers/microsoft_power_bi.svg)

# Connect Shopify to Power BI with Coupler.io

Shopify Analytics works fine until you need something it wasn't built for: margin after refunds by SKU, stock cover by location, or one view across several stores. Power BI has no built-in Shopify connector, so the usual workarounds are order CSV exports that break whenever someone adds a column, or a Power Query script paging through the Admin API and fighting rate limits. Coupler.io handles the store side. It pulls orders, line items, refunds, transactions, fulfillments, inventory, products, and customers through Shopify's bulk API on a schedule, and gives Power BI one URL per dataset to read from.

## How to export Shopify data to Power BI

Pick the dataset, choose the columns, narrow by date, then filter by status. The orders entities have the most settings. Customers, inventory, and products skip several of them, as noted below.

### Step 1. Basic settings

- **Data entity** — what one row means in your table: an order, a line item, a refund transaction, an inventory level, a product variant, and so on. All twelve options are described further down. The wizard opens on Orders, and changing the entity resets the filters and columns to suit the new choice.

### Step 2. Columns

- **Columns** — available for the order-based entities. Pick fields one by one or by whole group (Order, Customer, Billing address, Shipping address, Order totals in shop currency, Order totals in presentment currency, Customer journey, Line items, Refund, Transaction, Fulfillment, and more, depending on the entity). Store name, the record's ID, and Updated At always come through, so multi-store tables and incremental logic still have keys. Customers, Inventory, Products, Products with variants, and Orders with activities return their standard set of columns, so this step doesn't apply to them.

### Step 3. Date filters

All four fields accept a relative date (*days ago*, *this month*, *last month*, *this year*, *last year*) as well as a fixed one, and they follow your browser's time zone. For a trailing year, `{{365daysago}}` works in any of them.

- **Created after date** — only records created after this point: orders by order date, customers by sign-up date, products by the date they were added. Not available for Inventory or Orders with activities.
- **Created before date** — the upper limit on creation date, with the same coverage as above. Leave it empty to include everything up to now.
- **Changed after date** — only records updated after this point. This is the one date filter that also works for Inventory and Orders with activities. Useful for keeping a large order history fast to pull while still catching late refunds and edits.
- **Changed before date** — the upper limit on last-updated date. Not available for Inventory or Orders with activities.

### Step 4. Advanced filters

- **Filter after ID** — start after a specific customer, product, or order ID and skip everything older. Handy for very large stores where only recent IDs matter. Not available for Inventory or Orders with activities.
- **Status** (orders) — Any, Open, Closed, or Cancelled. Applies to every order-based entity except Orders with activities.
- **Financial status** — Any, Authorized, Pending, Paid, Partially paid, Refunded, Voided, Partially refunded, or Unpaid. Shown for Orders, Orders with line items, Orders with shipping lines, and Order fulfillments with line items.
- **Fulfillment status** — Any, Shipped, Partial, or Unshipped. Shown for Orders, Orders with line items, Orders with shipping lines, Orders transactions, Orders refunds transactions, and Orders with refunded items.
- **Status** (products) — Any, Active, Archived, or Draft. Shown for Products and Products with variants.

### Step 5. Blend and transform data

- **Column management** — turn labels like "Order totals in presentment currency" into short names your report users will read, hide address fields you don't need, and set money columns to decimal before they reach Power BI.
- **Join** — attach a cost or supplier sheet to Orders with line items by SKU, or bring refund totals onto the order row, so margin is already calculated when the table loads.
- **Append** — stack several Shopify stores, or Shopify with another sales channel, into one table with matching columns.
- **Aggregate** — roll line items up to daily revenue per product or per vendor when the report never needs individual orders.
- **Custom SQL** — calculate net revenue after refunds, flag first versus repeat orders, or apply any rule that's easier to write as a query.

### Step 6. Power BI destination

You won't need to sign in to Power BI. The destination step has a single sub-step, **Basic settings**, where you create the link Power BI reads from.

1. Click **Generate link** to get the **Integration URL** for this Shopify flow.
2. Open Power BI Desktop, choose **Get data** → **Web**, paste the Integration URL into the From Web dialog, and click **OK**.
3. In Power Query, give the query a name that matches the entity, for example `Shopify Line Items` or `Shopify Inventory`. When you add a second Shopify flow, the names keep the model readable.
4. In model view, relate line items, refunds, and fulfillments to orders through the Order ID column, and mark Order created at (or Processed at) as the date that drives your date table.

Coupler.io sets the column names and types from the Shopify data, including datetimes and numbers, so there's nothing to map. The Integration URL includes an access token: share it only with people who should see your sales data. If you regenerate the link in the flow, point the Web source in Power BI at the new URL.

Started from one of Coupler.io's Shopify dashboard templates? Click **Download template**, open the file in Power BI Desktop, paste the **Integration URL** displayed below the button, and click **Load**.

### Step 7. Schedule

Choose a refresh interval: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. For a store taking orders all day, hourly keeps a sales dashboard close to live, and daily is plenty for inventory forecasts. Each run replaces the data behind the Integration URL with a complete fresh copy. That's useful with Shopify, because orders change after they're placed: a partial refund or a late fulfillment simply shows up in the next copy, and there are no stale rows to remove.

To get new data into the report, click **Refresh** in Power BI Desktop. In the Power BI Service, set the semantic model's scheduled refresh to run shortly after Coupler.io's run. To keep a long history, set Created after date far back, or leave it empty. Power BI holds the latest copy only, so it's the date window, not past runs, that decides how much history you have.

### Bonus: or just ask the AI Agent to build it for you

You can also skip the form. The Coupler.io AI Agent sits in the right sidebar of the app. Tell it what the report is for:

> *"Get Shopify orders with line items created this year, only paid and partially refunded orders, with line item SKU, vendor, quantity, discounted unit price, and order total amount, and load it into Power BI."*

The Agent picks the matching Data entity, chooses the columns, sets the date and status filters, and prepares the Power BI link. It asks when a choice is genuinely yours, such as whether cancelled orders belong in the numbers.

Four things stay with you:

1. **Connect your Shopify store** and approve access.
2. **Pick the store** when you run more than one.
3. **Paste the Integration URL** into Power BI Desktop under Get data → Web.
4. **Turn on scheduled refresh** for the semantic model in the Power BI Service.

## What data can you export from Shopify to Power BI with Coupler.io?

The **Data entity** field offers twelve options, and each flow brings in one of them. A typical Shopify model in Power BI uses three or four: orders as the fact table, line items for product detail, and inventory or customers as dimensions. Coupler.io's Join puts related entities into one table before loading, or you can load them separately and relate them on Order ID in Power BI's model view.

- **Orders** — one row per order, with Order name, Order created at, Financial status, Fulfillment status, Discount codes, Payment gateway names, customer details, billing and shipping addresses, and totals in both shop and presentment currency. The Customer journey group adds First visit UTM source, Last visit UTM campaign, and Days to conversion.
- **Customers** — your customer list, one row per customer, with contact and address details and lifetime spend. Use it as the customer dimension behind order-level facts.
- **Inventory** — stock levels per item and location, the starting point for stock cover and reorder reports. It filters on Changed after date only, so a scheduled run can focus on levels that recently moved.
- **Products** — the product catalog, one row per product, with status (Active, Archived, Draft) available as a filter.
- **Products with variants** — one row per variant rather than per product, so each size, color, or style has its own SKU, price, and stock figure. This is the table to relate to line items and inventory by variant.
- **Orders with line items** — one row per item in each order: Line item SKU, Line item name, Line item vendor, Line item quantity, Line item current quantity, discounted unit prices, and line totals, with the parent order's fields alongside. This is the main table for product-level revenue.
- **Orders with activities** — the sales agreements behind each order: the original sale plus later edits, returns, and refunds in sequence, for when you need to see how an order changed after checkout.
- **Orders transactions** — every payment event per order: Kind, Status, Gateway, Amount, Fees, Settlement currency, and Shopify Payments details. Use it to reconcile payouts against sales.
- **Orders refunds transactions** — refund records with Refund created at, Refund note, Refund total amount (per refund), and the transactions that paid each refund out.
- **Orders with refunded items** — one row per refunded line: Refunded quantity, Restock type, Restocked, the refunded product variant, and refund line totals. This tells you which products come back and whether they went back on the shelf.
- **Orders with shipping lines** — the shipping methods charged per order: Shipping line title, Shipping line code, Carrier identifier, discounted shipping amounts, and shipping tax lines.
- **Order fulfillments with line items** — each fulfillment and the items it contained: Fulfillment created at, Fulfillment delivered at, Fulfillment estimated delivery at, Fulfillment display status, Tracking info, and Fulfillment location name.

Coupler.io also offers 21 **dataset templates** for Shopify. They're preconfigured flows with the entities, columns, and transformations already set up for a common question, and you'll find them by name in the app. Each one can feed Power BI like any other flow. Three of them also use Google Analytics 4 and need a GA4 connection next to your store.

- **Shopify Returning customer rate** — the share of orders from repeat buyers over time, with new and returning customers side by side at daily, weekly, and monthly grain.
- **Shopify inventory monthly stockout forecast** — current stock against the monthly sales rate, giving the number of months each item is likely to last.
- **Shopify inventory weekly stockout forecast** — the same stock-versus-demand calculation at weekly pace, for items that sell fast enough to run out within a month.
- **Shopify inventory daily stockout forecast** — stock cover measured against daily sales, for best sellers and short-shelf-life products where a few days matter.
- **Shopify Sales by product variant** — revenue and units per variant, so you can see which sizes, colors, or styles carry a product.
- **Shopify Sales by sources** — orders and revenue by sales channel or source over time.
- **Shopify top customers** — buyers ranked by what they've spent and how often they order, for loyalty and VIP lists.
- **Shopify sales by order fulfillment status** — sales broken out into shipped, partly shipped, and unshipped, to spot a fulfillment backlog early.
- **Shopify sales by order financial status** — sales split by payment state, so pending and authorized amounts don't get counted as collected revenue.
- **Shopify Sales by billing location** — revenue by billing country over time, for comparing markets.
- **Shopify Total customers & Avg order value** — two headline trends, how many people buy and how much they spend per order, with the individual orders underneath.
- **Shopify Top selling products** — products ranked by sales across daily, weekly, and monthly views.
- **Shopify Sales by new vs returning customers** — how much of each period's revenue came from people buying for the first time and how much from people who had bought before.
- **Shopify Line Items with Shipping and Taxes** — every line item with its share of shipping and tax, for landed-cost and tax reporting.
- **Shopify Total sales by line item** — line-level sales totals from a single flow, without building the join yourself.
- **Shopify Total sales by order** — one row per order with customer and shipping details, for a quick view of current order status.
- **Shopify Orders with COGS** — order revenue next to cost of goods, grouped by order and date, so gross margin shows up per day.
- **Shopify Line items with COGS** — line items with their cost, taxes, and profit per item.
- **Shopify and GA4 monthly performance** — also uses Google Analytics 4. Monthly store sales combined with GA4 site traffic and engagement, to compare long-term trends from visit to order.
- **Shopify and GA4 weekly performance** — also uses Google Analytics 4. The same blend at weekly grain, for checking campaigns and promotions against sales.
- **Shopify and GA4 daily performance** — also uses Google Analytics 4. Daily sessions next to daily orders, for seeing how changes in traffic show up in revenue.

## Why export Shopify to Power BI — real life cases

These are the Shopify reports that are hard to get from the admin and straightforward to build in Power BI once the tables refresh on their own:

- **Contribution margin by SKU after refunds**

  Join Orders with line items to a cost sheet on Line item SKU in Coupler.io, then load Orders with refunded items alongside. DAX measures subtract Refunded quantity times unit cost and discounted line totals to give net margin per SKU. A matrix with Line item vendor on rows and month on columns, with conditional formatting on margin percent, shows which suppliers' products lose money after returns.

- **Stockout early warning**

  Start from the Shopify inventory weekly stockout forecast dataset template, or relate Inventory to Products with variants yourself. A what-if parameter for supplier lead time in weeks feeds a measure that flags variants whose stock cover is shorter than the lead time. KPI cards count the at-risk SKUs, and a drill-through page per variant shows its sales trend so buyers can confirm before ordering.

- **Fulfillment speed by warehouse**

  Order fulfillments with line items gives you Order created at, Fulfillment created at, Fulfillment delivered at, and Fulfillment location name. A DAX measure for hours from order to dispatch, shown in a line and clustered column chart by Fulfillment location name, makes a slow warehouse obvious, and a data alert on the dashboard tile warns ops when average dispatch time goes past 48 hours.

- **Payout reconciliation for finance**

  Orders transactions lists Kind, Gateway, Amount, Fees, and Settlement currency for every payment. A DAX measure subtracts Fees from captured Amount, and finance builds a paginated report of net settlements per Gateway per day, exported to PDF at month end, so the figures match what actually landed in the bank, not just gross sales.

- **Repeat purchase behavior**

  Customer order index on Orders separates first orders from later ones. A ribbon chart of revenue by customer order index over time shows whether repeat buyers make up a growing share, and a slicer on Customer journey fields such as First visit UTM source shows which acquisition channels bring customers who come back.

- **Multi-store view with row-level security**

  Coupler.io's Append combines orders from each store into one table, and Store name is always included. Row-level security on Store name lets each country manager see only their own shop, while the head office sees everything. The whole report is shared as one Power BI app instead of a file per store.

- **Return reasons and restocking**

  Orders with refunded items brings Restock type, Restocked, and the refunded product variant. A decomposition tree splits refunded value by product type, variant, and restock outcome, so merchandising can tell sizing problems (returned and restocked) from damaged goods (refunded but not restocked).

- **Traffic-to-revenue for campaign reviews**

  The Shopify and GA4 daily performance dataset template puts sessions next to orders in one table. In Power BI, a combo chart of sessions and revenue with a slicer on campaign shows whether a promotion brought in buyers or just visitors. A weekly email subscription sends the page to the marketing lead every Monday.

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
- [Connect TikTok Ads to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-ads-to-power-bi.md)
- [Connect TikTok Organic to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-tiktok-organic-to-power-bi.md)
- [Connect WooCommerce to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-woocommerce-to-power-bi.md)
- [Connect Xero to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-xero-to-power-bi.md)
- [Connect YouTube Analytics to Power BI](https://github.com/coupler-io/top-power-bi-integrations/blob/main/connect-youtube-analytics-to-power-bi.md)
