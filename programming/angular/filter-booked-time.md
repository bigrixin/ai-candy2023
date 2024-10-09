# Filter booked time



{% code fullWidth="true" %}
```
// delete booked time by max limit time

  fiterBookedTime(data: any){
      // get all booked time that have not completed
      data.forEach((element: any) => {
        let dateString = element.bookingStartTime;
        const time = dateString.substr(11, 5);
        this.bookedTimeArray.push(time)                      //  ['10:20','11:30',....]
      });

      // count booked time
      const itemCount: Map<string, number> = new Map();
      for (const item of this.bookedTimeArray) {
        itemCount.set(item, (itemCount.get(item) || 0) + 1);  // {size:2, '10:20'=>2,'11:30'=>1}
      }

      // Filter booking times by maxBookingLimit
      itemCount.forEach((value, key)=>{
        if (value == this.maxBookingLimit)
          this.timeIntervals = this.timeIntervals.filter(item => item !== key);
      })

  }

```
{% endcode %}
