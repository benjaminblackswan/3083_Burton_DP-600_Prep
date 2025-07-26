## 32. The RELATED function

Using `LOOKUPVALUE` you have to specify the linking keys (foreign key and primary key)
```
OrganizationName = LOOKUPVALUE(DimOrganization[OrganizationName],
                                DimOrganization[OrganizationKey],
                                FactFinance[OrganizationKey])
```

However, if the relationship already exist in the data model, then you can use `RELATED` function. The relationship must be active.

<img width="500" height="556" alt="image" src="https://github.com/user-attachments/assets/8486362e-9de1-4405-9777-58fad6149486" />

```
OrganizationName2 = Related(DimOrganization[OrganizationName])
```

**equivalent SQL is**

```
select OrganizationName
from FactFinance f
left  join DimOrganization o
on f.OrganizationKey = o.OrganizationKey
```

as you can see, we get the same result
<img width="1217" height="297" alt="image" src="https://github.com/user-attachments/assets/ae3b8d16-230e-4b17-871f-cca08118a2c9" />

However if we delete this relationship, then the formula will not work. 
<img width="554" height="560" alt="image" src="https://github.com/user-attachments/assets/7c194c0a-3216-43f0-bf0a-42fdb60225bd" />

<img width="1514" height="352" alt="image" src="https://github.com/user-attachments/assets/d7ee53be-cde1-45f9-85b0-2b3d38b88e78" />

However, `LOOKUPVALUE` function still works because it does not use any relationships.

You can append more things to the DAX

```
OrganizationName2 = Related(DimOrganization[OrganizationName]) & " (" & FactFinance[ScenarioKey] & ")"
```


```
OrganizationName2 = Related(DimOrganization[OrganizationName]) & " (" & FactFinance[ScenarioKey] & ")"
```

```
OrganizationName2 = Related(DimOrganization[OrganizationName]) & " (" & RELATED(DimOrganization[CurrencyName]) & ")"
```

**the purpose of using `RELATED` is that it can be used to hide another table.**











## 33. Using the RELATEDTABLE and COUNTROWS functions

### Sum

*RELATED is one of the most commonly used DAX functions. You use RELATED when you are scanning a table, and within that row context you want to access rows in related tables. RELATEDTABLE is the companion of RELATED, and it is used to traverse relationships in the opposite direction.*

https://www.sqlbi.com/articles/using-related-and-relatedtable-in-dax/

from Many to One, use `RELATED`
from One to Many, use `RELATEDTABLE`

<img width="665" height="383" alt="image" src="https://github.com/user-attachments/assets/1a279b26-51ed-47a0-b85e-1e43afbc1a80" />

```
Answer = SUMX(RELATEDTABLE(FactFinance), FactFinance[Amount])
```

what is dax is doing is that is it aggregating (summing up) all the Amount column from the FactFinance table for the OrganizationName.

See it is travelling in the opposite direction

<img width="445" height="610" alt="image" src="https://github.com/user-attachments/assets/3dad0b9e-a214-4d95-bb1e-434811240733" />


**SQL equivalent**
```
select o.OrganizationKey, sum(amount)
from FactFinance f
left  join DimOrganization o
on f.OrganizationKey = o.OrganizationKey
group by o.OrganizationKey
order by o.OrganizationKey
```

This is basically group by.



---
### Count

We can also use `COUNTX` to count all the rows for that `OrganisationName'. 

```
Answer = COUNTX(RELATEDTABLE(FactFinance), FactFinance[Amount])
```

for example, there are 5127 Northeast Divisions in the FactFinance table, this number include blank rows.

<img width="776" height="388" alt="image" src="https://github.com/user-attachments/assets/479e5f75-47f1-42aa-b934-451c27902b9a" />


If we don't want to count the blank rows, we change the column name from Amount to AmountActual.

```
Answer = COUNTX(RELATEDTABLE(FactFinance), FactFinance[AmountActual])
```

<img width="789" height="380" alt="image" src="https://github.com/user-attachments/assets/64eaf797-ed63-4f45-bad6-0d8387466b4f" />

We can also use `COUNTROWS`, which gives the same result as `COUNTX(RELATEDTABLE(FactFinance), FactFinance[Amount])

