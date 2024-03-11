# Convert UTC to Local time

```
// Convert UTC to Timezone

Power BI ->Edit Query

1. Add new Colume switch zone, from UCT, +10:30

  =DateTimeZone.SwitchZone([timestamp],10,30)    

2. Change type to Date/Time/TimeZone

Save-> Close $ Apply

3. Change Format to '14/03/2001 1:30:55 PM (d/mm/yyyy h:nn:ss AM/PM)'
```

```
// The below method can be used for DirectQuery

 = Date.AddDays([timestamp],8.5/24)
```
