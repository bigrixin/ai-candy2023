---
description: This is a Angualr UI that be used to display Power BI report
---

# PowerBI embed app - Client

**Power BI web portal**

https://app.powerbi.com/

Workspace->Create a workspace->New -->wiseclient -->Access-> PowerBIGroup ->select Admin

workspaces->Reports->select wiseclient/ Copy URL https://app.powerbi.com/groups/\*\*\*\*\*\***/reports/**\*\*\*\*\*\*/ReportSection

get \[Workspace id] and \[Report id]

* Workspace id(afer ../groups/): \*\*\*\*\*\*
* Report id (after ../reports/): \*\*\*\*\*\*



**Power BI Desktop**

1.  Create new Power BI app: new -> Get Data ->select Azure Data Explorer (Kusto)-> -> Connect: cluster: https://nwiiot.australiaeast.kusto.windows.net Data Connectivity mode: DirectQuery (duration: 15 min) -> Ok

    ```
     DATABASE: SampleDb -> Transform Data
    ```
2.  Data type convert and design:

    ```
    A. Change (ABC)change type->(1.2)Decimal Number, 
       = Table.TransformColumnTypes(Speed1,{{"Speed", type number}})
       
    B. add text fillter (Enabled equal 1)
      = Table.SelectRows(#"Changed Type", each [SpeedEnabled] = "1")
      
    C. Add a local time column, (does not support for DirectQuery)

       = DateTime.LocalNow())

    D. In timestamp column, press top tab of Add column, select Time->  Local Time

       Column = [timestamp] + #duration(0,10,30,0)    //add 10 hours + 30min
       
    E. Change (ABC)change type->Date/Time

    F. Filter is in day (Today, Tomorrow, Yesterday)

    G. close & apply
    		
    H. add new column
    			
    	Modeling-> New Column
    	NewLocalTime = 'tablename'[timestamp] + 5/24

    I. Add field and select visualizations
    ```
3. Publish to Power BI web portal

**Add Workspace Access - (https://www.powerbi.com/)**

```
create new workspace, 
go to My workspace, at the name of Workspacs, select [Workspace Access]
input PowerBIGroup (create by Azure portal), Add
```

**Setting in Power BI Desktop**

File-> Options and settings ->Options

GLOBAL ->DirectQuery ->select \[Allow Unrestricted Measure} ->Preview features->select\[Azure map visual] ->Security -> select\[Map and Filled Map visuals]

CURRENT FILE ->Regional settings-> Locale for import-> English(Australia) ->Report setting-> select \[Allow reporters to personalize visuals to suit their needs]

**Azure Data Explorer**

Settings->General->Time Zone

**Power BI web portal**

```
https://www.powerbi.com/
At right top ... select Setting, Scheduled refresh, 15 minutes
```

**Update parameter in Angualr UI**

* set \[Workspace id] and \[Report id] in constants.ts of Angular UI,
* run API App to get embedUrl and accessToken

ng run build
