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
| post_invoice_deductions | data of amount that needs to be deducted after net invoice sales |
| manufacturing costs | data of manufacturing amount that needs to be deducted after net sales |
| freight cost | data about transportation cost that needs to be deducted after net sales |
| operational expenses | data about operational cost that needs to be deducted after net sales |


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

