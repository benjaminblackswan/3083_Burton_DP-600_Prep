## 27. 31a. Getting additional tables and creating relationships

## 28. 31a. Inactive relationships and cross-filter direction

single cross filter direction is much faster than both directions


## 28. 31a. Inactive relationships and cross-filter direction

```
Divide = 4/0
Divide = iserror(4/0)
Divide = if(iserror(4/0),blank(), 4/0)

```

```
Divide = iferror(4/0, blank())
```

## 29. 32d. Using information functions

```
OrganizationName = LOOKUPVALUE(DimOrganization[OrganizationName], DimOrganization[OrganizationKey],FactFinance[OrganizationKey])
```


## 30. Practice Activity Number 4

1. Let's develop the model that we created in previous Practice Activities.
   - From the workbook "FabricData", "Get Data" from the spreadsheet "DimSalesTerritory".
   - If a relationship has been created between the DimSalesTerritory and DimGeography tables, check the relationship between these two tables.
   - If a relationship has not been created, please create it using the SalesTerritoryKey columns.


2. Create a calculated column in the table DimGeography called Group, which:
   - retrieves the SalesTerritoryGroup from DimSalesTerritory, using the SalesTerritoryKey.
  
3. Then use this new Group column to create another calculated column in the table DimGeography called Country.
   - This new column retrieves the SalesTerritoryCountry from DimSalesTerritory, using the SalesTerritoryGroup column from DimSalesTerritory and the new Group column.
   - This results in an error. Use IFERROR to replace any errors with a Blank.
     
3. Where do you have a non-blank value in the Country column?

### solution

```
Group = LOOKUPVALUE(DimSalesTerritory[SalesTerritoryGroup], DimSalesTerritory[SalesTerritoryKey], DimGeography[SalesTerritoryKey])
```

```
Country = iferror(LOOKUPVALUE(DimSalesTerritory[SalesTerritoryCountry], DimSalesTerritory[SalesTerritoryGroup], DimGeography[Group]), blank())
```

```
Country = LOOKUPVALUE(DimSalesTerritory[SalesTerritoryCountry], DimSalesTerritory[SalesTerritoryGroup], DimGeography[Group], blank())
```



    
