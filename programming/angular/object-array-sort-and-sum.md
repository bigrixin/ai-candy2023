# Object array sort and sum



{% code fullWidth="true" %}
```
// Some code
let traveler = [
  { companyname: 'CSC', Amount: 1000 },
  { companyname: 'PHP', Amount: 5200 },
  { companyname: 'AMP', Amount: 1125 },
  { companyname: 'SPS', Amount: 1235 },
  { companyname: 'IKKEY', Amount: 1280 }
];


//sort companyname in an object array
res.sort((a: any, b: any) =>
    (a.companyname > b.companyname) ? 1 : ((b.companyname > a.companyname) ? -1 : 0)
)


// sum in an object array
          let sumNet = res.map((a: { Amount: any; }) => a.Amount);
          let total = sumNet.reduce((acc: any, curr: any) => acc + curr);
```
{% endcode %}
