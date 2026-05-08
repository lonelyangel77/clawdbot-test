# Measure Categorization Design
**Date:** 2026-05-07  
**Model:** Power BI Desktop (localhost:56543)  
**Total measures:** 807 across 23 tables

---

## Goal

Organize 807 measures — currently all flat with no `displayFolder` — into a logical subject-area table structure with hierarchical display folders. Approach: **Option A — Subject-area split** with region tables kept as dedicated tables.

---

## 1. Table Structure

### New / Renamed Tables (from `Key Measures`)

| New Table | Source | Purpose |
|---|---|---|
| `Sales` | Key Measures | Base actuals: value, units, cost, GP, returns, productivity, store info, rankings |
| `Time Intelligence` | Key Measures | All MTD / YTD / WTD / LY / Weekly / Previous-period variants |
| `Targets & Budget` | Key Measures + Target Ach% | TGT, TGT-U, TGT-GP, budgeted sales, all target variances |
| `Forecast` | Key Measures | Run-rate, ME Forecast, budget dilution, growth vs plan, indicators |
| `Pricing & Dilution` | Key Measures | Dilution%, Discount, Full Price, A.S.P, price variances |
| `Inventory` | Key Measures_Inv (rename) | Inventory on-hand, receiving, sell-thru, aging, POs, stock movement |

### Tables Kept As-Is

| Table | Notes |
|---|---|
| `UK_Measures` | Keep; add/clean subfolders |
| `US_Measures` | Keep; add subfolders (currently all flat) |
| `UAE_Measures` | Keep; add subfolders (currently all flat) |
| `ROW_Measures` | Keep; expand from flat `ROW` folder into subfolders |
| `Retail KPIs` | Keep; add subfolders |
| `Promo_Measures` | Keep; no changes needed |
| `Ramzan_Measures` | Keep; no changes needed |
| `FootFall Measures` | Keep; add subfolders |
| `Store_Budget_Measures` | Keep; add subfolders |
| `Seasonal_Measure` | Keep as-is |
| `SVG_Measures` | Keep as-is |
| `WIF Measures` | Keep as-is |
| `WIF_OFT_ACLASS` etc. (6 parameter tables) | Keep as-is |
| `Calendar` | Keep; assign label measures to `Labels` folder |
| `Stores` | Keep; assign M.SQFT and BrandN to `Store Info` folder |

### Duplicates to Resolve Before Migration

The following measure pairs have overlapping purpose and should be consolidated (keep one, hide the other):

| Pair | Notes |
|---|---|
| `GP` vs `GP-1` | `GP` uses `SUM(Sales Tax)`, `GP-1` uses `SUM(Sal Tax)` — different tax columns |
| `CGS` vs `CGS-1` | Different cost sources; confirm which is authoritative |
| `Sales Tax` vs `Sales Tax-1` | Two different columns (`SALES-TAX DATA` vs `SALES[Sal Tax]`) |
| `Net Sales` vs `Net Sales-1` | Follow from the Sales Tax pair above |
| `LY GP` vs `LY G.P` | Identical expressions — `LY G.P` is a duplicate, hide it |
| `MTD TGT GP Var%` vs `MTD TGT GP VAR` | Similar names, check if both needed |

---

## 2. Display Folder Structure

### `Sales`
```
Sales
├── Base
│   └── Sale-Value, Sale-Units, Net Sales, Sales Tax, CGS, GP, GP%, Sale Share %
├── Returns
│   └── Sale Return (U), Sale Return (V), Sale Return %
├── Pricing
│   └── A.S.P, Avg Sale Price, LY A.S.P, A.S.P Var%
├── Productivity
│   ├── Per Day Sale, No of Days, No of Month, Sale per Month
│   ├── Per Day-Per Emp Sale, Emp_Store_Count
│   └── Sale per Sq. FT per Month, SQFTAREA, LYSQFTArea
├── Store Info
│   └── Opening Date, Active Days, Store New Status, Last Sale, last Sale Age
│       First_Date, Last_Date, No Sale Days, Ly Active Days
└── Rankings
    └── Rank Item, Rank Visb, Bottom5Stores
```

