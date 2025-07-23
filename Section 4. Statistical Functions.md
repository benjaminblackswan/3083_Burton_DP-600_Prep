# Section 4. Statistical Functions

## 22. 21. Measures - an introduction, with standard aggregations including Countblank

Create a measure that count number of blanks

<img width="155" height="143" alt="image" src="https://github.com/user-attachments/assets/173a7795-9837-4a12-9129-6d6cfd26558f" />

```
AmountActualBlanks = COUNTBLANK(FactFinance[AmountActual])
```

you can create measures instead of using aggregations, ie instead of using

<img width="259" height="448" alt="image" src="https://github.com/user-attachments/assets/4fe6f745-612d-4d7e-a6bd-cfa40d7206e0" />

you can create a measure instead.

```
AmountActualSum = sum(FactFinance[AmountActual])
```

Implicit Measure vs. Explicit Measure

https://radacad.com/explicit-vs-implicit-dax-measures-in-power-bi/

the measure that you create from DAX is called **Explicit Measure**

what are some reasons you might want to use explicit measures? When you want to control what the end users can see, you might not want the end users to control the aggregation, or you want to hide the original column.

## 23. 32a. Aggregation of calculations (iterator functions)

All iterator functions have X after it, eg SUMX. there were originally called eXtended functions.

<img width="837" height="621" alt="image" src="https://github.com/user-attachments/assets/8b3d9543-533c-4535-b521-e2dd0e75839f" />

**why do we need iterator functions**

say for example you want to add $1 to **Amount** before sum it up, you can not do that with a normal aggregation function `SUM`

<img width="516" height="81" alt="image" src="https://github.com/user-attachments/assets/7b7f9472-b844-4b2e-ad6f-e468d7a41fcb" />

So how can you create a Measure that lets you add $1 to **Amount** before sum it up? There are two methods.
1. Using a helper column and then use an aggregate function.
2. Using an iterator function.

### Using a helper column and then use an aggregate function.

columns created in Power Query are called **Computed Columns**, columns created with DAX are called Calculated Columns, see https://www.sqlbi.com/articles/comparing-dax-calculated-columns-with-power-query-computed-columns/

_The choice of a DAX calculated column should be limited to cases where the result is obtained by accessing data in different rows of the same table or in different tables; you should however choose a Power Query computed column when the business logic to implement relies on the values of other columns of the same table._

create a new column called `AmountActualPlus1`
```
AmountActualPlus1 = FactFinance[AmountActual] + 1
```

Then create a new called `AmountActualPlus1Sum`

```
AmountActualPlus1Sum = sum(FactFinance[AmountActualPlus1])
```

So in this method, you created a new Measure with the "help" of the helper column AmountActualPlus1`

<img width="192" height="495" alt="image" src="https://github.com/user-attachments/assets/be6451e6-5bdb-4317-930e-0261ade6e80c" />

### create a measure using iterator function

```
AmountActualPlus1Sum = sumx(FactFinance, FactFinance[AmountActual] + 1)
```

## 24. Other statistical functions

```
Rank = rank.eq(FactFinance[Amount], FactFinance[Amount], asc)
```

<img width="1172" height="193" alt="image" src="https://github.com/user-attachments/assets/d0d8c1c6-fcdc-444b-b7f5-ff787ffac154" />

## Quiz 2: Statistical functions

<img width="923" height="584" alt="image" src="https://github.com/user-attachments/assets/6a34a6d7-b5aa-43d1-881f-629a7b7a9aba" />

<img width="930" height="664" alt="image" src="https://github.com/user-attachments/assets/f3ef7e5a-94d1-4ad6-bac0-6579032bfc61" />

<img width="910" height="700" alt="image" src="https://github.com/user-attachments/assets/b430ed6c-e2c4-41c3-9d17-235d8777e31b" />

## Practice Activity Number 3

Let's practice creating measures.

First of all, let's create a series of calculations using helper columns. Then, we'll recreate this calculation as a measure, without using any of the helper columns.

Finally, we'll have a look at the RANK.EQ function.

    Open up the model that you have created in previous Practice Activities.

    Please add a Calculated Column called ProcessingFeeColumn who calculates the processing fee for each location.

        For the United States, the processing fee is $1.

        For Canada, the processing fee is $2.

        For other locations, the processing fee is $5.

    Then create a visual which calculates the average of the ProcessingFeeColumn.

    Having done this, please create a Measure called ProcessingFeeAverage which calculates the average processing fee, without using the ProcessingFeeColumn helper column. Then add this into the visual.

    Finally, please create a calculated column called RankEQ which calculates the RANK.EQ of the SalesTerritoryKey column. Please arrange it so that the highest SalesTerritoryKey is number 1.

### Solution

```
ProcessingFeeColumn = SWITCH(DimGeography[CountryRegionCode], "US", 1, "CA", 2, 5)
```

```
ProcessingFeeAverage = AVERAGEX(DimGeography, (SWITCH(DimGeography[CountryRegionCode], "US", 1, "CA", 2, 5)))
```

<img width="658" height="466" alt="image" src="https://github.com/user-attachments/assets/41a0b8bf-faed-4c89-a2e9-f7eb6f040e27" />







