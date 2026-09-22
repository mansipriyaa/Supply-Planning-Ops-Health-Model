# Supply Planning & Ops Health Model

Two connected planning-and-operations layers for a D2C brand: an **AOP unit-planning
model** that turns a revenue plan into a per-SKU manufacturing ask, and an **operations
health tracker** that measures how well those orders are actually fulfilled and delivered.

## Problem

Planning and operations sit on opposite ends of the same flow and neither could be seen
clearly. On the planning side, a revenue target can't be handed to a factory — they need
*units*, per SKU, per month, which depends on realised selling price, recent run-rate
(unreliable for new SKUs), and a growth uplift. On the operations side, once orders ship
there was no single view of fulfillment speed, dispatch performance, and delivery
failures (RTO/NDR) across facilities, couriers, and zones — so process problems stayed
invisible until they showed up as customer complaints.

## Approach

**Supply planning**
- Built an **AOP unit-requirement model** — editable revenue plan × per-SKU average
  selling price → units required per category and SKU, with a configurable AOP-uplift
  factor for growth planning.
- **Projected new SKUs to a 30-day run rate** rather than a naive lifetime average, so
  recently launched products aren't under-planned.
- **Kept contribution editable** so planners can override the data-driven split with
  commercial judgement and watch units recompute.
- Added **replenishment and forward-supply views** projecting per-SKU depletion and
  reorder timing across upcoming months, exportable to Excel for the supply team.

**Ops health**
- Built a **period-over-period ops tracker** with day/week/month grain and filters for
  channel, courier, and zone (metro/non-metro).
- **Decomposed fulfillment into stages** — order→fulfilment→dispatch→delivered — split by
  facility and by order type (regular / custom / pre-order), to locate exactly where time
  is lost.
- **Tracked delivery-failure metrics** — RTO and NDR split by prepaid vs. COD, plus a
  delivery-speed distribution (<3 / 3–5 / >5 days), against the prior period.

## Tech

- **SQL (BigQuery)** — run-rate/ASP and unit-requirement logic on the planning side;
  stage-wise fulfillment timing, facility/zone splits, and RTO/NDR computation on the ops
  side
- **Python** — renders the interactive plan tables and ops KPI cards as HTML/CSS
- **Hex** — hosts both dashboards: the editable plan with Excel export, and the filtered
  ops WBR
- Daily SKU revenue, order/fulfillment/logistics data, and a master SKU/category sheet

## What you'd see

**Supply planning**
- **Supply AOP Plan** — units required by category and SKU from an editable revenue plan ×
  ASP, with adjustable AOP uplift and category→SKU drill-down, exportable to Excel
- **Replenishment Tracker** — per-SKU depletion and reorder timing from sell-through
- **Forward Supply Plan** — forward unit-requirement projection across upcoming months

**Ops health**
- **Ops Summary** — orders received / delivered / cancelled and stage-wise fulfillment
  timing (order→delivered, same-day fulfilment/dispatch), split by facility and order type
- **Logistics** — delivery-speed distribution and RTO/NDR by prepaid vs. COD, period over
  period
- **Metro / Non-Metro** — the same operational view cut by zone
