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


## Play 21. Practice Activity Number 2 - The Solution

```
LocationName = DimGeography[StateProvinceName] & ", " & DimGeography[EnglishCountryRegionName]
IsUS = if(DimGeography[CountryRegionCode] = "US", "In US", "Outside of US")
IsTexasOrCalifolia = if(DimGeography[StateProvinceCode] = "CA" || DimGeography[StateProvinceCode] = "TX", "Yes", "No")
Continent = SWITCH(DimGeography[CountryRegionCode], "US", "North America", "CA", "North America", "GB", "Europe", "FR", "Europe", "DE", "Europe", "AU", "Oceania", "Unknown")
NewKey = DimGeography[SalesTerritoryKey] - 1
Division = divide(DimGeography[GeographyKey], DimGeography[NewKey])
```