### `Time Intelligence`
```
Time Intelligence
├── MTD
│   ├── MTD Sales, MTD Sales (U), MTD GP, MTD GP%, MTD Full Price Sales, MTD Dilution%
│   ├── LY\     LY MTD Sales, LY MTD Sales (U), LY MTD GP, LY MTD GP%
│   │           LY MTD Full Sales, LY MTD DILUTION%
│   └── Variance\  MTD Var, MTD Var (U), LY MTD Var%, LY MTD (U) Var%
│                  MTD GP Var, MTD GP Var%, MTD GP% Var%
├── YTD
│   ├── YTD Sales, YTD Sales (U), YTD GP, YTD GP%, YTD Full Sales
│   ├── LY\     LYTD Sales, LYTD Sales (U), LYTD GP, LYTD GP%, LYTD Full Sales
│   └── Variance\  YTD Var, YTD Var%, YTD (U) Var, YTD (U) Var%
│                  YTD GP Var, YTD GP Var%, YTD GP% var
├── WTD
│   ├── WTD_Value, WTD Units, WTD GP, WTD GP%
│   │   WTD Sales Var, WTD Sales Var%, WTD Units Var%
│   ├── LY\     LY WTD Value, LY WTD Units, LY WTD GP, LY WTD GP%
│   │           LY WTD Sales Tax, LY WTD CGS
│   ├── UK\     WTD_Value-UK, WTD_Value-UK_BM, WTD Target-UK
│   ├── US\     WTD_Value-US, WTD Target-US
│   └── Combined\  WTD targets All, WTD Value All, WTD Var% All
├── Weekly
│   ├── This Week, Last Week, Week Day Sale, Last Week Day Sale
│   │   Previous Week Var%, Week Day Var%
│   ├── UK\     This Week-UK
│   ├── US\     This Week-US
│   └── Combined\  This Week-All, This Week-Targets-PK
├── LY
│   ├── LY Sales, LY Units, LY GP, LY GP%, LY NetSales
│   │   LY Full Price Sale, LY Per Day Sale, LY No of Days, LY A.S.P
│   └── Variance\  Sales Var, Sales Var%, Units Var, Units Var%
│                  GP var, GP Var%, GP%Var
└── Previous Period
    └── Previous Day Sales, Previous Day %, Previous Month Sales
        Previous Year Sales, HA, HA-week, PW SALES
```

### `Targets & Budget`
```
Targets & Budget
├── Sales Target
│   ├── TGT, MTD Target, YTD Target
│   └── Variance\  Target Ach %, MTD Target Ach %, YTD Target Ach %
│                  MTD Tar Var%, MTD Tar Var, YTD Tar Var%, YTD Tar Var, Remaining Target
├── Units Target
│   ├── TGT-U, MTD TGT-U, YTD TGT-U
│   └── Variance\  TGT-U Var, TGT-U Var%, MTD Tar-U Var%, YTD TGT-U Var%
├── GP Target
│   ├── TGT-GP, MTD TGT GP, YTD TGT GP
│   └── Variance\  TGT GP Var %, GP TGT Var%, MTD TGT GP Var%
│                  MTD TGT GP VAR, YTD TGT GP Var, YTD TGT GP Var%
├── Budget
│   ├── Budgeted Sales, Bud NetSales, Bud. Sale Var%, Target vs Budget (Value)
│   └── GP\  Bud GP%, Bud GP% Var, MTD Bud GP%, MTD Bud GP% Var
│            YTD Bud GP%, YTD Bud GP% var
└── LY Targets
    └── LY MTD Targets, LY Target, Target Var, LY Tar Var%
        LY Tar Bal %, Tar% Var
```

