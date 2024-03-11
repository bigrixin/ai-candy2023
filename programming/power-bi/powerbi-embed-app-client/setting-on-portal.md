# Setting on portal

Display (Embed) Power BI report in Angualr UI

## &#x20;Create report in PowerBI

refer: https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers https://www.youtube.com/watch?v=0y2oJikC6Xc

Power BI web portal: https://app.powerbi.com/

***

### Create workspace

1.  Create workspace

    Workspace->Create a workspace->New --> name\[wiseclient] Enable access -> from the More menu, select Workspace access. --> PowerBIGroup ->select Admin ->Add
2. Update Credentials:\
   Report, mouse right, \[Setting]: clicked on Edit Credentials and simply pressed Sign-in.
3. Change referesh time \[Setting]: select 15 minutes on Scheduled referesh

***

### Design report in PowerBI Desktop

Power BI Desktop -

1.  Copy weight-report.pbix and weight-vision.pbix,

    * open .pbix use Power BI Desktop
    * select WEIGHTING on right side, ... edit query,
    * Press Source, change Server and Database with new (Azure SQL) Server: tcp:nwiapps.database.windows.net,1433 Database: user name, password -- some columns may need to update

    \*\*\* Do not switch all tables to import mode \*\*\*

or Create new Power BI app: new -> Get Data -> A. select SQL Server database -> Server: tcp:nwiapps.database.windows.net,1433 Database: \*\*

```
	   Data Connectivity mode: DirectQuery
	   Select Table -> Transform Data
```

or B. select Azure Data Explorer (Kusto)-> -> Connect: cluster: https://nwiiot.australiaeast.kusto.windows.net

```
       Data Connectivity mode: DirectQuery
	   -> Ok
	   DATABASE: [WISE_4250] -> Transform Data
```

### Data type convert and design:

```
	Change (ABC)change type->(1.2)Decimal Number, 
	   = Table.TransformColumnTypes(Speed1,{{"Speed", type number}})
	   
	add text fillter (Enabled equal 1)
	  = Table.SelectRows(#"Changed Type", each [SpeedEnabled] = "1")

	close & apply

	add field and select visualizations
```

2\. Publish to new workspace that created by Power BI web portal, replace (if has existed)

***

### \[GroupId],\[ReportId] - PowerBI

1.  Open PowerBI portal, Select new report has created by Power BI Desktop

    URL example: https://app.powerbi.com/groups/###########/reports/@@@@@@@@@@@@/ReportSection

    GroupId(Workspace): (afer ../groups/): ########### ReportId: (after ../reports/): @@@@@@@@@@@@
2. Update \[GroupId] and \[ReportId] in constants.ts (Angular UI)

***

### Add API access

In new workspace, => Access entry PowerBIGroup => Add

select right top ... > Settings > Settings Datasets=> Scheduled refresh : 15 minutes

select right top ... > Settings > Admin portal. Select Tenant settings and then scroll down to the Developer settings section. Expand Allow service principals to use Power BI APIs, and enable this option.

Group -> \[PowerBIGroup] => Add members -> new workspace name has created

## &#x20;App Registration in Azure portal

refer: https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-spa-app-registration https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-sample-for-customers?tabs=net-core

Azure portal: https://portal.azure.com/

***

### Registration and Authentication

A. Register an applicatioin

```
 Search for App registrations:
      + New registration: 
 Register an applicatioin:
  ==> Name: @@@@
 
 ==> Register
  
```

B. Add Authentication URL

```
 Select just registed app @@@@=>
 [Manage]->Authentication->Add a platform
  ==>Select Web  
  
 Add Redirect URIs: 
   https://localhost:5002
   https://*****auth.azurewebsites.net/

 ==>Configure
```

C. Add API permissions

```
Select just registed app @@@@=>
[Manage]=> API permissions.

New window, select Power BI Service.
 =>Select Delegated permissions. 
 =>Select the permissions 
 
Add permissions below:

 Microsoft Graph (1)
	User.Read
 Power BI Service (16)
	App.Read.All
	Capacity.Read.All
	Capacity.ReadWrite.All
	Content.Create
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

    ... may need more ....
==>Save
```

***

### \[ApplicationId],\[TenantId],\[SecretId]

```
1. Select just registed app @@@@=>  Oveview
  => [Application (client) ID]
  => [Directory (tenant) ID]
  
2. [Manage] => 
  Certificates & secrets -> 
  + New client secret, 
  ==> description and select 24 months
  ==> Add 
  Get [Value] ---> is Secret value

Update appsettings.json in WWWebApp
"applicationId": ===>[Application (client) ID]
"tenant": ====>[Directory (tenant) ID]
"applicationSecret": =====> [Secret ID] 
```

### &#x20;run API App to test embedUrl and accessToken

Update by Steven, July 2022
