# disable and readonly



{% code fullWidth="true" %}
```
//field readonly

checkbox(switch): 
  onclick="return false;"

   <span class="form-check form-switch">
       <input class="form-check-input" type="checkbox" style="width:35px;height:20px;" onclick="return false;"
          formControlName="overweight" value=0>
   </span>

input:
  [readonly]="true"

    <input class="one-third-control" formControlName="Threshold" [readonly]="true">


//field hidden
hidden:
 [attr.disabled]="true"

      <input class="form-control" [attr.disabled]="true" formControlName="ID">
```
{% endcode %}
