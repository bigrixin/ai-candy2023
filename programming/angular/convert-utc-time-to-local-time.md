# Convert UTC time to Local time

{% code fullWidth="true" %}
```typescript
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
