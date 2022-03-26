## PowerBI-Reports-and-Dashboard
### => Get the Most Expensive Product
```
Most_Expensive_Product = 
    FIRSTNONBLANK(
        TOPN(
            1 , 
            VALUES('Product'[ProductName]), 
            Calculate(Max('Product'[UnitPrice])), DESC),1)
  ```
  
### Get the Most Selling Product 
```
Most_Selling_Product = 
            TOPN(
                1,VALUES('Product'[ProductName]),
                                CALCULATE(SUMX(Sales,[SalesAmount])),DESC)
```                          

### => Total Sales by Top 10 Products
```
Total_Sales_Top_10_Products = 
            CALCULATE([Total_Sales],
                TOPN(10 , VALUES('Product'[ProductName]), [Total_Sales] , DESC))
```
### => Show only Top 10 Products
```
Top_10_Products = 
            CALCULATE([Total_Sales] ,
                    FILTER(VALUES('Product'[ProductName]),
                            IF(RANKX(All('Product'),[Total_Sales],,DESC) <=10,[Total_Sales],BLANK())))

```
### No. of Products with more than Avg Selling Price
```
Count_products_more_than_average_selling_price = 
            var avg_selling_price = AVERAGE('Product'[UnitPrice])
            return              COUNTROWS(
                                    CALCULATETABLE('Product',
                                                FILTER('Product',[UnitPrice] > avg_selling_price)))
```                                             


### => Stock Price Percentage Difference from date selected

https://app.powerbi.com/view?r=eyJrIjoiOWNiMDEzZmYtNTAzMS00NDdhLTgwZTUtZmExZjBhY2Y0NjIwIiwidCI6IjQ5OWJjMjFhLTc2NDAtNGJmZi1hNjMzLTU3NmY0NmZkNmJmNCJ9
![Screenshot (90)](https://user-images.githubusercontent.com/19778041/160073077-628f9813-3868-49bd-bdb1-2a051d11b208.png)

### => Sales Percentage Summarize by Region and Year
https://app.powerbi.com/view?r=eyJrIjoiZDZkMzQ1NGYtYjZkZC00ZTI5LTljYzQtMDAxNjBlNjIxZDNjIiwidCI6IjQ5OWJjMjFhLTc2NDAtNGJmZi1hNjMzLTU3NmY0NmZkNmJmNCJ9
![Screenshot (91)](https://user-images.githubusercontent.com/19778041/160074104-face1a4d-3c4f-4623-9a67-8a0e92332f44.png)