### `Forecast`
```
Forecast
├── Run Rate
│   └── ME Forecast, EoM Sales, Current Daily Avg, Req Avg
│       Remaining Sales, Remaining Tget, Days Passed, DaysLeft
├── Budget Forecast
│   ├── Forcast Sales, Forecast Sales Cost, Forecast Sales Tax
│   │   Forecast Sales G.P, Forecast Sales G.P%
│   ├── MTD\  MTD Forecast Sales, MTD Forecast Var, MTD ForeCast Var%
│   │         MTD ForeCast GP Var%, MTD Forecast GP Var
│   └── YTD\  YTD Forecast, YTD Forecast Var, YTD ForeCast Var%
│             YTD Forecast GP Var, YTD TGT GP Var%
├── Budget Dilution
│   ├── MTD\  Bud.FullPriceSale, MTD Bud.FP Sales, MTD Bud. Dilution%
│   │         MTD Bud. DILUTION% VAR, MTD Bud. DILUTION% SHORT
│   └── YTD\  YTD Bud. FP Sale, YTD Bud. Dilution%, YTD Bud. Dilution Var%
│             YTD Bud. Dilution Short%
├── Growth vs Plan
│   └── BudgetVSLY, MTDBudgetVSLY, YTD SALE GROWTH VAR%, MTD SALE GROWTH VAR%
│       YTD GP GROWTH, YTD GP GROWTH VAR%, MTD GP GROWTH, MTD GP GROWTH VAR
└── Indicators
    └── Sales Indicator PP KPI, Sales Indicator MTD KPI, Green Max, Red Max
```

### `Pricing & Dilution`
```
Pricing & Dilution
├── Dilution
│   ├── Dilution%, Dilution (V), LY Dilution, Dilution% Var
│   ├── MTD\  MTD Dilution%, LY MTD DILUTION%, MTD DILUTION% VAR, MTD DILUTION% SHORT
│   └── YTD\  YTD Dilution%, LYTD Dilution%, YTD Dilution Var%, YTD Dilution Short%
├── Discount
│   └── Disct%, Discount (Val), Discount (V) Sale, Sale-Discount
│       FP Sale Share, Disc Sale Share
└── Full Price
    └── Full Price Sale Vaule, Full Price Sale, MTD Full Price Sales
        YTD Full Sales, LYTD Full Sales, LY Full Price Sale, LY MTD Full Sales
```

### `Inventory` (renamed from `Key Measures_Inv`)
```
Inventory
├── On Hand
│   └── Inv_Units, Inv_Cost, Inv_Retail, Inv_OnHand_U, Inv_Transit_U
│       Inv_RetailValue, Inv_RetailValue(K), Inv_Units(K), Inv_Cost1, Inv_FP (V)
├── Receiving
│   └── Receivied Units, Receivied Cost, Received Units RT, Received Cost RT
│       Sale Units RT, CGS RT, Receiving Date, ReceivedDate, ReceivedAge
│       Age, Receivied Cost-1, Received Units ALL
├── Sell-Thru & Turns
│   └── SellThru%, SellThru%_MTD, SellThru%_YTD, NDI, NDI_YTD
│       Inv_Liq%, Cost Liq%, Liq%, Inv_Days, Inv_Rec%
├── Aging
│   └── Aging_Units, Aging_30_U, Aging_60_U, Aging_90_U, Aging_120_U, Aging_120+_U
│       Aging_Cost, Aging_30_C, Aging_60_C, Aging_90_C, Aging_120_C, Aging_120+_C
├── Purchase Orders
│   └── PO Order Qty, PO Receive Qty, PO Balance Qty, No of PO
│       No of PO Items, PO Received%, Dispatch Qty, Dispatch Qty%
├── Stock Movement
│   └── Closing, Closing (C), Inv_Adj, Inv_Mov, Stock Adjustment
│       Closing Difference, Balance Units, Balance Cost
├── GP & Profitability
│   └── Inv_GP%, Inv_GP, Inven_GP%, Inv_Net Sales, ROI, Cost Rec
│       Sales Tax_Inv, Inv_Retail_FP, Inv_Retail_Disc
└── Diagnostics
    └── zaro CGS Items Without Cost, CGS Diff
```

