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




## 52. 1. Implement workspace-level access controls

Creating a new workspace called `Part 1 Level 7 Workspace` under the trial or pro licence.

In Manage Access, I am the only admin

<img width="268" height="266" alt="image" src="https://github.com/user-attachments/assets/3a5ef519-b332-4e7b-b259-bf3edf255fa3" />

We can add people in the same org with one of four level of access privileges.

<img width="362" height="452" alt="image" src="https://github.com/user-attachments/assets/c63fb6b9-338e-4451-b479-40b845a74641" />






## 53. 39. Implement incremental refresh for semantic models

This time we will use **Incremental Refresh**. 

Connect to myfirstdatabase and choose `SalesTL.Product` table and load the data.

Now, we need to create two parameters of `RangeStart` and `RangeEnd`, which we will use later for filtering dates. Go to Manage Parameters in Home tab.

The `RangeStart` parameter will have a value of 2002-01-01

<img width="406" height="663" alt="image" src="https://github.com/user-attachments/assets/09fd5fda-ee6b-4b7d-8742-b6861f79f154" />

Create `RangeEnd`

<img width="409" height="658" alt="image" src="https://github.com/user-attachments/assets/972cf9c0-1b26-4e65-bca7-c805d7216f0d" />

<img width="562" height="412" alt="image" src="https://github.com/user-attachments/assets/0b0b6631-446b-4a05-8c82-3e171f1db44e" />


Now filter SellStartDate with between

<img width="453" height="336" alt="image" src="https://github.com/user-attachments/assets/d4f092d6-4ae8-41dc-ae31-9f7833e55be9" /> <br> <br>

<img width="708" height="306" alt="image" src="https://github.com/user-attachments/assets/615a9b3a-b8bf-4766-a140-c30dd1f25e52" />

Make sure you have is after or equal to for start and is before for end, that each date only occurs in one load.

<img width="773" height="309" alt="image" src="https://github.com/user-attachments/assets/0b668d0f-13ed-4bcd-9481-8e94df0e4fe6" />

the result of the filtering is

<img width="1181" height="222" alt="image" src="https://github.com/user-attachments/assets/fb2b71d3-daf2-4c89-8f49-097839a78b67" />


Right click on the SalesLT Product table > Incremental Refresh

<img width="240" height="477" alt="image" src="https://github.com/user-attachments/assets/c19da827-4f87-4b54-9e6e-d07236cce095" />


Apply using the following settings.

<img width="522" height="926" alt="image" src="https://github.com/user-attachments/assets/c0bdb518-70de-4766-a291-11c185dd320a" />

Now save and upload the power bi report to pbi service workspace.

give the semantic model Oauth2 credential.

<img width="1055" height="593" alt="image" src="https://github.com/user-attachments/assets/f651d559-7ecf-4a33-aa9a-a739f5f39448" />

Refresh the semantic model

<img width="505" height="120" alt="image" src="https://github.com/user-attachments/assets/097f419e-0f7b-40da-ba85-63485d0a5790" />


click on **Refresh Visual** button.

<img width="613" height="238" alt="image" src="https://github.com/user-attachments/assets/5b16822a-42bd-4a61-bcaf-d1bfba9ce617" />

Your visuals will now be updated. 

<img width="830" height="345" alt="image" src="https://github.com/user-attachments/assets/008cd223-8fee-476d-8609-a8d4e47ef653" />



## 55. 2. Implement item-level access controls

To share, click on **Share**

<img width="448" height="100" alt="image" src="https://github.com/user-attachments/assets/64841518-4b1d-4976-a8b1-28c1d8b265b0" />

To manage access permission from a pbis report, click on the three dots

<img width="660" height="452" alt="image" src="https://github.com/user-attachments/assets/8b22ed5e-e5e3-43a7-a06a-d158365d3e91" />



To remove people, you have to go into advanced or go to Manage Permission

<img width="443" height="1199" alt="image" src="https://github.com/user-attachments/assets/181688f4-9896-4657-bca1-a33c8d475471" />

you will see links and who has direct access.


<img width="1259" height="204" alt="image" src="https://github.com/user-attachments/assets/2a0adfd7-81da-4cfe-8b91-e307dfb4ef5e" />











## Quiz 5: Other PL-300 exam topics needed for the DP-600 exam

<img width="829" height="1017" alt="image" src="https://github.com/user-attachments/assets/a32a73c3-f395-4b7e-9022-ab56cf1bb67e" />


<img width="840" height="513" alt="image" src="https://github.com/user-attachments/assets/17ea991c-8f02-4f15-9e4b-984675510828" />

<img width="845" height="482" alt="image" src="https://github.com/user-attachments/assets/9b050cfc-56b5-4015-a257-12d8b994e418" />

<img width="846" height="487" alt="image" src="https://github.com/user-attachments/assets/7bc14418-1971-4ed0-9cf2-581fee36c1ba" />








