# JSON Parse && Object Array



```
// Convert to JSON  
  this.stringifiedData = JSON.stringify(this.myData);  
    console.log("With Stringify :" , this.stringifiedData);  
  
  {  "businessHour": "0", "closed": "yes",  "reviewStatus": "F"}
  

// Parse from JSON  
  this.parsedJson = JSON.parse(this.stringifiedData);  
    
    {
        businessHour: "0",
        closed: yes,
        reviewStatus: "F",
    }

```

```
// push new object array to other array         
          
            
    let oldData = resp.data;
    var newData = [];

    for (let i = 0; i < oldData.length; i++) {
      if (customerName !== '') {
        let item = oldData[i];
        delete item['customerAccountCode'];
        newData.push(item);
      }
    }
```
