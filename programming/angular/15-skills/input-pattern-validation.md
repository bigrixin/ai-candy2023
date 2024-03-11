# Input pattern (validation)



{% code fullWidth="true" %}
```
// Angular input pattern

Force to UpperCase:
<input class="form-control" formControlName="rego" oninput="this.value = this.value.toUpperCase()">


Only numbers with a maximum of 2 decimal places:
<input class="form-control" formControlName="tare"  step="0.01" pattern="^[0-9]+(\.[0-9]{1,2})?$" placeholder="99.99 - only numbers with a maximum of 2 decimal places">

Limit 4 and number only:   
<input class="form-control" formControlName="postcode" [maxlength]="4" pattern="^-?[0-9]\d*(\9)?$" placeholder="0-9 number only">

Email
<input type="email" class="form-control" formControlName="email" pattern="^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" placeholder="name@example.com">


<input autoComplete="email" cFormControl formControlName="Email" placeholder="Email" email="true" pattern="^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" oninput="this.value = this.value.toLowerCase()" />
 



```
{% endcode %}
