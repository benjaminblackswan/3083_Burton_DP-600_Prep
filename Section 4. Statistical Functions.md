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
Rank = rank.eq(FactFinance[Amount], FactFinance[Amount], asc)

