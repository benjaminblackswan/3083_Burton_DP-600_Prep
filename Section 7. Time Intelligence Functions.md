## 43. Date and Time functions and creating a Date Table

```
DateTable = CALENDAR(DATE(2026,1,1), DATE(2031,12,31))
DateTable = CALENDARAUTO(12)
```

## 44. 32b. Using the USERELATIONSHIP, EDATE and CROSSFILTER functions

Create a `Due Date` Column

```
DateDue = EDATE(FactFinance[Date],1)
```

### Use USERELATIONSHIP

```
DateDueYear = CALCULATE(MIN(DateTable[Year]), USERELATIONSHIP(FactFinance[DateDue],DateTable[Date]))
DateDueMonth = CALCULATE(MIN(DateTable[Month]), USERELATIONSHIP(FactFinance[DateDue],DateTable[Date]))
```

## 45. Time Intelligence Functions

### Running total

Method 1

```
AmountCalc = CALCULATE(SUM(FactFinance[Amount]), DATESYTD(FactFinance[Date]))
```
Method 2
```
AmountCalc2 = CALCULATE(SUM(FactFinance[Amount]), DATESYTD(FactFinance[Date]))
```
<img width="615" height="684" alt="image" src="https://github.com/user-attachments/assets/9e3a02c1-1216-4dbf-8a59-5ba875e1ca34" />

---

### previous time period

backtime = CALCULATE(SUM(FactFinance[Amount]), DATEADD(FactFinance[Date], -12, MONTH))

backtime2 = CALCULATE(SUM(FactFinance[Amount]), SAMEPERIODLASTYEAR(FactFinance[Date]))
       
## 46. Quiz 4: Date and Time Intelligence Functions

<img width="712" height="416" alt="image" src="https://github.com/user-attachments/assets/eca65343-a1ec-4b2b-a402-30f1dc0fe811" />

<img width="695" height="624" alt="image" src="https://github.com/user-attachments/assets/92dc60b9-ab7f-4c87-800b-b03c751bc9fa" />






## 46. Practice Activity 6

Let's practice the Time Intelligence functions. We will be using the model that we have used in the previous Practice Activities. All of the fields in this Practice Activity are from the DimCustomer table.

1. Create a Table visualization, with DateFirstPurchase in the Rows, and the count of the CustomerKey column (I will call this KeyCount). If DateFirstPurchase is shown as a hierarchy (with Year, Quarter, Month and Day), right-hand click on it in the Build pane, and click on "DateFirstPurchase" in the context menu.

2. Add a measure called YearStart to the visualization which shows the STARTOFYEAR of DateFirstPurchase.

3. Note that the results from the first three rows are not 1 January - why is this?

    Create a measure called RunningTotal to the visualization which shows a running total of KeyCount, which resets every month. (Use either TOTALMTD or CALCULATE and DATESMTD.)

    Create a measure called PrevYear to the visualization which retrieves KeyCount from the previous year. Use DATEADD, and then use SAMEPERIODLASTYEAR.

    Create another measure called CurrentMonth using the PARALLELPERIOD function to get the total for the current month. (The current month is zero months away.)





YearStart = STARTOFYEAR(DimCustomer[DateFirstPurchase])

RunningTotal = TOTALMTD(COUNT(DimCustomer[CustomerKey]), DimCustomer[DateFirstPurchase])


PrevYear = CALCULATE(COUNT(DimCustomer[DateFirstPurchase]), DATEADD(DimCustomer[DateFirstPurchase], -12, MONTH))

PrevYear = CALCULATE(COUNT(DimCustomer[DateFirstPurchase]), SAMEPERIODLASTYEAR(DimCustomer[DateFirstPurchase]))

CurrentMonth = CALCULATE(COUNT(DimCustomer[CustomerKey]), PARALLELPERIOD(DimCustomer[DateFirstPurchase], 0, MONTH))
