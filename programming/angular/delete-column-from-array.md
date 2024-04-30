# Delete column from Array



{% code fullWidth="true" %}
````
// delete the first item

keys = 
    [
      "errorId",
      "location",
      "Name",
      "type",
      "startDate",
      "startTime"
    ]



// delete "errorId",

```typescript
   keys.shift();
```
````
{% endcode %}



{% code fullWidth="true" %}
````
// delete the first column from a array list

rows =
[
  {
    errorId: 1,
    location: 1,
    name: "Big door",
    type: "Door",
    startDate: "29/04/2024",
    startTime: "02:43:18",
  },
  {
    errorId: 2,
    location: 1,
    name: "Big Auger",
    type: "Auger",
    startDate: "29/04/2024",
    startTime: "02:47:00",
  },
  {
    errorId: 3,
    location: 5,
    name: "Little door",
    type: "Dump",
    startDate: "29/04/2024",
    startTime: "02:49:55",
  },
  {
    errorId: 4,
    location: 1,
    name: "Big door",
    type: "Dump",
    startDate: "29/04/2024",
    startTime: "02:54:13",
  }
]


```typescript
  rows.forEach(obj=>{delete obj['errorId']});
```
````
{% endcode %}
