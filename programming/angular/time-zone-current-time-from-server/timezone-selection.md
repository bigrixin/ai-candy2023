# Timezone selection



{% code fullWidth="true" %}
```
// Some code

  timezones = [
    { id:0, state: 'NSW', tz: 'Australia/Sydney', code: 'AEST AEDT' },
    { id:1, state: 'QLD', tz: 'Australia/Brisbane', code: 'AEST AEDT' },
    { id:2, state: 'SA', tz: 'Australia/Adelaide', code: 'ACST ACDT' },
    { id:3, state: 'TAS', tz: 'Australia/Hobart', code: 'AEST AEDT' },
    { id:4, state: 'NT', tz: 'Australia/Darwin', code: 'ACST ACDT' },
    { id:5, state: 'VIC', tz: 'Australia/Melbourne', code: 'AEST AEDT' },
    { id:6, state: 'WA', tz: 'Australia/Perth', code: 'AWST AWDT' },
    { id:7, state: 'ACT', tz: 'Australia/Sydney', code: 'AEST AEDT' }
  ];
    


<div class="form-group mb-2">
  <label>State</label>
  <select class="form-select form-control" formControlName="state" id="inputGroupSelect01">
	<option *ngFor="let timezone of timezones" [ngValue]="timezone.state" >{{timezone.state}}</option>
  </select>
</div>
```
{% endcode %}
