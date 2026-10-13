# Report Spec

## Report identity
- Report name: Merchandise Sales Report
- Semantic model: Merchandise Sales Report (local PBIP, import from the Onyx Data workbook)
- Audience: Lee Chatmen's merchandise team and portfolio viewers; mixed, non-technical
- Primary purpose: Answer the ten questions in `Description_Merchandise Sales Analysis.docx` about how the merchandise line is selling
- Delivery target: Local PBIP only (no Fabric publish)

## Semantic model binding
- Entry mode:          Local Model
- Model name:          Merchandise Sales Report
- connectionString:    byPath ../Merchandise Sales Report.SemanticModel

## User decisions and constraints
- Scope: Four pages covering all ten brief questions. Not a challenge entry, so no ZoomCharts visuals and no single-image constraint.
- Page count: 4
- Interactivity: page navigator in the header band of every page; three synced dropdown slicers (Month, Product Category, Market); default cross-highlighting between visuals; no drillthrough or bookmarks in the first build.
- Design direction: Clean modern light.
- Publishing: None.
- Tooling: Power BI Desktop is open on this PBIP; modeling MCP connected to it; `powerbi-report-author` CLI and Node 22 available.
- Model edit permissions: Add measures, calculated columns and a Date table. Existing columns and the Power Query steps are not changed.
- Accessibility: WCAG AA contrast on all text; alt text on every chart; colour never the only carrier of meaning (direct labels and legends).
- Data caveats:
  - **No discount data.** Each of the 13 products has exactly one price for the whole year. Discounts are covered by a proxy: the share of reviews that praise discounts or affordability. Labelled as a proxy on the visual.
  - **No customer ID.** Repeat purchases cannot be measured. Proxy: multi-unit orders (35% of orders have quantity 2 to 5). Labelled as a proxy.
  - `Total Sales` includes shipping: `Total Sales = (Sales Price + Shipping Charges) x Quantity` on every row. Shipping is therefore charged per unit, and `SUM(Shipping Charges)` understates shipping revenue.
  - Shipping fees are fixed per destination (US 0, Canada 25, UK and Ireland 40, France 50, India 70, Australia 100), so shipping "trends" only reflect changes in destination mix.
  - Data runs 5 Nov 2023 to 4 Nov 2024. November 2024 holds 4 days and is excluded from monthly trend charts; November 2023 starts on the 5th.
  - Products have IDs only, no names. Reviews are 29 canned sentences.
  - There is no country column; it is derived from the 25 cities.

## Narrative
- Core story: A year in, the line earns about $856K from 7,394 orders. Clothing carries roughly three quarters of it. International buyers are under a third of orders but close to half of sales, partly because shipping fees are counted in sales. Ratings sit near 3.5 for every product and do not track sales.
- Audience promise: Each page answers a group of the brief's questions in one screen, and says plainly where the data stops.
- Key questions answered:
  1. Overall sales trends: Overview
  2. Best product categories: Overview
  3. Most and least popular products: Products
  4. Location and sales: Overview, Geography and Shipping
  5. Impact of international shipping: Geography and Shipping
  6. Buyer demographics: Buyers
  7. Ratings and reviews vs sales: Products
  8. Shipping charge trends: Geography and Shipping
  9. Discounts and promotions (proxy): Products
  10. Repeat purchases (proxy): Buyers

## Design identity
- Tone: Clean modern light. Soft grey page, white visual containers, one indigo accent, generous spacing, Segoe UI throughout.
- Signature: A KPI strip of large indigo numbers directly under the header on every page, and sentence-style chart titles that say what the chart is for.

## Page plan
1. Overview
   - Archetype: Executive Summary
   - Layout variant: A. KPI strip, wide trend plus category panel, two supporting panels. Chosen because there is one headline measure with a 12-point monthly series.
   - Purpose: How much is the line selling, when, and where does it come from?
   - Visuals: 5 KPI cards; monthly sales line; sales by category bar; orders by weekday column; sales by country bar
   - Fields/measures: Total Sales, Orders, Units Sold, Avg Order Value, Avg Rating; Date[Month], Date[Weekday]; Data[Product Category]; Data[Country]
   - Slicers/interactions: Month, Category, Market
