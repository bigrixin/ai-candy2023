# Input pattern (validation)



<pre data-full-width="true"><code>// Angular input pattern

### Force to UpperCase:
&#x3C;input class="form-control" formControlName="rego" oninput="this.value = this.value.toUpperCase()">


### Only numbers with a maximum of 2 decimal places:
&#x3C;input class="form-control" formControlName="tare"  step="0.01" pattern="^[0-9]+(\.[0-9]{1,2})?$" placeholder="99.99 - only numbers with a maximum of 2 decimal places">

<strong>### Limit 4 and number only:   
</strong>&#x3C;input class="form-control" formControlName="postcode" [maxlength]="4" pattern="^-?[0-9]\d*(\9)?$" placeholder="0-9 number only">

### Email
&#x3C;input type="email" class="form-control" formControlName="email" pattern="^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" placeholder="name@example.com">


&#x3C;input autoComplete="email" cFormControl formControlName="Email" placeholder="Email" email="true" pattern="^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" oninput="this.value = this.value.toLowerCase()" />
 
### multi line 
    &#x3C;div class="form-group mb-2">
      &#x3C;label>Notes&#x3C;/label>
      &#x3C;textarea rows="3" class="form-control" formControlName="notes">&#x3C;/textarea>
    &#x3C;/div>


</code></pre>