### `Retail KPIs`
```
Retail KPIs
├── Traffic
│   └── FF_TGT, TGT-FF Var%
├── Conversion & Basket
│   └── CY Conversion%, LYConversion%, Conv%_TGT, Conv% Var
│       ATV, ATV_TGT, TGT-ATV Var%, UPT, UPT_TGT, UPT Var
├── ASP
│   └── ASP, ASP_TGT, TGT-ASP Var%
└── Season Mix
    └── CY Season Sale, LY_Season Sale, Sale Season (V) Var%, CY Old Sale, LY Old Sale
```

### `FootFall Measures` (add subfolders)
```
FootFall Measures
├── Base
│   └── Sales(V), Units Sold, Transactions, Footfall, Return Units, Return Value
├── LY
│   └── LY Sales (V), LY Units Sold, LY Transactions, LY Footfall, LY ASP, LY ATV, LY Conversion%, LY UPT
└── KPIs & Variance
    └── ASP, ASP Var%, ATV, ATV Var%, UPT, UPT Var%, Conversion%, Conversion% Var, Conversion% Var%
        Footfall Var%, Units Sold Var%
```

### `Store_Budget_Measures` (add subfolders)
```
Store_Budget_Measures
├── Budget
│   └── StoreBudgetSales
└── Variance
    └── StoreBudVar, StoreBudVar%, BudLYVar, BudLYVar%
```

---

## 3. Region Tables

### `UK_Measures`
```
UK_Measures
├── Sales
│   ├── UK_Sale Value, UK_Sale Units, UK_Net Sales, UK_Tax, UK_CGS
│   │   UK_GP, UK_GP%, UK_FullPrice, UK_Sales Value
│   └── Returns\  UK_Return_Sale Units, UK_Return_Sale Value, UK_Return_No of Orders
├── Sales\B&M
│   ├── UK_Sale_B&M (Rs), UK_Sales_B&M (GBP), UK_Sales_Total
│   ├── MTD\  UK_Sales_B&M_MTD, UK_Sales_B&M_MTD (GBP)
│   └── YTD\  UK_Sales_B&M_YTD, UK_Sales_B&M_YTD (Rs)
├── Sales\Shopify
│   ├── UK_Sale(Rs/GBP/USD), UK_fulfilled(Rs/GBP/USD)
│   │   UK_Pending(Rs/GBP), UK_Return(Rs/GBP), UK_SalesTax(GBP)
│   ├── MTD\  UK_MTD Sales(Rs/GBP/USD)
│   └── YTD\  UK_YTD Sales(Rs/GBP/USD)
├── Targets
│   ├── UK_Targets(GBP/PKR/USD), UK_TGT_OL, UK_TGT_BM
│   ├── MTD\  UK_MTD Targets, UK_MTD Targets(GBP/USD), UK_MTD Sales Var%
│   └── YTD\  UK_YTD Sales, UK_YTD Targets(GBP/USD)
├── Budget
│   ├── UK_Budget, MTD_UK_Budget, UK_Budget_YTD
│   └── Variance\  UK_Budget Var/Var%, UK_MTD Budget Var/Var%, UK_YTD Budget Var/Var%
├── LY
│   └── UK_LY Sales, UK_LY Sale(Rs), UK_MTD LY Sales(Rs), UK_YTD LY Sales(Rs)
├── Invoicing
│   └── UK_INV_Charges, UK_INV_Net Sales, UK_INV_Orders, UK_INV_Tax
└── Combined (All Regions)
    ├── Sale-Value All, Net Sales All, CGS All, GP All, GP% All
    │   Fullprice All, Dilution% All, Targets All, Targets Var All, Targets Var% All
    │   Budget All, Budget Var All/Var% All
    │   LY Sale-Value All, LY Sale Var% All
    │   Previous Day Sales All, Previous Day % All
    ├── MTD\  MTD Sales All, MTD Targets All, MTD Target Var/Var% All
    │         MTD Budget All, MTD Budget Sales All, MTD Budget Sales Var/Var% All
    │         MTD Budget Var/Var% All, MTD LY Sales All, MTD LY Sale Var% All
    └── YTD\  YTD Sale-Value All, YTD Targets All, YTD Net/CGS/GP/GP%/Fullprice/Dilution% All
              YTD Budget/Var/Var% All, YTD LY Sale-Value/Var% All
              YTD Previous Day Sales/% All
```

