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





## 35. The ALL function










## 36. The FILTER function













## 37. The CALCULATE function







## 38. The ALLEXCEPT function














## 39. The ALLSELECTED function
