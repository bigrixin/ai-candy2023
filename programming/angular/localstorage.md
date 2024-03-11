---
description: error Type 'null' is not assignable to type 'string'.ts(2345)
---

# LocalStorage

```
// Some code

localStorage.setItem('token', user.token);
let token = localStorage.getItem('token');

>>>> error Type 'null' is not assignable to type 'string'.ts(2345)

let token =  JSON.parse(localStorage.getItem('token') || '{}');

let location = localStorage.getItem('Location')||'';

let locationId =  Number(localStorage.getItem('LocationId'));


```
