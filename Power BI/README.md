## Problem Statement

Atliq is a company that sells hardware like PC, mouse, printers and so on to different customers – Croma, BestBuy, Staples, Flipkart, Amazon. These stores will then sell the hardware to consumers like you and me.  

Atliq Hardware has become one of the fastest growing company in the electronic Goods Market. While they grew substantially in last few years, they had some bad experience  in Latin America. Their goal was to establish a presence there. Despite opening their own store, they suffered a huge loss.. And this happened because they made all this decisions just based on some surveys that they conducted and just based on their intuitions.   

In the annual strategic meeting one of the top items was to onboard data analytics in the company and to make data-driven decisions. So the company has hired a data analytics team and they have assigned the task to that team to onboard data analytics and to bring a lot of transparency into their data so that they can make accurate decisions. 

## Data Modeling

<img width="900" alt="image_2023-11-28_12-55-38" src="https://github.com/leylavel/Visualisations/assets/61410191/7a08741a-5973-4fa5-bee8-3942e70f55f7">

| Tables  | Description |
| ------------- | ------------- |
| fact_sales_monthly  | transactional sales data  |
| fact_forecast_monthly  | forecast quantity of products |
| fact_actuals_estimates | both fact_sales_monthly and fact_forecast_monthly |
| post_invoice_deductions | amount that needs to be deducted after net invoice sales |
| manufacturing costs | manufacturing amount that needs to be deducted after net sales |
| freight cost | transportation cost that needs to be deducted after net sales |
| operational expenses | operational cost that needs to be deducted after net sales |

**fact_actuals_estimates** columns  
* ads_promotion
```
ads_promotions = 
var res = CALCULATE(MAX('Operational Expence'[ads_promotions_pct]), RELATEDTABLE('Operational Expence'))
return res*fact_actual_estimates[net_sales_amount]
```

* freight_cost
```
freight_cost = 
var res = CALCULATE(MAX(freight_cost[freight_pct]), 
RELATEDTABLE(freight_cost))
return res*fact_actual_estimates[net_sales_amount]
```

* manufacturing_cost
```
manufacturing_cost = 
var res = CALCULATE(MAX(manufacturing_cost[manufacturing_cost]), 
RELATEDTABLE(manufacturing_cost))
return res*fact_actual_estimates[Qty]
```

## Dashboards
<img width="500" alt="image" src="https://github.com/leylavel/Visualisations/assets/61410191/a06f45d2-ef6b-4718-a3ba-9950f10e67a0">

### Finance view  
Get P & L statement for any customer / product / country or aggregation of the above over any time period
![Business360_2_page-0001](https://github.com/leylavel/Visualisations/assets/61410191/3ff1a26d-a2ac-417a-af50-594540b96ab3)

### Sales view  
Analyze the performance of your customer(s) over key metrics like Net Sales, Gross Margin and view the same in profitability / Growth matrix.
![Business360_2_page-0002](https://github.com/leylavel/Visualisations/assets/61410191/01aeda3a-d2aa-4776-85b2-8dbdf5f93f4b)

### Marketing view  
Analyze the performance of your product(s) over key metrics like Net Sales, Gross Margin and view the same in profitability / Growth matrix.  
![Business360_2_page-0003](https://github.com/leylavel/Visualisations/assets/61410191/11175c52-3730-40be-94c0-90db6fe27480)

### DAX
#### P & L Final Value
```
P & L Final Value = SWITCH(TRUE(),
SELECTEDVALUE(fiscal_year[fy_desc]) = MAX('P & L Columns'[Col Header]), [P & L Values],
MAX('P & L Columns'[Col Header]) = "LY", [P & L LY],
MAX('P & L Columns'[Col Header]) = "YoY Cng", [P & L YoY Chg],
MAX('P & L Columns'[Col Header]) = "YoY Cng%", [P & L YoY Chg %]
)
```
#### P & L Values
```
P & L Values = 
var res = SWITCH(TRUE(),
MAX('P & L Rows'[Order]) = 1, [GS $]/1000000,
MAX('P & L Rows'[Order]) = 2, [Pre Invoice Deduction $]/1000000,
MAX('P & L Rows'[Order]) = 3, [NIS $]/1000000,
MAX('P & L Rows'[Order]) = 4, [Post Invoice Deduction $]/1000000,
MAX('P & L Rows'[Order]) = 5, [Post Invoice Other Deduction $]/1000000,
MAX('P & L Rows'[Order]) = 6, [Post Invoice Deduction $]/1000000 + [Post Invoice Other Deduction $]/1000000,
MAX('P & L Rows'[Order]) = 7, [NS $]/1000000,
MAX('P & L Rows'[Order]) = 8, [Manufacturing Cost $]/1000000,
MAX('P & L Rows'[Order]) = 9, [Freight Cost $]/1000000,
MAX('P & L Rows'[Order]) = 10, [Other Cost $]/1000000,
MAX('P & L Rows'[Order]) = 11, [Total COGS $]/1000000,
MAX('P & L Rows'[Order]) = 12, [GM $]/1000000,
MAX('P & L Rows'[Order]) = 13, [GM %],
MAX('P & L Rows'[Order]) = 14, [GM / Unit],
MAX('P & L Rows'[Order]) = 15, [Operational Expense $]/1000000,
MAX('P & L Rows'[Order]) = 16, [Net Profit $]/1000000,
MAX('P & L Rows'[Order]) = 17, [Net Profit %]*100
)
return IF(HASONEVALUE('P & L Rows'[Line Item]), res, [NS $]/1000000)
```
#### Sales Qty
```
Sales Qty = CALCULATE([Quantity], fact_actual_estimates[date] <= MAX(LastSalesMonth[LastSalesMonth]) )
```

#### Selected P & L Row
```
Selected P & L Row = IF(HASONEVALUE('P & L Rows'[Line Item]), SELECTEDVALUE('P & L Rows'[Line Item]), "Net Sales")
```

#### Total COGS $
```
Total COGS $ = 'Key Measures'[Manufacturing Cost $] + 'Key Measures'[Freight Cost $] + 'Key Measures'[Other Cost $]
```

#### P & L LY
```
P & L LY = CALCULATE([P & L Values], SAMEPERIODLASTYEAR(dim_date[date]))
```
#### P & L Columns
```
P & L Columns = 
var x = ALLNOBLANKROW(fiscal_year[fy_desc])

return UNION(
    ROW("Col Header", "LY"),
    ROW("Col Header", "YoY Cng"),
    ROW("Col Header", "YoY Cng%"), x)
```
#### ABS Error 
```
ABS Error = 
SUMX(                       // 5 Iterate this formula for all months and sum them up
DISTINCT(dim_date[month]),  // 4 convert net error to Abs error by taking absolute value for each month

SUMX(                                 // 3 Iterate for all products and sum them up
DISTINCT(dim_product[product_code]), 
ABS([Net Error]) // 1 convert net error to Abs error by taking absolute value
)
)
```
