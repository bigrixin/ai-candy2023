# Power BI

### A. Convert Import to DirectQuery

{% hint style="info" %}
Import data mode: reflash data is every 3 hours.

DirectQuery data mode: reflash data in min is every 15 mins&#x20;
{% endhint %}

### B. Change Time zone - Power BI Desktop

1. In timestamp column, press top tab of Add column, select Time-> Local Time

```
       Column = [timestamp] + #duration(0,10,30,0)    //add 10 hours + 30min
```

* Power BI Desktop, go to Transform data,
* At top menu, press Recent Sources, select cluster URL.
* Select Database, Table, --> OK
* Select DirectQuery, delete the old one

#### **2. Other setting**

&#x20;   **File-> Options and settings ->Options**

#### &#x20;  **GLOBAL**&#x20;

* \->**DirectQuery** ->select \[Allow Unrestricted Measure}&#x20;
* \->**Preview feature**s->select\[Azure map visual]&#x20;
* \->**Security** -> select\[Map and Filled Map visuals]

#### &#x20;  **CURRENT FILE**&#x20;

* \->**Regional settings**-> Locale for import-> English(Australia)&#x20;
* \->**Report setting**-> select \[Allow reporters to personalize visuals to suit their needs]

#### 3. In Power BI - Web

* Left side menu, press Datasets, at dataset, select Settings,&#x20;
* Scheduled refresh ->ON
* Select Refresh frequency ( 15min to Weekly)

### C. Azure Data Explorer

* Top right Seting Icon, In General select Time Zone
