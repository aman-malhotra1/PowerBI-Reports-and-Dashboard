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
  
### =>Get the Most Selling Product 
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
### => No. of Products with more than Avg Selling Price
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

### New Customers , Old Customers , Lost Customers 
```
New_Customers = 
            CALCULATE([Total_Customers] ,
                        FILTER(Data,[Invoice_Date_EOM] = [Customer_First_Purchase_date_EOM]))
                        
Old_Customers = 
            [Total_Customers] - [New_Customers]
 
Lost_Customers = 
        var selected_period = SELECTEDVALUE(Months[ID])
        var current_month = MAX(Calender[Date])
        var previous_month = EOMONTH(current_month,-selected_period+1)
        var all_unique_customers_till_date = 
                    DISTINCT(
                        SELECTCOLUMNS(
                            FILTER(ALL(Data),
                                    [Invoice_Date_EOM] <= current_month),"Customers_ID",[Customer ID]))
        var all_unique_customers_in_selected_period = 
                DISTINCT(
                    SELECTCOLUMNS(
                        FILTER(ALL(Data),
                            [Invoice_Date_EOM] >= previous_month &&  [Invoice_Date_EOM] <= current_month),
                                "Customer_ID",[Customer ID]))
        return COUNTROWS(EXCEPT(all_unique_customers_till_date , all_unique_customers_in_selected_period))
        
```
![Screenshot (92)](https://user-images.githubusercontent.com/19778041/160233575-145b944c-13f8-419b-8851-ac2b045db1ee.png)

### => Customers Count with Single Invoice
```
Customer_Count_With_single_Invoice = 
        COUNTROWS(
            Filter(
                    SUMMARIZE(
                            Data,[Customer ID],"Unique_Orders",DISTINCTCOUNT(Data[Invoice])),
                                [Unique_Orders]=1) )
```

### => Sales Segmentation as per Profit Margin
```
Sales_Segmentation = 
            CALCULATE([Total_Sales],
                FILTER(VALUES(Margin_table[Margin Percentage]),
                    COUNTROWS(
                            FILTER(Margin_Type,
                                    Margin_table[Margin Percentage]>= Margin_Type[Min] && 
                                    Margin_table[Margin Percentage] <Margin_Type[Max]))>0
                    ))   
```            

![Screenshot (95)](https://user-images.githubusercontent.com/19778041/160267666-8ffe7f3b-4c28-4c10-a0a7-c8fe188d23fd.png)

![Screenshot (94)](https://user-images.githubusercontent.com/19778041/160267670-3074eac2-503b-491b-8879-fe9fbba7266d.png)

### => Date based DAX calculations
```
Commulative_Sales = 
            var current_month = MAX(Calender_Table[Date])
            return 
                CALCULATE([Total_Sales] , 
                        FILTER(ALL(Calender_Table ), 
                                    [Date]<= current_month))

Commulative_Sales_Yearly = 
            var current_year = year(Max(Calender_Table[Date]))
            var current_date = Max(Calender_Table[Date])
            return 
                CALCULATE([Total_Sales] ,
                        FILTER(All(Calender_Table),
                                [Date]<=current_date && [Year] = current_year))
                                
Previous_Month_Sales = 
            CALCULATE(
                    [Total_Sales] , 
                            DATEADD((Calender_Table[Date]),-1,MONTH))
                            
Same_Period_Last_Year_ = 
                CALCULATE([Total_Sales],
                            SAMEPERIODLASTYEAR(Calender_Table[Date]))

Moving_Average = 
        var moving_average_window = 2
        var current_date = Max(Calender_Table[Date])
        var previous_date =  EOMONTH(current_date,-moving_average_window-1)
        return 
            DIVIDE(
                   CALCULATE([Total_Sales] , 
                     FILTER(All(Calender_Table),
                       [Date]> previous_date && [Date]<= EOMONTH(current_date,-1))),
                   moving_average_window,0)

```

![Screenshot (97)](https://user-images.githubusercontent.com/19778041/160267931-46206e8c-ef00-4ad4-826a-b0d5f21d9884.png)

### => Sector Report for IHS Markit 
https://app.powerbi.com/view?r=eyJrIjoiNTQ2NzhmNzQtZjUyYi00YjliLTkxMmYtZWFlZWI4OThkMmY3IiwidCI6IjQ5OWJjMjFhLTc2NDAtNGJmZi1hNjMzLTU3NmY0NmZkNmJmNCJ9

![Screenshot (99)](https://user-images.githubusercontent.com/19778041/160268173-06b795a0-6b7e-46ca-9cf3-cd5a99423061.png)
