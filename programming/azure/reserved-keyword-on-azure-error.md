---
description: '''bin'' is a reserved word on Azure    --- azure conflict'
---

# Reserved keyword on Azure Error

&#x20;

{% code fullWidth="true" %}
```
Error: 
   Bin blocked: Bin Access-Control-Allow-Origin Missing Header	


Below code in Angular UI does not work.

 headers = new HttpHeaders()
    .set('content-type', 'application/json')
    .set('Access-Control-Allow-Origin', '*')
    .set('charset','utf-8');

 this.httpClient.get<any>(this.rootURL + 'api/bin/list', { headers: this.headers })


Solutions:

1. change bin to bins
2. change bin to ashbin   
   example: Change 'api/bin/create' to 'api/ashbin/create'   
3. 
  UI: if(type=='Bin') type = 'Bins';
  API: if(type=='Bins')  type = 'Bin';

```
{% endcode %}
