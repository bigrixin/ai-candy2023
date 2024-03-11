# Angular add header



````
// add headers

```typescript

  headers = new HttpHeaders()
     .set('content-type', 'application/json')
     .set('Access-Control-Allow-Origin', '*');
     
  return this.httpClient.get<any>(this.rootURL, { headers: this.headers }).pipe(map(res => res));

```


```typescript

    let jsonOption = { 'content-type': 'application/json', 'charset': 'utf-8' };
    let header = new HttpHeaders(jsonOption);
  
    return this.httpClient.put<HttpResponse<Object>>(endPoint, jsonBody, { observe: 'response', headers:  header }).pipe(resp => resp);
 
```


````
