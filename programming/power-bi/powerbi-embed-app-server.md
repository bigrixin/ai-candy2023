---
description: >-
  This API App API (.NET Core V5.0) is used to obtain PowerBI Token and
  embedUrl.
---

# PowerBI embed app - Server

Setting steps: - Register app and Add permissions in Power BI, - Update id, key in API app.

*   [x] App registrations - (https://portal.azure.com/)

    * new web a single page app \[AAA]


*   [x] Add Security Group - Azure Active Directory

    * new Security Group \[GGG]
    * new member \[AAA] in the \[GGG]


* [x] Add Workspace Access - (https://www.powerbi.com/)
  * create new workspace
  * add access Group \[GGG]

\------------------



### Add a app registeration in Auzre portal

https://portal.azure.com/

**App registeration:**

```
1. app name: [pwbiclient] --- the name that has create in PowerBI

   web, Redirect URLs: https://pwbiclient

2. Overview->Application (client) ID and Directory (tenant) ID

Location:
	 【application id】: ******
	 【tenant id】: ******

3. get [Secret Key]
   Manage->Certificates & secrets, New client secret, Expires (select 24 months) 
```

【applicationSecret】: (value/key): \*\*\*\*\*\*

```
4. Add API permissions:
 
       Manage->API permissions->Add a permission 
         ==>select and add Power BI Service 
         ==>select Delegated permissions as below:

	App.Read.All

	Capacity.Read.All
	Capacity.ReadWrite.All

	Dashboard.Read.All
	Dashboard.ReadWrite.All

	Dataset.Read.All
	Dataset.ReadWrite.All

	Gateway.Read.All
	Gateway.ReadWrite.All

	Report.Read.All
	Report.ReadWrite.All

	StorageAccount.Read.All
	StorageAccount.ReadWrite.All

	Workspace.Read.All
	Workspace.ReadWrite.All
```

**Add Security Group in the AAD:**

```
1. Add -> new Group, 
   Group type: Security
   Group name: [PBGroup]
   Membership type: Assigned

2. Azure Active Directory-> Manage -> Groups 
   Security/PBGroup/Assigned/->Members->
   [Add member - which is a Service Principal that created by App registrations]
```

### Update parameter in .NET Core (5.0) API App

Open appsettings.json, Update:

```
"applicationId": "*********",
"tenant": "*********",
"applicationSecret": "*********",
```
