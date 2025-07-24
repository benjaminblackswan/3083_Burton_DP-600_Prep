## 32. The RELATED function

Using `LOOKUPVALUE` you have to specify the linking keys (foreign key and primary key)
```
OrganizationName = LOOKUPVALUE(DimOrganization[OrganizationName], DimOrganization[OrganizationKey],
                    FactFinance[OrganizationKey])
```

However, if the relationship already exist in the data model, then you can use `RELATED` function. The relationship must be active.

<img width="500" height="556" alt="image" src="https://github.com/user-attachments/assets/8486362e-9de1-4405-9777-58fad6149486" />

```
OrganizationName2 = Related(DimOrganization[OrganizationName])
```

