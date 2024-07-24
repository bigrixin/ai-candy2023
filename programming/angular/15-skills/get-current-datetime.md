# Get current datetime

{% code fullWidth="true" %}
```typescript
    let currentDateStr = formatDate(new Date(), 'YYYY-MM-dd', 'en-AU');
    let currentTime1 = new Date(currentDateStr + ' ' + formGroupData['sendTime']).toLocaleString("en-AU", {
      day: '2-digit', month: '2-digit',
      year: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit'
    });


    let currentTime2 = new Date('2023-10-10 ' + formGroupData['sendTime']).toLocaleString();


    let currentTime3 = currentDateStr+' ,'+ formGroupData['sendTime'];
    
    
  
    var formData: any = new FormData();
    formData.append("SendTime", formGroupData['sendTime'] != null ? sendTime : "''");
```
{% endcode %}
