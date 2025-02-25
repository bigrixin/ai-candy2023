# Get first day of year, month, and date



```
  let date = new Date();
  let beginDate = new Date(date.getFullYear(), 0, 1);  // first day of this year
  beginDate.setHours(0, 0, 0, 0);

  let endDate = new Date();
  endDate.setDate(date.getDate() + 1);                 // first hour of next day
  endDate.setHours(0, 0, 0, 0);

  if (reportName == 'Month to Date Records')
    beginDate = new Date(date.getFullYear(), date.getMonth(), 1);   // first day of this month
  else if (reportName == 'Current day Records') {
    beginDate = new Date();
    beginDate.setDate(date.getDate());
    beginDate.setHours(0, 0, 0, 0);                     // first hour of today
  }
  

let date = new Date(2014, Number(json['month']) - 1, 1);
var monthName = date.toLocaleString('default', { month: 'short' });
		  
	  
var date = new Date(), y = date.getFullYear(), m = date.getMonth();
var firstDay = new Date(y, m, 1);         // first day in this month
var lastDay = new Date(y, m + 1, 0);      // last day in this month		  
```

