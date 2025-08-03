# 62. 33a. Enabling the calculation groups preview feature

Create a new table visual like this 

<img width="949" height="540" alt="image" src="https://github.com/user-attachments/assets/34409d8c-dc62-4f4e-8b72-ba8061a3875a" />

Create a new measure that calculates running total for `SalesAmount` and `OrderQuantity`

```
SalesAmount YTD = TOTALYTD(sum(FactInternetSales[SalesAmount]), FactInternetSales[OrderDate].[Date])
OrderQuantity YTD = TOTALYTD(sum(FactInternetSales[OrderQuantity]), FactInternetSales[OrderDate].[Date])
```

You can use **Calculation Group** to create measure on a group of columns.

**As of 2025-08-03, Calculation Group is GA**

It can be found in **Model View**

<img width="1682" height="262" alt="image" src="https://github.com/user-attachments/assets/dbe38dd3-ebec-4218-906d-b11f7de1f07e" />

read more about the **Model Explorer**

https://learn.microsoft.com/en-us/power-bi/transform-model/model-explorer

# 63. 33a. Creating our first calculation group and item

First create a **DimDate** date table.

```
DimDate = ADDCOLUMNS(CALENDARAUTO(), 
        "CalendarYear", Year([Date]),
        "MonthNumber", Month([Date]),
        "MonthName", Format([Date], "MMM"),
        "Quarter", "Q" & QUARTER([Date]),
        "DayOfWeekNumber", WEEKDAY([Date]),
        "DayOfWeekName", FORMAT([Date], "dddd")
)
```

Create a relationship with **FactInternetSales** using `Date` column and `OrderDate` column

<img width="626" height="863" alt="image" src="https://github.com/user-attachments/assets/906fa14c-bfc5-4dff-89bc-0ea770b19a25" />

Make it as the Date Table

<img width="359" height="308" alt="image" src="https://github.com/user-attachments/assets/86c8101a-7c35-4686-bd90-ec40492ef316" />


Then disable **Time Intelligence** in PBID Options

<img width="697" height="786" alt="image" src="https://github.com/user-attachments/assets/a6460d49-94b2-4268-9cf9-6d505cd0340c" />

You will get errors on the measures now because there use **Time Intelligence**

<img width="439" height="450" alt="image" src="https://github.com/user-attachments/assets/13abd2ec-c1bd-4f6e-84b8-c091a3dbc880" />

So we change from FactInternetSales`[OrderDate].[Date]` to `DimDate[Date]`


```
SalesAmount YTD = TOTALYTD(sum(FactInternetSales[SalesAmount]), DimDate[Date])
OrderQuantity YTD = TOTALYTD(sum(FactInternetSales[OrderQuantity]), DimDate[Date])
```

Update the table with the following columns, note that you will need to add the dates and hierchary back in.

<img width="1163" height="622" alt="image" src="https://github.com/user-attachments/assets/a5ed4ddc-0bab-4ed7-b40e-55b5c7785331" />


### Create Calculation Group

<img width="1148" height="888" alt="image" src="https://github.com/user-attachments/assets/641c54f1-e73d-4588-bc9a-c89543f388be" />

Click on *yes*. This will stop implicit measures from being created in visuals.


```
YTD = CALCULATE(SELECTEDMEASURE(), DATESYTD(DimDate[Date]))
```

