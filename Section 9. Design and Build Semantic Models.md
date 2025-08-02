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
























