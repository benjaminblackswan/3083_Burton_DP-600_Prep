# 48. 36, 37. Performance improvements to report visuals and DAX

We can use Performance analyzer under **Optimize** tab.

<img width="755" height="286" alt="image" src="https://github.com/user-attachments/assets/a468903a-099f-4b79-94c5-2c2a1e408c3c" />

bi-directional Filtering will take more time.

<img width="307" height="176" alt="image" src="https://github.com/user-attachments/assets/542e28bd-3944-41e6-945e-182176af79a2" />


# 49. 29. Choose a storage mode

Go to https://portal.azure.com

<img width="917" height="650" alt="image" src="https://github.com/user-attachments/assets/adddb3d9-2c31-4af7-a687-44181c824119" />

We will see something called SQL Server and SQL Database. Just like on-prem, SQL database is a db within a SQL Server.

<img width="746" height="103" alt="image" src="https://github.com/user-attachments/assets/5d10800e-dbe7-45c5-8d89-490f9744ce61" />



Click on myfirstdatabase and use SQL Server authentication, the password is the one I set when I created this db. Both authentication method should work but I find the Entra ID is less reliable, gives error alot.

<img width="814" height="466" alt="image" src="https://github.com/user-attachments/assets/4b89684c-9f46-4efe-8346-cc71cf4f788d" />


**to query the Azure SQL db**

use Query Editor
<img width="289" height="252" alt="image" src="https://github.com/user-attachments/assets/24b82f19-fb2f-4fe4-93c8-469a8b7fc8fc" />




### Connect Power BI Desktop to Azure SQL DB

Copy the SQL Server name, which you will need to for the connection.

<img width="1390" height="224" alt="image" src="https://github.com/user-attachments/assets/29983f28-f2e7-4d01-9bbe-5d1b9da64c02" />

Open a new Power Bi Desktop file and choose SQL Server for data source.

<img width="166" height="277" alt="image" src="https://github.com/user-attachments/assets/5bdae012-ef05-4678-bf95-26d7c9320a29" />

and paste in the SQL Server name and **choose between Import mode vs. DirectQuery mode**. Import has faster visual rendering but will not have the latest data. DirectQuery will have the latest data but every visual change means it has to fetch the data from the source so it is a lot slower.

We are going to choose **Import** mode.

<img width="705" height="342" alt="image" src="https://github.com/user-attachments/assets/faa4c7d9-db03-4e43-9edf-d43f8089934c" />

Here you will see all the db in `myfirstserverben`, currently there is only one db. 

<img width="548" height="270" alt="image" src="https://github.com/user-attachments/assets/2d445cec-ad1f-405e-a4d9-740987b746af" />

we are going to choose the `salesLT.Address` table.

<img width="674" height="718" alt="image" src="https://github.com/user-attachments/assets/435e52d7-2025-497f-9df9-549a8d1cfbc7" />

**clean the data if necessary in PQ** such as promote first row to header, change data type etc.

In **Model View**, you can see the Storage Mode is Import, we can not change Import mode back to DirectQuery mode.

<img width="776" height="812" alt="image" src="https://github.com/user-attachments/assets/a8519016-2cf4-43aa-8811-25a3ef0df175" />



# 50. 36. Improve query performance by using query folding

### Check Query Folding

First lets **merge** City and StateProvince columns (Add Column > Merge Columns)

If we right click on the Applied Steps and if we can choose **View Native Query**, then that means the step is Query Folder, you can see the SQL.

<img width="274" height="387" alt="image" src="https://github.com/user-attachments/assets/9b009f3c-4d00-4df2-8de2-6d945c690d71" /> <br><br>
<img width="555" height="218" alt="image" src="https://github.com/user-attachments/assets/bc21a56f-2d62-4bc8-a5f9-0b4147ff165e" />

Now add an index (Add column > Index Column > From 1) and you see that the View Native Query is grey out.

<img width="269" height="313" alt="image" src="https://github.com/user-attachments/assets/e9402342-6cdf-4c8f-a8c0-b7b11541b5c9" />

What this mean is that SQL Server does the transformation from step 1 to 4 and provides the result in step 4 to Power Query.

<img width="383" height="360" alt="image" src="https://github.com/user-attachments/assets/ea86499e-9846-4c0c-9007-e3060377cd5b" />


**Create a new workspace in Fabric online**

and upload the power bi file into 3083 - Burton, [DP-600 Prep **workspace**](https://app.fabric.microsoft.com/groups/fd0057e2-9a8e-4058-84fd-47b80cab783b/list?experience=power-bi)

Go to the workspace and look for 3083 workspace

<img width="322" height="294" alt="image" src="https://github.com/user-attachments/assets/b64c9495-7904-4579-b760-efa57ab0fc30" />

Once you are in the 3083 workspace, look for the Power Bi semantic model and report we just uploaded.

<img width="1645" height="94" alt="image" src="https://github.com/user-attachments/assets/ba88a74c-5ad3-4d0e-b234-0c410550fcd1" />

Click on semantic model and click on Explore.

<img width="287" height="218" alt="image" src="https://github.com/user-attachments/assets/fbf068a8-4aba-41ed-ad4a-e18e8a4ad240" />

## Explore This Data

allowes user to use bascially the **Power bi online** version.

<img width="1473" height="964" alt="image" src="https://github.com/user-attachments/assets/0415a203-4568-44df-a618-e3b4d5949b27" />

## Auto-create a report

<img width="1163" height="866" alt="image" src="https://github.com/user-attachments/assets/63b5a02e-6967-421a-85be-408ae04762b8" />





