2. Products
   - Archetype: Comparative Benchmark
   - Layout variant: B. Tall ranking on the left, evidence panels on the right. Chosen because 13 products fit in one ranked bar chart, so top and bottom are visible together.
   - Purpose: Which products sell most and least, and do ratings, reviews or discount praise explain it?
   - Visuals: 4 KPI cards; product ranking bar coloured by category; rating vs sales scatter; review theme by sentiment stacked bar; discount/affordability praise by product bar (proxy)
   - Fields/measures: Total Sales, Units Sold, Avg Rating, Positive Review %, Negative Review %, Price Praise %; Data[Product ID], Data[Product Category], Data[Review Theme], Data[Sentiment]
   - Slicers/interactions: Month, Category, Market
3. Geography and Shipping
   - Archetype: Analytical Canvas
   - Layout variant: A. Map and ranking side by side over two shipping panels. Chosen because 25 cities carry coordinates and a domestic/international split.
   - Purpose: Where do sales come from, and what does international shipping add?
   - Visuals: 4 KPI cards; city bubble map; city ranking bar coloured by market; monthly shipping revenue with international order share; country table
   - Fields/measures: Total Sales, Orders, International Orders %, International Sales %, Shipping Revenue, Shipping % of International Sales, Product Revenue, Avg Order Value, Avg Rating; Data[Order Location], Data[Latitude], Data[Longitude], Data[Country], Data[Market]
   - Slicers/interactions: Month, Category, Market
4. Buyers
   - Archetype: Analytical Canvas
   - Layout variant: C. Four equal panels under the KPI strip. Chosen because age, gender, category mix and basket size are equally weighted questions.
   - Purpose: Who is buying, and how much do they buy at once?
   - Visuals: 4 KPI cards; orders by age and gender stacked column; category mix by age group 100% stacked bar; orders by quantity column (proxy); multi-unit order rate by product bar (proxy)
   - Fields/measures: Orders, Total Sales, Avg Buyer Age, Male Buyer %, Units per Order, Multi-Unit Order %; Data[Buyer Age], Data[Age Group], Data[Buyer Gender], Data[Product Category], Data[Quantity], Data[Product ID]
   - Slicers/interactions: Month, Category, Market

## Design system summary
- Theme name + base palette: "Merch Light", adapted from the skill's base theme. Page #F5F6F8, containers #FFFFFF, text #1F2937, secondary text #4B5563, accent indigo #4F46E5.
- Colour semantics: Total Sales indigo #4F46E5; Orders slate #334155; Shipping Revenue orange #C2410C; Avg Rating amber #B45309. Category: Clothing #4F46E5, Ornaments #0891B2, Other #64748B. Market: Domestic #64748B, International #C2410C. Gender: Male #0F766E, Female #9333EA. Sentiment: Positive #15803D, Neutral #94A3B8, Negative #B91C1C.
- Typography pairing: Segoe UI Semibold for the page title (24pt) and KPI values (32pt); Segoe UI for chart titles (14pt), labels and axes (11 to 12pt).
- Layout pattern: 1920 x 1080, 12 x 12 grid, 32px margin, 24px gutter, 8px snap; header band on row 1, KPI strip on rows 2 to 3.
- Accessibility commitments: AA contrast, alt text per chart, data labels on bars, legends on every multi-series chart, tab order following reading order.