```
Answer = COUNTROWS(RELATEDTABLE(FactFinance))
```
<img width="741" height="364" alt="image" src="https://github.com/user-attachments/assets/4571e357-0e4e-4574-a028-5f258988e88e" />

**SQL equivalent**
```
select o.OrganizationKey, count(amount)
from FactFinance f
left  join DimOrganization o
on f.OrganizationKey = o.OrganizationKey
group by o.OrganizationKey
order by o.OrganizationKey
```

## 34. Context

https://www.sqlbi.com/articles/filter-context-in-dax/

<img width="880" height="458" alt="image" src="https://github.com/user-attachments/assets/13f99bff-19aa-456f-9664-278971c966c8" />

**create a temp table in SQL Server**
```
drop view if exists financeorg;
Go
create view financeorg
as (
select o.OrganizationKey OrgKey
, OrganizationName OrgName
, Date
, Amount
from DimOrganization o
full outer join FactFinance f
on f.OrganizationKey = o.OrganizationKey);
go
```

### With filter context

```
Answer = SUMX(RELATEDTABLE(FactFinance), FactFinance[Amount])
```

**In SQL, this would be simply a group by statement**

```
select o.OrganizationKey, sum(amount)
from DimOrganization o
left  join FactFinance f
on f.OrganizationKey = o.OrganizationKey
group by o.OrganizationKey
order by o.OrganizationKey
```

or

```
select orgkey, sum(amount)
from financeorg
group by orgkey
order by orgkey
```


<img width="249" height="334" alt="image" src="https://github.com/user-attachments/assets/2d26b07a-a1ee-46d0-8fc8-12a8f7c73e53" />

---

### without filter context
```
answer 2 = sumx(FactFinance, FactFinance[Amount])
```

The second dax does not have filter context, it simply adds up everything.

**The SQL equivalent one with over() clause**

```
select o.OrganizationKey, sum(sum(amount)) over ()
from DimOrganization o
left  join FactFinance f
on f.OrganizationKey = o.OrganizationKey
group by o.OrganizationKey
order by o.OrganizationKey
```
or

```
select orgkey, sum(sum(amount)) over()
from financeorg
group by orgkey
order by orgkey
```

<img width="252" height="310" alt="image" src="https://github.com/user-attachments/assets/d45edbee-fef6-4021-a8ba-53163d4637b3" />

### Iterative function vs. Aggregate function

note that the following two measures will give the same result.

answer 2 is a iterative function and answer 3 is an aggregate function. Answer 3 simply adds everything in the column up.

```
answer 2 = sumx(FactFinance, FactFinance[Amount])
answer 3 = sumx(FactFinance, FactFinance[Amount])
```
answer 2 is more flexible because you can add additional things in the expression part eg

answer 4 = sumx(FactFinance, FactFinance[Amount] + 1)

### Calculate percentage

answer 3 = DimOrganization[Answer] / DimOrganization[answer 2]

**SQL**
```
select orgkey, format(sum(amount)/sum(sum(amount)) over(),'P') as percentage
from financeorg
group by orgkey
order by orgkey
```







## 35. The ALL function
```
PercentOfTotal = SUMX(FactFinance, FactFinance[Amount])
```

<img width="629" height="264" alt="image" src="https://github.com/user-attachments/assets/1259cacd-c37e-423f-9235-da691fcd78b4" />







The ALL functions ignores all filter context
```
PercentOfTotal = SUM(FactFinance[amount]) / SUMX(all(FactFinance),FactFinance[amount])
```
<img width="402" height="256" alt="image" src="https://github.com/user-attachments/assets/7e8c09bb-ad29-4c0b-9f68-6a894947de53" />























