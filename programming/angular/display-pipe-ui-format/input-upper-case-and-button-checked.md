# Input upper case and button checked



{% code fullWidth="true" %}
```html
// Force to Upper case input

<input class="form-control" formControlName="name"  oninput="this.value = this.value.toUpperCase()" >


// Lower case
 <input class="form-control" formControlName="name" oninput="this.value = this.value.toLowerCase()" >


```
{% endcode %}

{% code fullWidth="true" %}
```
// Some code
html:
      <div class="form-group mb-2">
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="checkbox" formControlName="isActived" value="0">
            <label class="form-check-label">Carrier</label>
      </div>

add:

        isActived: false,

edit:

        isActived: Boolean(JSON.parse(data.customer.isCarrier)),


model:
        IsActived!: boolean;


service:
          formData.append("IsActived", Number(formGroupData['isActived']));
```
{% endcode %}