### `US_Measures`
```
US_Measures
├── Sales
│   └── US_Sale(Rs), US_Sales(USD), US_Sales_BM(Rs/USD)
│       US_Online_Sales(USD), US_fulfilled(Rs), US_Pending(Rs), US_Return(Rs)
├── Targets
│   ├── US_Targets Value, US_Targets(USD)
│   ├── MTD\  US_MTD Sales, US_MTD Sales(USD), US_MTD Targets, US_MTD Targets(USD), US_MTD Target Var%
│   └── YTD\  US_YTD Sales(Rs/USD), US_YTD Targets(USD)
├── Budget
│   ├── US_Budget, US_Budget_YTD
│   └── Variance\  US_Budget Var/Var%, US_MTD Budget/Var/Var%, US_YTD Budget Var/Var%
├── LY
│   └── US_LY Sales(Rs), US_MTD LY Sales(Rs), US_YTD LY Sales(Rs)
└── International (Int_ combined)
    ├── Int_Sales, Int_Targets, Int_Budget
    ├── MTD\  Int_MTD Sales(Rs/USD), Int_MTD Targets(Rs/USD)
    │         Int_MTD TGT Var/Var%, Int_MTD Budget/Var/Var%
    └── YTD\  Int_YTD Sales, Int_YTD Targets, Int_YTD_TGT_Var/Var%
              Int_YTD Budget/Var/Var%
```

### `UAE_Measures`
```
UAE_Measures
├── Sales
│   └── UAE_Sales(Rs), UAE_Sales_OL(Rs)
├── Targets
│   ├── UAE_Targets_RS
│   ├── MTD\  UAE_MTD_Sales_Rs, UAE_MTD_Targets_Rs
│   └── YTD\  UAE_YTD Sales(Rs)
├── Budget
│   ├── UAE_Budget(Rs), UAE_Budget_YTD
│   └── Variance\  UAE_Budget Var/Var%, UAE_MTD Budget/Var/Var%, UAE_YTD Budget Var/Var%
└── LY
    └── UAE_LY Sales(Rs), UAE_MTD LY Sales(Rs), UAE_YTD LY Sales(Rs)
```

### `ROW_Measures`
```
ROW_Measures
├── Sales
│   └── ROW_Sale(Rs), ROW_Sales(USD), ROW_Online_Sales(USD), ROW_fulfilled(Rs)
├── Targets
│   ├── ROW_Targets Value, ROW_Targets(USD)
│   ├── MTD\  ROW_MTD Sales, ROW_MTD Sales(USD), ROW_MTD Targets
│   │         ROW_MTD Targets(USD), ROW_MTD Target Var%
│   └── YTD\  ROW_YTD Sales(Rs/USD), ROW_YTD Targets(USD)
├── Budget
│   ├── ROW_Budget(Rs), ROW_Budget_YTD
│   └── Variance\  ROW_Budget Var/Var%, ROW_MTD Budget/Var/Var%, ROW_YTD Budget Var/Var%
└── LY
    └── ROW_LY Sales(Rs), ROW_MTD LY Sales(Rs), ROW_YTD LY Sales(Rs)
```

---

## 4. Calendar & Stores Cleanup

**`Calendar` table** — assign label helper measures to a `Labels` display folder:
- YTD Selected, MTD Selected, Yesterday Date, Month Selected, Selected Month, Ramzan No

**`Stores` table** — assign to `Store Info` display folder:
- M.SQFT, BrandN

---

## 5. Out of Scope

The following tables are not touched in this work:
- `Ramzan_Measures` — already purpose-built, no folders needed
- `Promo_Measures` — already purpose-built
- `Seasonal_Measure`, `SVG_Measures` — keep as-is
- `WIF Measures` and the 6 WIF parameter tables — keep as-is

---

## 6. Implementation Approach

All changes are applied via `pbi measure update --display-folder` and `pbi table rename`. No DAX expressions are modified. Report rebinding is required only for measures that move to a new table name (all `Key Measures` → new table names). Region tables, Inventory rename, and folder-only changes require no rebinding.