## 36. The FILTER function

```
Amount2028 = sumx(filter(FactFinance, FactFinance[date].[Year] = 2028), FactFinance[Amount])
```

<img width="226" height="271" alt="image" src="https://github.com/user-attachments/assets/25da21a8-667f-44b9-bcee-b8ff9568b9bd" />















## 37. The CALCULATE function

Calculate function puts the filter at the end, for example you can change the `PercentOfTotal` from

```
PercentOfTotal = SUM(FactFinance[amount]) / SUMX(all(FactFinance),FactFinance[amount])
```

To

```
PercentOfTotal = SUM(FactFinance[amount]) / calculate(SUMX(FactFinance,FactFinance[amount]),all(factfinance))
```

<img width="393" height="280" alt="image" src="https://github.com/user-attachments/assets/dada9373-de3c-42e9-b0e0-07308e487a57" />


---

```
Amount2028 = sumx(filter(FactFinance, FactFinance[date].[Year] = 2028), FactFinance[Amount])
```

To

```
Amount2028 = calculate(sumx(FactFinance, FactFinance[Amount]), filter(FactFinance, FactFinance[Date].[Year] = 2028))
```

<img width="229" height="287" alt="image" src="https://github.com/user-attachments/assets/ddfd054c-0e8d-42a2-8f3f-5b15c6e97755" />


<img width="1385" height="541" alt="image" src="https://github.com/user-attachments/assets/f203cbda-b8be-4cc2-be3d-c089afb16d98" />

















## 38. The ALLEXCEPT function

Returns all the rows in a table or all the values in a column, ignoring all context filters applied but taking into account the specified columns filter.

```
ActualAllExcept = CALCULATE(sum(FactFinance[Amount]), ALLEXCEPT(FactFinance, DimOrganization[OrganizationName], FactFinance[Date].[Year]))
```

<img width="1124" height="660" alt="image" src="https://github.com/user-attachments/assets/be03873c-7e7c-4589-93d3-5246694296cf" />




























## 39. The ALLSELECTED function

*Returns all rows in a table, or all the values in a columns, ignoring any filters that might have been applied inside the query, but keeping filters that come from outside.*

*The ALLSELECTED function gets the context that represents all rows and columns in the query, while keeping explicit filters and contexts other than row and column filters. This function can be used to obtain visual totals in queries.*

```
ActualAll = CALCULATE(sum(FactFinance[Amount]), all(FactFinance))
ActualAllExcept = CALCULATE(sum(FactFinance[Amount]), allexcept(FactFinance, DimOrganization[OrganizationName], FactFinance[Date].[Year]))
```

<img width="824" height="827" alt="image" src="https://github.com/user-attachments/assets/fcd552d4-3e91-4d17-9e61-516a7b23efd3" />

---
```
ActualAllSelected = CALCULATE(sum(FactFinance[Amount]), ALLSELECTED(FactFinance))
```
Since there is no column inside `ALLSELECTED(FactFinance)`, this dax ignores all column filter context in the visual. In this case, the number is the grand total.

<img width="476" height="796" alt="image" src="https://github.com/user-attachments/assets/ea39ad51-9c56-46cb-9aa2-e710f5e32674" />

---
By adding a  slicer on ScenarioKey, we introduced an explicit filter. This explicit filter will act on Sum of Amount and ActualAllSelected, but it will not act on ActualALL because that measure uses ALL() function, which ignores all filters, that is both visual and explicit filters.

<img width="1137" height="871" alt="image" src="https://github.com/user-attachments/assets/96b67c00-8087-42bf-9a9a-65a0f4a657ec" />

---
lets put ScenarioKey inside ALLSELECTED, this will remove add the Scenariokey to the ignore list, so the measure will return values ignoring scenariokey in the visual.

