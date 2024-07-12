# Angular - Convert UTC to local time



* [https://www.localeplanet.com/icu/en-AU/timezone.html](https://www.localeplanet.com/icu/en-AU/timezone.html)   (Time Zones)
* [https://stackblitz.com/edit/angular-ivy-kdhxck?file=src%2Fapp%2Fapp.component.html](https://stackblitz.com/edit/angular-ivy-kdhxck?file=src%2Fapp%2Fapp.component.html) (Online test)
* [https://www.utctime.net/](https://www.utctime.net/)  (UCT time converter and <mark style="color:red;">Date Time Format List</mark>)



{% code fullWidth="true" %}
```
// This function is working
   function convertUTCToLocalTime(utcTime:any)
    {
      let dateTime =new Date(utcTime);
      let offsetMinutes = dateTime.getTimezoneOffset();                   //returns the difference in minutes.
      let numberOfMilliseconds = dateTime.getTime();                      //returns the number of milliseconds  since January 1, 1970 00:00:00.
      let offsetMilliseconds = - offsetMinutes  * 60 * 1000;
      dateTime.setTime(numberOfMilliseconds + offsetMilliseconds);
      let localTime = dateTime
      return localTime;
    }
```
{% endcode %}

{% code fullWidth="true" %}
```
// location time
<td>{{dataItem.updateTime+'Z' | date: 'dd/MM/yyyy hh:mm:ss a' }}</td>

>> 24/10/2023 04:24:24 PM
```
{% endcode %}



<pre data-full-width="true"><code>// Some code

data: 'TestDateTime', render: function (data: any, type: any, row: any) {
 return '&#x3C;span style="color:SandyBrown">' +
   formatDate(((new Date(data)).setHours((new Date(data)).getHours() + (new Date(data).getTimezoneOffset()/-60))), 'dd/MM/yyyy HH:mm:ss a', 'en-AU')
}


<strong>data: 'TestDateTime', render: function (data: any, type: any, row: any) {
</strong>    if (formatDate(new Date(data), 'dd/MM/yyyy', 'en-AU') != "01/01/0001")
      return '&#x3C;span class="text-info">' +
        formatDate(new Date(data), 'dd/MM/yyyy hh:mm:ss a', 'en-AU')+'&#x3C;/span>';
    else
      return ''
}
</code></pre>

{% code fullWidth="true" %}
```
// formatDate function

data: 'testDateTime', render: function (data: any, type: any, row: any) {
	return '<span style="color:SandyBrown">' + new Date(data) + '<br>' + 
	  data + '</span><br>' +
	  formatDate(((new Date(data)).setHours((new Date(data)).getHours() + 10)), 'dd/MM/yyyy HH:mm:ss a', 'en-AU') + '<br>' +
	  formatDate(data, 'dd/MM/yyyy HH:mm:ss a', 'en-AU') + '<<<<br>' +
	  formatDate(data, 'short', locale, '+1000') + '<br>'+
	  new Date(data).getTimezoneOffset()
	  }

>>
	Thu Jul 27 2023 01:14:30 GMT+1000 (Australian Eastern Standard Time)
	2023-07-27T01:14:30.304
	27/07/2023 11:14:30 AM
	27/07/2023 01:14:30 AM
	7/27/23, 1:14 AM
	-600
	
```
{% endcode %}



{% code fullWidth="true" %}
```
// UI show local time

	<p>UTC: {{ date | date: 'dd/MM/yyyy hh:mm:ss a':'+0000' }}</p>
	<p>Full: {{ date }}</p>
	<p>Local: {{ date | date: 'dd/MM/yyyy hh:mm:ss a' }}</p>
	<p>UTC-Local: {{ date | date: 'dd/MM/yyyy hh:mm:ss a':'+1100' }}</p>
	<p>Local: {{ date | date: 'medium' }}</p>
	<p>{{date.getTimezoneOffset()}}</p>


>>
	UTC: 01/11/2023 01:04:01 AM
	Full: Wed Nov 01 2023 12:04:01 GMT+1100 (Australian Eastern Daylight Time)
	Local: 01/11/2023 12:04:01 PM
	UTC-Local: 01/11/2023 12:04:01 PM
	Local: Nov 1, 2023, 12:04:01 PM
	-660
	


```
{% endcode %}