## Model requirements
- Existing measures: none (the model has only raw columns).
- New table: `Date` (calculated, 1 Nov 2023 to 30 Nov 2024, marked as date table) with `Date`, `Month` (e.g. "Nov 2023", sorted by `Month Start`), `Month Start`, `Weekday` (sorted by `Weekday No`), `Weekday No`, `Is Complete Month`. Relationship `Data[Order Date]` to `Date[Date]`, many to one, single direction.
- New calculated columns on `Data`:
  - `Country`: United States, United Kingdom, Ireland, Canada, France, India, Australia, mapped from `Order Location`
  - `Market`: Domestic / International, from `International Shipping`
  - `Age Group`: 18-20, 21-25, 26-30, 31-35
  - `Sentiment`: Positive (rating 4-5), Neutral (3), Negative (1-2), with a sort column
  - `Review Theme`: Delivery, Quality, Design, Price and Discounts, General, mapped from the 29 review sentences
  - `Order Size`: Single unit / Multi-unit
- New measures (in `Data`, display folders by topic):
  - `Total Sales` = SUM(Data[Total Sales]) ($#,0)
  - `Product Revenue` = SUMX(Data, Data[Sales Price] * Data[Quantity])
  - `Shipping Revenue` = SUMX(Data, Data[Shipping Charges] * Data[Quantity])
  - `Orders` = COUNTROWS(Data); `Units Sold` = SUM(Data[Quantity])
  - `Avg Order Value` = DIVIDE([Total Sales], [Orders]); `Units per Order` = DIVIDE([Units Sold], [Orders])
  - `Avg Rating` = AVERAGE(Data[Rating]) (0.00)
  - `Positive Review %`, `Negative Review %` (share of orders by Sentiment)
  - `Price Praise %` = share of orders whose review is "Discount deals was greatly appreciated." or "The product was affordable here compared to other brands."
  - `International Orders %`, `International Sales %`, `Shipping % of International Sales`
  - `Avg Buyer Age`, `Male Buyer %`, `Multi-Unit Order %`
- Housekeeping: set `summarizeBy: none` on Order ID, Buyer Age, Rating, Latitude, Longitude, Sales Price; set data categories on Latitude, Longitude, Order Location (City), Country; hide the `Dictionary` table from report view.

## Canonical design contract

```yaml
Design Brief:
  generated_by: powerbi-report-cli
  contract_version: 1
  mode: greenfield
  design_identity:
    tone: "Clean modern light: soft grey page (#F5F6F8), white containers, single indigo accent, Segoe UI, medium density"
    signature: "Indigo KPI strip under the header on every page plus sentence-style chart titles"
  archetype: Executive
  navigation_model: page_navigator
  color_map:
    - measure: Data[Total Sales]
      color: "#4F46E5"
      tint: "#E0E7FF"
    - measure: Data[Orders]
      color: "#334155"
      tint: "#E2E8F0"
    - measure: Data[Shipping Revenue]
      color: "#C2410C"
      tint: "#FFEDD5"
    - measure: Data[Avg Rating]
      color: "#B45309"
      tint: "#FEF3C7"
    - measure: Data[Price Praise %]
      color: "#0891B2"
      tint: "#CFFAFE"
    - measure: Data[Multi-Unit Order %]
      color: "#0891B2"
      tint: "#CFFAFE"
  category_colors:
    Data[Product Category]: { Clothing: "#4F46E5", Ornaments: "#0891B2", Other: "#64748B" }
    Data[Market]: { Domestic: "#64748B", International: "#C2410C" }
    Data[Buyer Gender]: { Male: "#0F766E", Female: "#9333EA" }
    Data[Sentiment]: { Positive: "#15803D", Neutral: "#94A3B8", Negative: "#B91C1C" }
  pages:
    - name: Overview
      role: landing
      archetype: Executive
      layout_variant: A
      variant_rationale: "One headline measure with a 12-point monthly series suits a KPI strip over a wide trend."
      navigation:
        - element: page_navigator
          action: PageNavigation
          target: all pages
          label: "Overview | Products | Geography and Shipping | Buyers"
      page_background: "#F5F6F8"
      layout_summary: "Header band, five KPI cards, wide monthly trend beside the category split, weekday pattern and country split underneath."
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:   [1, 1, 6, 2]
            nav:      [6, 1, 9, 2]
            filters:  [9, 1, 13, 2]
            kpis:     [1, 2, 13, 4]
            trend:    [1, 4, 9, 9]
            category: [9, 4, 13, 9]
            weekday:  [1, 9, 7, 13]
            country:  [7, 9, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "$856K in 12 months, three quarters from Clothing"
            purpose: "State the headline before any chart."
          - id: page_nav
            region: nav
            kind: pageNavigator
          - id: month_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Month]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: category_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Product Category]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: market_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Market]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: kpi_total_sales
            region: kpis
            kind: cardVisual
            purpose: "How much has the line sold?"
            field_bindings: Data[Total Sales]
            color_strategy: measure_match
            slot: 1
            of: 5
          - id: kpi_orders
            region: kpis
            kind: cardVisual
            purpose: "How many orders?"
            field_bindings: Data[Orders]
            color_strategy: measure_match
            slot: 2
            of: 5
          - id: kpi_units
            region: kpis
            kind: cardVisual
            purpose: "How many units?"
            field_bindings: Data[Units Sold]
            color_strategy: none
            slot: 3
            of: 5
          - id: kpi_aov
            region: kpis
            kind: cardVisual
            purpose: "What is a typical order worth?"
            field_bindings: Data[Avg Order Value]
            color_strategy: none
            slot: 4
            of: 5
          - id: kpi_rating
            region: kpis
            kind: cardVisual
            purpose: "How satisfied are buyers?"
            field_bindings: Data[Avg Rating]
            color_strategy: measure_match
            slot: 5
            of: 5
          - id: sales_trend
            region: trend
            kind: lineChart
            title: "Monthly sales (Nov 2024 excluded: 4 days of data)"
            purpose: "How are sales trending month to month?"
            field_bindings: { Category: Date[Month], Y: Data[Total Sales] }
            visual_filter: "Date[Is Complete Month] = TRUE"
            sort_policy: natural_order
            color_strategy: measure_match
          - id: sales_by_category
            region: category
            kind: barChart
            title: "Sales by product category"
            purpose: "Which categories perform best?"
            field_bindings: { Category: Data[Product Category], Y: Data[Total Sales] }
            sort_policy: value_desc
            color_strategy: unique
          - id: orders_by_weekday
            region: weekday
            kind: columnChart
            title: "Orders by day of week"
            purpose: "Is there a weekly buying pattern?"
            field_bindings: { Category: Date[Weekday], Y: Data[Orders] }
            sort_policy: natural_order
            color_strategy: measure_match
          - id: sales_by_country
            region: country
            kind: barChart
            title: "Sales by country"
            purpose: "Which countries contribute most?"
            field_bindings: { Category: Data[Country], Y: Data[Total Sales] }
            sort_policy: value_desc
            color_strategy: measure_match
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: trend, pct_of_content: 30 }
          balance_rationale: "The trend answers the lead question and gets the widest panel; category, weekday and country panels are each at least 4 columns by 4 rows."
    - name: Products
      role: detail
      archetype: Comparative
      layout_variant: B
      variant_rationale: "Thirteen products fit one ranked bar chart, so best and worst sellers are visible together."
      navigation:
        - element: page_navigator
          action: PageNavigation
          target: all pages
          label: "Overview | Products | Geography and Shipping | Buyers"
      page_background: "#F5F6F8"
      layout_summary: "Header band, four KPI cards, full-height product ranking on the left, rating scatter and two review panels on the right."
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:        [1, 1, 6, 2]
            nav:           [6, 1, 9, 2]
            filters:       [9, 1, 13, 2]
            kpis:          [1, 2, 13, 4]
            ranking:       [1, 4, 7, 13]
            rating_sales:  [7, 4, 13, 8]
            review_themes: [7, 8, 10, 13]
            price_praise:  [10, 8, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Five clothing lines lead; ratings barely differ"
            purpose: "State the headline before any chart."
          - id: page_nav
            region: nav
            kind: pageNavigator
          - id: month_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Month]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: category_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Product Category]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: market_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Market]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: kpi_rating
            region: kpis
            kind: cardVisual
            purpose: "What is the average rating?"
            field_bindings: Data[Avg Rating]
            color_strategy: measure_match
            slot: 1
            of: 4
          - id: kpi_positive
            region: kpis
            kind: cardVisual
            purpose: "What share of orders are rated 4 or 5?"
            field_bindings: Data[Positive Review %]
            color_strategy: none
            slot: 2
            of: 4
          - id: kpi_negative
            region: kpis
            kind: cardVisual
            purpose: "What share of orders are rated 1 or 2?"
            field_bindings: Data[Negative Review %]
            color_strategy: none
            slot: 3
            of: 4
          - id: kpi_price_praise
            region: kpis
            kind: cardVisual
            purpose: "How often do reviews praise discounts or affordability? (discount proxy)"
            field_bindings: Data[Price Praise %]
            color_strategy: measure_match
            slot: 4
            of: 4
          - id: product_ranking
            region: ranking
            kind: barChart
            title: "Sales by product, best to worst"
            purpose: "Which products are most and least popular?"
            field_bindings: { Category: Data[Product ID], Y: Data[Total Sales], Legend: Data[Product Category], Tooltips: [Data[Units Sold], Data[Orders], Data[Avg Rating]] }
            sort_policy: value_desc
            color_strategy: unique
          - id: rating_vs_sales
            region: rating_sales
            kind: scatterChart
            title: "Average rating vs sales, one bubble per product"
            purpose: "Do better-rated products sell more?"
            field_bindings: { Details: Data[Product ID], X: Data[Avg Rating], Y: Data[Total Sales], Size: Data[Units Sold], Legend: Data[Product Category] }
            color_strategy: unique
          - id: review_themes
            region: review_themes
            kind: hundredPercentStackedBarChart
            title: "What reviews talk about, by sentiment"
            purpose: "Which review themes skew positive or negative?"
            field_bindings: { Category: Data[Review Theme], Y: Data[Orders], Legend: Data[Sentiment] }
            sort_policy: category_asc
            color_strategy: semantic
          - id: price_praise_by_product
            region: price_praise
            kind: barChart
            title: "Discount and affordability praise by product (proxy: no discount data exists)"
            purpose: "Where do buyers most often credit price or discounts?"
            field_bindings: { Category: Data[Product ID], Y: Data[Price Praise %] }
            sort_policy: value_desc
            color_strategy: measure_match
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: ranking, pct_of_content: 41 }
          balance_rationale: "The ranking is the hero for the most/least popular question and needs height for 13 labelled bars; the three evidence panels are each at least 3 columns by 4 rows."
    - name: Geography and Shipping
      role: detail
      archetype: Analytical
      layout_variant: A
      variant_rationale: "Twenty-five geocoded cities with a domestic/international flag suit a map beside a ranking."
      navigation:
        - element: page_navigator
          action: PageNavigation
          target: all pages
          label: "Overview | Products | Geography and Shipping | Buyers"
      page_background: "#F5F6F8"
      layout_summary: "Header band, four KPI cards, city map beside city ranking, shipping trend beside a country table."
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:         [1, 1, 6, 2]
            nav:            [6, 1, 9, 2]
            filters:        [9, 1, 13, 2]
            kpis:           [1, 2, 13, 4]
            map:            [1, 4, 7, 9]
            city_ranking:   [7, 4, 13, 9]
            shipping_trend: [1, 9, 7, 13]
            country_table:  [7, 9, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "International: 30% of orders, 45% of sales"
            purpose: "State the headline before any chart."
          - id: page_nav
            region: nav
            kind: pageNavigator
          - id: month_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Month]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: category_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Product Category]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: market_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Market]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: kpi_intl_orders
            region: kpis
            kind: cardVisual
            purpose: "What share of orders ship internationally?"
            field_bindings: Data[International Orders %]
            color_strategy: none
            slot: 1
            of: 4
          - id: kpi_intl_sales
            region: kpis
            kind: cardVisual
            purpose: "What share of sales is international?"
            field_bindings: Data[International Sales %]
            color_strategy: none
            slot: 2
            of: 4
          - id: kpi_shipping_revenue
            region: kpis
            kind: cardVisual
            purpose: "How much of sales is shipping fees?"
            field_bindings: Data[Shipping Revenue]
            color_strategy: measure_match
            slot: 3
            of: 4
          - id: kpi_shipping_share
            region: kpis
            kind: cardVisual
            purpose: "How much of international sales is shipping rather than product?"
            field_bindings: Data[Shipping % of International Sales]
            color_strategy: none
            slot: 4
            of: 4
          - id: city_map
            region: map
            kind: azureMap
            title: "Sales by city"
            purpose: "Where in the world do orders come from?"
            field_bindings: { Latitude: Data[Latitude], Longitude: Data[Longitude], Size: Data[Total Sales], Legend: Data[Market], Tooltips: [Data[Order Location], Data[Orders]] }
            color_strategy: unique
            fallback: "If Azure Maps is unavailable in this Desktop, replace with a Total Sales by Country treemap in the same region."
          - id: city_ranking
            region: city_ranking
            kind: barChart
            title: "Sales by city, coloured by market"
            purpose: "Which cities sell most, and are they domestic or international?"
            field_bindings: { Category: Data[Order Location], Y: Data[Total Sales], Legend: Data[Market] }
            sort_policy: value_desc
            color_strategy: unique
          - id: shipping_trend
            region: shipping_trend
            kind: lineClusteredColumnComboChart
            title: "Shipping revenue and international order share by month"
            purpose: "How are shipping charges trending?"
            field_bindings: { Category: Date[Month], ColumnY: Data[Shipping Revenue], LineY: Data[International Orders %] }
            visual_filter: "Date[Is Complete Month] = TRUE"
            sort_policy: natural_order
            color_strategy: measure_match
          - id: country_table
            region: country_table
            kind: tableEx
            title: "Country summary"
            purpose: "How do order value, shipping cost and rating compare by destination?"
            field_bindings: [Data[Country], Data[Orders], Data[Total Sales], Data[Product Revenue], Data[Shipping Revenue], Data[Avg Order Value], Data[Avg Rating]]
            sort_policy: value_desc
            color_strategy: none
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: map, pct_of_content: 23 }
          balance_rationale: "Map and city ranking share the top half equally; the shipping trend and the 7-row country table each get 6 columns by 4 rows."
    - name: Buyers
      role: detail
      archetype: Analytical
      layout_variant: C
      variant_rationale: "Age, gender, category mix and basket size are equally weighted questions, so four equal panels."
      navigation:
        - element: page_navigator
          action: PageNavigation
          target: all pages
          label: "Overview | Products | Geography and Shipping | Buyers"
      page_background: "#F5F6F8"
      layout_summary: "Header band, four KPI cards, two demographic panels over two basket-size proxy panels."
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:        [1, 1, 6, 2]
            nav:           [6, 1, 9, 2]
            filters:       [9, 1, 13, 2]
            kpis:          [1, 2, 13, 4]
            age_gender:    [1, 4, 7, 9]
            category_mix:  [7, 4, 13, 9]
            basket_size:   [1, 9, 7, 13]
            multi_unit:    [7, 9, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Buyers are 70% male, spread evenly from 18 to 35"
            purpose: "State the headline before any chart."
          - id: page_nav
            region: nav
            kind: pageNavigator
          - id: month_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Month]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: category_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Product Category]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: market_slicer
            region: filters
            kind: slicer
            field_bindings: Data[Market]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: kpi_avg_age
            region: kpis
            kind: cardVisual
            purpose: "How old is the typical buyer?"
            field_bindings: Data[Avg Buyer Age]
            color_strategy: none
            slot: 1
            of: 4
          - id: kpi_male_share
            region: kpis
            kind: cardVisual
            purpose: "What share of orders come from male buyers?"
            field_bindings: Data[Male Buyer %]
            color_strategy: none
            slot: 2
            of: 4
          - id: kpi_units_per_order
            region: kpis
            kind: cardVisual
            purpose: "How many units per order?"
            field_bindings: Data[Units per Order]
            color_strategy: none
            slot: 3
            of: 4
          - id: kpi_multi_unit
            region: kpis
            kind: cardVisual
            purpose: "What share of orders contain more than one unit? (repeat-purchase proxy)"
            field_bindings: Data[Multi-Unit Order %]
            color_strategy: measure_match
            slot: 4
            of: 4
          - id: orders_by_age_gender
            region: age_gender
            kind: columnChart
            title: "Orders by buyer age and gender"
            purpose: "What is the age and gender profile of buyers?"
            field_bindings: { Category: Data[Buyer Age], Y: Data[Orders], Legend: Data[Buyer Gender] }
            sort_policy: category_asc
            color_strategy: unique
          - id: category_mix_by_age
            region: category_mix
            kind: hundredPercentStackedBarChart
            title: "What each age group buys"
            purpose: "Does category preference change with age?"
            field_bindings: { Category: Data[Age Group], Y: Data[Total Sales], Legend: Data[Product Category] }
            sort_policy: category_asc
            color_strategy: unique
          - id: orders_by_quantity
            region: basket_size
            kind: columnChart
            title: "Orders by units bought (repeat-purchase proxy: no customer ID exists)"
            purpose: "How often do buyers take more than one unit?"
            field_bindings: { Category: Data[Quantity], Y: Data[Orders] }
            sort_policy: category_asc
            color_strategy: measure_match
          - id: multi_unit_by_product
            region: multi_unit
            kind: barChart
            title: "Multi-unit order rate by product (proxy)"
            purpose: "Which products are most often bought in multiples?"
            field_bindings: { Category: Data[Product ID], Y: Data[Multi-Unit Order %] }
            sort_policy: value_desc
            color_strategy: measure_match
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: age_gender, pct_of_content: 23 }
          balance_rationale: "Four equally weighted questions get four panels of 6 columns by 4 to 5 rows; no panel dominates."
  interaction_pattern:
    drill_targets: []
    cross_filter_rules: "Default cross-highlight between charts on a page; slicers filter every visual and are synced across all four pages."
  accessibility:
    alt_text_strategy: "chart+structure: each chart's alt text names the measure, the breakdown and the takeaway"
    contrast_notes: "All text colours meet AA on #FFFFFF and #F5F6F8; Neutral grey #94A3B8 is used for fills only, never for text."
  theme:
    base: "references/design/assets/base.json adapted to the Merch Light palette"
    user_overrides: "none: the report currently uses the default theme and has one empty page"
```

## Implementation notes

- Model changes: made through the modeling MCP against the model open in Power BI Desktop, each measure checked with a DAX query against known totals (Total Sales 856,462; Orders 7,394; Units 12,334), then saved to TMDL.
- PBIR/report authoring: the existing empty page is replaced by the four pages; theme registered as a custom theme; generated with the `powerbi-report-author` CLI.
- Validation: CLI validation after each page; JSON parse check; `definition.pbir` still points to the local model.
- Desktop screenshot verification: reload in Desktop and screenshot all four pages; fix layout, binding and contrast issues found.
- Publishing boundary: none; files stay local.
- Risks:
  - Azure Maps may be disabled in this Desktop or tenant; fallback is recorded in the contract.
  - Page titles state findings for the full dataset; they do not change when a slicer is applied.
  - Manchester has two slightly different latitude values in the source, which may show as two near-identical map bubbles; the map will use one averaged point per city if so.
  - Desktop must save after the model edits for the TMDL files on disk to match.