```
ActualAllSelected = CALCULATE(sum(FactFinance[Amount]), ALLSELECTED(FactFinance[ScenarioKey]))
```

<img width="628" height="381" alt="image" src="https://github.com/user-attachments/assets/c83008fc-3ef0-4414-a39a-48dbdf54fa2e" />




## 40. Other filter functions

## Quiz 3: Filter and Value Functions

<img width="728" height="677" alt="image" src="https://github.com/user-attachments/assets/48f05116-18d5-4115-980e-e793f0052331" />

<img width="711" height="590" alt="image" src="https://github.com/user-attachments/assets/58663b6e-6f3f-4fc4-b860-8453111f0bf8" />

<img width="706" height="516" alt="image" src="https://github.com/user-attachments/assets/ef7c8e85-12d3-40e0-bbee-1bf37b01fce9" />

<img width="706" height="553" alt="image" src="https://github.com/user-attachments/assets/20c3b282-dbd5-41e4-9ffc-566d6fe04c28" />



## 41. Practice Activity 5

Practice Activity 5

Let's practice our Filter and Value Functions. We will using the model we have developed during previous Practice Activities.

1. From the workbook "FabricData", "Get Data" from the spreadsheet "DimCustomer".
   - If a relationship has been created between the DimCustomer and DimGeography tables, check the relationship between these two tables.
   - If a relationship has not been created, please create it using the GeographyKey columns.
   - In the Model view, change the relationship between DimGeography and DimCustomer  to "Cross filter direction: Both".
  <img width="951" height="710" alt="image" src="https://github.com/user-attachments/assets/01d7ca85-e1fa-4781-a0dd-b5b0d35e0771" />


2. In the DimCustomer table, use the RELATED function to create a new Calculated Column called Region, which retrieves the SalesTerritoryRegion column from the DimSalesTerritory table.
```
Region = Related(DimSalesTerritory[SalesTerritoryRegion])
```
  
3. In the DimSalesTerritory table, use the RELATEDTABLE and COUNTROWS functions to create a new Calculated Column called NumberOfCustomers. It should count the number of relevant rows in the DimCustomer table.
   
```
NumberOfCustomers = countrows(RELATEDTABLE(DimCustomer))
```

4. Create a matrix visualization which includes:
   - SalesTerritoryGroup and SalesTerritoryRegion from DimSalesTerritory in the Rows,
   - Gender from DimCustomer in the Columns, and
   - a count of CustomerKey from DimCustomer.
   - Then expand the visualization so you can see both SalesTerritoryGroup and SalesTerritoryRegion.

5. Use the CALCULATE and FILTER functions to create a measure called NumberSingle.
   - It should calculate the count of CustomerKey where the MaritalStatus in DimCustomer is 'S' (which means Single).
   - Add it into the Values section of the visual, and then replace the Gender column with the MartialStatus column.
   ```
    NumberSingle = CALCULATE(count(DimCustomer[CustomerKey]), FILTER(DimCustomer, DimCustomer[MaritalStatus] = "S"))
   ```
     
6. Create a similar measure called GrandTotal which uses the ALL function, and add it into the Values section of the visual.
   
   ```
    GrandTotal = CALCULATE(count(DimCustomer[CustomerKey]), all())
   ```
   <img width="886" height="446" alt="image" src="https://github.com/user-attachments/assets/e93b034e-c436-41f0-9f9f-90dfbc51da95" />

8. Create a measure called **NumberContinent** which uses the ALLSELECTED function to remove the impact of SalesTerritoryRegion. Then add it into the Values section of the visual.

```   
NumberContinent1 = CALCULATE(COUNT(DimCustomer[CustomerKey]), ALLSELECTED(DimSalesTerritory[SalesTerritoryGroup]))
```

```   
NumberContinent2 = CALCULATE(COUNT(DimCustomer[CustomerKey]), ALLEXCEPT(DimSalesTerritory, DimSalesTerritory[SalesTerritoryGroup]))
```

