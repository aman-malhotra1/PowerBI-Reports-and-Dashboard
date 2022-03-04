## PowerBI-Reports-and-Dashboard
### 1) Get the Most Expensive Product
```
Most_Expensive_Product = 
    FIRSTNONBLANK(
        TOPN(
            1 , 
            VALUES('Product'[ProductName]), 
            Calculate(Max('Product'[UnitPrice])), DESC),1)
  ```
### 2) Total Sales by Top 10 Products
```
Total_Sales_Top_10_Products = 
            CALCULATE([Total_Sales],
                TOPN(10 , VALUES('Product'[ProductName]), [Total_Sales] , DESC))
       ```
