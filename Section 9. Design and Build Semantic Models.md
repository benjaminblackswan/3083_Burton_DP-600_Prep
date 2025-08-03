# 56. Creating semantic model

Open a new PBI report.  Connect to FabricData and import these four tables.

<img width="259" height="105" alt="image" src="https://github.com/user-attachments/assets/0904ac3d-128e-4b28-9621-1d29bc71ac9a" />

Our model should look like this. 

FactInternetSales and DimProduct are connected via the `Productkey` key

DimProduct and DimProductSubcategory are connected via the `DimProductSubcategory` key

DDimProductSubcategory and DimProductCategory are connected via the `ProductCategoryKey` key.

<img width="1101" height="878" alt="image" src="https://github.com/user-attachments/assets/62c038e2-4039-41d2-a782-45e4f4cf440a" />

Create a table.

<img width="313" height="120" alt="image" src="https://github.com/user-attachments/assets/cf7edfc2-6ac1-412c-b2c0-bd79b5b2c59f" />

# 57. 30. 1st Normal form

## Designing a star schema

**Bad table design type 1**

<img width="446" height="150" alt="image" src="https://github.com/user-attachments/assets/c92f4fbd-d687-4dd0-959f-882b25b2d255" />

**Bad table design type 2**

<img width="676" height="77" alt="image" src="https://github.com/user-attachments/assets/a7919a56-6d56-4640-8724-838ef238c32a" />

### First Normal Form

<img width="655" height="152" alt="image" src="https://github.com/user-attachments/assets/e23b47cd-6dd6-40ba-acda-a47692aab159" />

### Second Normal Form

<img width="474" height="582" alt="image" src="https://github.com/user-attachments/assets/92298fc4-c078-48e5-833b-cee193719e06" />


### Third Normal Form

<img width="1150" height="603" alt="image" src="https://github.com/user-attachments/assets/40ff9ce4-4acc-461a-ba7c-d2fa4b87e582" />







## 59. 30. Implementing a star schema for a semantic model

Lets first build a snowflake schema based on the **3rd normal form**

<img width="848" height="218" alt="image" src="https://github.com/user-attachments/assets/9b01b995-a2c5-44aa-9da4-354ecabaf93d" />

For database with a lot of write operations, 3rd normal form is the best. But for reporting and data analysis, the 2nd normal form (ie **star schema**) is better because of the faster read speed.

Therefore in PBI, this is better. 

<img width="615" height="235" alt="image" src="https://github.com/user-attachments/assets/132c3205-6aa4-4c69-b05e-a26a4763187f" />

## Denormalization of our PBI snowflake schema

we need to denormalise this

<img width="1001" height="304" alt="image" src="https://github.com/user-attachments/assets/152402cc-e2c4-408e-add7-888a7261a1a7" />

We do this by merging **DimProduct** with **DimProductSubcategory** using `ProductSubcategoryKey`. **Remember to use Left Outer Join**

<img width="725" height="642" alt="image" src="https://github.com/user-attachments/assets/f9b5f4c0-122a-48cf-895f-a97aa5efe95e" />

We only need the `EnglishProductSubcategoryName` and `ProductCategoryKey`

<img width="341" height="282" alt="image" src="https://github.com/user-attachments/assets/bcc91e8d-4e4b-40a7-a735-1d542df3b792" />


Now we merge **DimProduct** with **DimProductCategory** using `ProductCategoryKey`

<img width="725" height="646" alt="image" src="https://github.com/user-attachments/assets/9673ffed-cbbe-492a-8261-847ca62aa91a" />


Go to **Model View** and hide **DimProductSubcategory** and **DimProductCategory** tables

<img width="1703" height="917" alt="image" src="https://github.com/user-attachments/assets/23958a75-402f-4c6d-88dc-d6faa89d02df" />




## 61. Practice Activity Number 7

```
Let's practice what you have learned.

This practice activity uses the FabricData workbook. It is available as a resource to lecture 6, in the Data zip.

    Create a new file and load the FactInternetSales, DimCustomer, DimGeography and DimSalesTerritory spreadsheets (within the FabricData workbook).

    Ensure that the following relationships have been added and are active, and delete any other relationships:

        FactInternetSales to DimCustomer, using the CustomerKey field.

        DimCustomer to DimGeography, using the Geography field.

        DimGeography to DimSalesTerritory, using the SalesTerritoryKey field.

Let's then implement a star schema for our semantic model.

    In the table DimCustomer, create two new calculated columns which contains:

        the City from the table DimGeography, and

        the SalesTerritoryCountry from the table DimSalesTerritory.

    Then hide the DimGeography and DimSalesTerritory tables.

    Create a matrix, with:

        In the rows - SalesTerritoryCountry from the table DimCustomer, and

        In the columns - DueDate.Year from the table FactInternetSales.

        In the values - the calculated measure SalesAmountTotal from the table FactInternetSales.
```

Load them in and promote headers and change data type.

<img width="1780" height="490" alt="image" src="https://github.com/user-attachments/assets/6dbc4c8b-dd2f-43cf-96bf-ba9d64a250fa" />

Create relationship like this

<img width="868" height="320" alt="image" src="https://github.com/user-attachments/assets/183f3ea2-987e-40fd-a464-0af1b9aa8ab3" />


<img width="1251" height="825" alt="image" src="https://github.com/user-attachments/assets/a7d0ee85-8971-4922-8f01-24616aeac4ed" />


### Denormalisation

We are going to use DAX instead of PQ.


Because this is from Many to one table, we use **RELATED**

```
City = RELATED(DimGeography[City])

SalesTerritoryCountry = Related(DimSalesTerritory[SalesTerritoryCountry])
```

Then we hide the **DimGeography** and **DimSalesTerritory** tables to make it Star Schema.

<img width="1258" height="856" alt="image" src="https://github.com/user-attachments/assets/d75ce37d-4c6d-49cf-9682-13956c05e98e" />

Create a matrix with the `SalesTerritoryCountry` calculated column we just created.

# Quiz 6: Design and build semantic models

<img width="543" height="447" alt="image" src="https://github.com/user-attachments/assets/0632e352-aa6d-43a3-8523-8a645bbe2be5" />

<img width="532" height="430" alt="image" src="https://github.com/user-attachments/assets/a814bfa1-1abe-4bba-ab74-648d94bfc113" />

