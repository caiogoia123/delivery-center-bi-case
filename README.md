# Delivery Center — Operations BI Dashboard

*[Leia em português](README.pt-br.md)*

A Power BI dashboard built on top of the "Delivery Center" case — a public BI practice case that simulates the operation of a delivery platform connecting stores/restaurants and marketplaces through regional distribution hubs. The dataset is fictional/scrambled and not real client or company data.

The goal was to go beyond just building the report: model the data properly (Power Query + star schema), calculate the requested business metrics in DAX, and **redesign the visual layer** — layout, color, tooltips, filter panel — as its own deliverable.

**[Interactive Figma prototype →](https://www.figma.com/proto/4ybE2H7eK4rPlueljGwlsF/OPERA%C3%87%C3%83O-DO-DELIVERY-CENTER-%E2%80%93-CAIO-PRADO?node-id=1-627&p=f&viewport=-32%2C156%2C0.25&t=SFTmUkSk5FtjkSHh-1&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1)**

**[LinkedIn post →](https://www.linkedin.com/feed/update/urn:li:ugcPost:7484364847732908032/)**

## Stack

`Power BI Desktop` · `Power Query (M)` · `DAX` · `Figma` (visual design)

## Walkthrough

Page by page — not just what each visual shows, but the business finding it surfaces.

### Global filter panel
![Filter panel](screenshots/filtro.png)

A floating layer available from any page via the funnel icon, filtering by **period, channel, store segment and city**. One slicer set reapplied across all 5 pages means switching a cut once updates every KPI, chart, map and table together, instead of duplicating visuals per channel or city.

### Sales overview
![Sales overview](screenshots/visao-geral-vendas.png)
![Sales overview tooltip](screenshots/visao-geral-tooltip.png)

- **368,999** orders, **R$39M** in revenue, **R$105.15** average ticket, **951** active stores — the four numbers any leadership review asks for first.
- Clear weekly seasonality: orders climb from **44k** on Monday to **61k** on Friday, a ~40% swing — the operation clearly pulses with the weekend.
- Heavy marketplace dependency: **90.3%** of orders come through third-party channels vs **9.7%** through the owned channel.

> **Finding in the tooltip:** the average-ticket card hides a trend — **on-time delivery rate dropped from 94.6% to 88.2%** over the 4-month period. No headline KPI shows this; it only surfaces on hover, the kind of signal that gets lost if a report only shows big numbers on the front page.

### Operations & delivery
![Operations and delivery](screenshots/operacao-entrega.png)
![Operations tooltip and drillthrough](screenshots/operacao-entrega-tooltip-drill.png)

- Average delivery time of **156.6 min** against a median of only **42.2 min** — the mean sitting so far above the median already flags outliers pulling the number up.
- Fleet is mostly motorbike (**73.1%**) and freelance (**71.5%**) — the operation leans on informal labor, not a fixed owned fleet.

> **Finding in the concentration chart:** the Pareto-by-hub view shows **Elixir Shopping alone accounts for 47%** of total delivery time among the 12 hubs with the highest total delivery time. Remove that hub and the network-wide average falls from **156.6 to ~94.2 min** — the delivery-time problem isn't spread across the network, it's concentrated in one spot. The tooltip already ships a drillthrough straight to that hub's detail, without leaving the page.

### Quality & finance
![Quality and finance](screenshots/qualidade-financeiro.png)
![Quality and finance tooltip](screenshots/qualidade-financeiro-tooltip.png)

- Cancellation rate of **4.6%** and returns (chargeback proxy) of just **0.115%** — cancellation is the real friction point, returns barely register.
- March stands out with revenue cancellation near **16%**, well above the period average of **9.04%**.

> **Finding in the logistics margin:** the delivery-cost line sits **above** the fee-charged line on **107 of 120 days (89%)** of the period, closing the four months at **-R$355,974** in logistics margin. In other words, the delivery operation loses money most days — not one bad month, the pattern.

### Geographic distribution
![Geographic distribution](screenshots/distribuicao-geografica.png)

- Network concentrated in South/Southeast Brazil: São Paulo, Rio de Janeiro, Curitiba, Porto Alegre, plus a smaller hub in Belo Horizonte.
- São Paulo leads on both sides — **167,590** orders and **460** stores — demand and supply growing together.

> **Finding in the city comparison:** Rio de Janeiro is 2nd in orders (**137,594**) but carries **326** stores — a higher order-to-store ratio than Curitiba or Porto Alegre. That's the kind of read that turns into an expansion question: is RJ's store network thinner than its demand warrants, while Curitiba/Porto Alegre already have more store capacity than they're selling through?

### Order-level detail (drillthrough)
![Order detail](screenshots/detalhamento.png)

The last page is the raw table, order by order — channel, store, hub, quantity, delivery fee and delivery cost side by side.

> **What this page is for:** any KPI from the previous pages can be audited down to the underlying row here — for instance, confirming why a **CANCELED** order still carries a delivery cost (the driver had already been dispatched before the cancellation was logged), the kind of case that explains in practice why the logistics margin closes negative.

**Three findings a plain data dump wouldn't show:**
1. "Bad" delivery time is concentrated in one hub (Elixir Shopping), not spread across the network.
2. The delivery operation loses money on most days of the period — the fee charged doesn't cover the real cost.
3. Demand and store supply don't grow at the same rate across cities, a direct input for expansion decisions.

## Data model

Star schema built from 7 source tables:

- **Fact tables**: `orders` (order lifecycle timestamps, amount, delivery fee/cost), `deliveries` (distance, status), `payments` (amount, fee, method)
- **Dimension tables**: `stores`, `hubs`, `channels`, `drivers`

Relationships link orders to stores/channels, deliveries to drivers, and stores to hubs, enabling analysis from order-level detail up to hub/city rollups.

## KPIs delivered

- Total orders by period, orders by weekday
- Total revenue, average ticket
- Average delivery time, average driver response time
- % of orders delivered on time
- % of cancelled orders, % of returned orders
- Active partner stores/restaurants
- Average delivery cost
- Cities with highest demand
- Most ordered products (quantity)
- Orders by channel type and store segment

## Opening the report

Download **[delivery-center-operation-caio-prado.pbix](delivery-center-operation-caio-prado.pbix)** and open it with **Power BI Desktop** (GitHub cannot preview `.pbix` files inline — use the screenshots or the Figma prototype above for a quick look).
