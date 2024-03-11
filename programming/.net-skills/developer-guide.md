---
description: Code first update model and deploy
---

# Developer Guide

#### Bulid database

1. Delete database MAF\_dev (if existed)
2. At Package Manager Console (generate tables)

&#x20;   (a). Select MyAbilityFirst.Infrastructure.Auth

```
   update-database     ==> (create asp.net tables)
```

&#x20;   (b). Select MyAbilityFirst.Infrastructure.Data

```
   updata-database     ==> (create all other tables)
```

3\. Select MyAbilityFirst.Infrastructure.Data\SQL

&#x20;    a. Select seed.sql \
&#x20;    b. Connect SQL Server(local or Azure), Select database name: MAF\_dev \
&#x20;    c. Execute

#### Add or Change model

get-migrations (check migrations status) \
get-help entityframework (help)

#### _Add new table (model)_

At \MyAbilityFirst.Infrastructure.Data

1. In the class MyAbilityFirstDbContext Add: (for example) public DbSet UserAttachments { get; set; }
2. In the OnModelCreating(DbModelBuilder modelBuilder) Add:(for example) modelBuilder.Entity().ToTable("UserAttachment");

#### _Change table (model)_

At \MyAbilityFirst.Infrastructure.Data

add-migration name: AddUrltoTablePicture (for example)

<\<Change anything in up() function which generated>>

update-database

#### _Delete bad Migration_

At \MyAbilityFirst.Infrastructure.Data

Update-database -TargetMigration: \[last good migration name]

Select bad migration, mouse right key, Exclude from project (or delete bad in VS and remove code in .csproj)

update-database

Check \_MigratonHistory table and .csproj

#### _Update_

Add-Migration \[full name of migration] -Force

#### _Merge two branch_

At migration folder, select and add some migration have not include in VS.

1. Update-database -TargetMigration: \[second last good migration name]
2. Exclude new migrations from project
3. Include one by one migration Add-Migration \[full name of migration]
4. Finally, update-database

#### Deploy to Azure

1. In VS, Open Solution Exploer window
2. Select defalut project
3. Mouse right key menu, select Publish
4. At Profile tag, select Microsoft Azure App Service as publish target
5. Publish

#### \*\*\* Do not forget to change "connectionString" for local or Azure \*\*\*
