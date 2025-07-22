## Practice Activity 1

It's time for your first Practice Activity.

    If you haven't already downloaded the workbook "FabricData", please do so. It is included in the zipped file from the Resources lecture in Section 1 of this course.

    Open up Power BI, and in a new Power BI file use this workbook to "Get Data" from the spreadsheet "DimGeography".

    Create a stacked column chart and resize it so that it fills the page.

    With that new visual selected, drag the column EnglishCountryRegionName (in the DimGeography table) from the Data pane to the X-axis and the Y-axis in the Build pane.

    Save this page to your local hard drive, and publish it to the Power BI service.

Good luck. If you get stuck, then please watch the next video, when I will do this Practice Activity as well.



## 18. The SWITCH function
```
Location = if (FactFinance[OrganizationKey] < 8, "US", "Other")
```


## 19. Other logical functions, together with the DIVIDE function


I want to create a calculated column, which doubles the Employee[Bonus] column.

I want this column to be called DoubledBonus.

What is the DAX formula I should use?

DoubleBonus = Employee[Bonus] * 2

---
<img width="958" height="560" alt="image" src="https://github.com/user-attachments/assets/39e63611-298a-45d9-9bcb-8977431340a1" />

<img width="1009" height="607" alt="image" src="https://github.com/user-attachments/assets/3dc77adc-1ea8-4ce1-aae1-e69cd1cd3895" />

<img width="954" height="658" alt="image" src="https://github.com/user-attachments/assets/348b9cf7-7de7-4d57-bb89-2507f2018f67" />



Practice Activity Number 2

Let's practice these initial functions. We will be using the Model that you developed in the previous Practice Activity.

Create the following calculated columns:

    A column called LocationName which combines:

        the StateProvinceName,

        a string literal containing a comma and a space, and

        the EnglishCountryRegionName.

    A column called IsUS which shows:

        "In US" if CountryRegionCode is "US", and

        "Outside of US" if it doesn't.

    A column called IsTexasOrCalifornia which shows "Yes" if StateProvinceCode is "CA" or "TX", and "No" if it doesn't..

    A column called Continent which shows

        "North America" if CountryRegionCode is "US" or "CA",

        "Europe" if CountryRegionCode is "GB", "FR" or "DE",

        "Oceania" if CountryRegionCode is "AU",

        and "Unknown" otherwise.

        Use the SWITCH function.

    A column called NewKey which subtracts one from the SalesTerritoryKey.

    A column called Division which divides GeographyKey into NewKey. If the answer is an error, then the answer should be blank.

Please save the Model developed in this Practice Activity. We will be using it in later Practice Activities.


## 21. Practice Activity Number 2 - The Solution

```
LocationName = DimGeography[StateProvinceName] & ", " & DimGeography[EnglishCountryRegionName]
IsUS = if(DimGeography[CountryRegionCode] = "US", "In US", "Outside of US")
IsTexasOrCalifolia = if(DimGeography[StateProvinceCode] = "CA" || DimGeography[StateProvinceCode] = "TX", "Yes", "No")
Continent = SWITCH(DimGeography[CountryRegionCode], "US", "North America", "CA", "North America", "GB", "Europe", "FR", "Europe", "DE", "Europe", "AU", "Oceania", "Unknown")
NewKey = DimGeography[SalesTerritoryKey] - 1
Division = divide(DimGeography[GeographyKey], DimGeography[NewKey])
```

# Section 4. Statistical Functions

## 22. 21. Measures - an introduction, with standard aggregations including Countblank

Create a measure that count number of blanks

<img width="275" height="143" alt="image" src="https://github.com/user-attachments/assets/173a7795-9837-4a12-9129-6d6cfd26558f" />

```
AmountActualBlanks = COUNTBLANK(FactFinance[AmountActual])
```

you can create measures instead of using aggregations, ie instead of using

<img width="399" height="448" alt="image" src="https://github.com/user-attachments/assets/4fe6f745-612d-4d7e-a6bd-cfa40d7206e0" />

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

<img width="1337" height="621" alt="image" src="https://github.com/user-attachments/assets/8b3d9543-533c-4535-b521-e2dd0e75839f" />

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

<img width="100" height="4950" alt="image" src="https://github.com/user-attachments/assets/49009e02-f625-450e-867d-e7d0012c6164" />



































