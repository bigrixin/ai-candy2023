# Add parameter to PowerBI report

```
// Add parameters to PowerBI report: (**** This part is not necessary ****)

1. open PowerBI report .pbix uses PowerBI Desktop.
2. go to Edit query
3. select a column, right mouse key
4. select Add as New Query
5. at New Query, right mouse key
6. select Remove Duplicates
7. may change the poperies (Name)
```

{% code fullWidth="true" %}
````
// Add parameters in Url for PowerBI report query filter:

The syntax is: ?filter=<Table name>/<Column name> eq <'value'>

  ***************（very important) *************
 --  Table and column names Case sensitive 
 ***************（very important) *************
 
```typescript
  customerFilter1: "?filter=stock/CUSTOMER eq '",       ///stock: table name, CUSTOMER: Column name
  customerFilter2: "' and stock/CUSTOMER eq '",
```

1. One value query via URL, append below code in URL:

   ?filter=Store/Territory eq 'Go NC'

2. More than on value:

   ?filter=Store/Territory eq 'NC' and Store/Chain eq 'Fashions Direct'
   ?filter=Store/Territory in ('NC', 'TN')


-- if have '/ReportSection?experience=power-bi' is end of url, use '&' instead by '?'

   /ReportSection?experience=power-bi&filter=Store%2FTerritory%20eq%20%27Go%20NC%27
   
   

````
{% endcode %}
