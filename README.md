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

### => Stock Price Percentage Difference from date selected

https://app.powerbi.com/view?r=eyJrIjoiOWNiMDEzZmYtNTAzMS00NDdhLTgwZTUtZmExZjBhY2Y0NjIwIiwidCI6IjQ5OWJjMjFhLTc2NDAtNGJmZi1hNjMzLTU3NmY0NmZkNmJmNCJ9
![Screenshot (90)](https://user-images.githubusercontent.com/19778041/160073077-628f9813-3868-49bd-bdb1-2a051d11b208.png)

### => Sales Percentage Summarize by Region and Year
https://app.powerbi.com/view?r=eyJrIjoiZDZkMzQ1NGYtYjZkZC00ZTI5LTljYzQtMDAxNjBlNjIxZDNjIiwidCI6IjQ5OWJjMjFhLTc2NDAtNGJmZi1hNjMzLTU3NmY0NmZkNmJmNCJ9
![Screenshot (91)](https://user-images.githubusercontent.com/19778041/160074104-face1a4d-3c4f-4623-9a67-8a0e92332f44.png)
