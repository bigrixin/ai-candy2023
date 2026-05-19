# Colour picker

{% code fullWidth="true" %}
````


import { MAT_COLOR_FORMATS, NgxMatColorPickerModule, NGX_MAT_COLOR_FORMATS } from '@angular-material-components/color-picker';

```
  imports:[
  NgxMatColorPickerModule,
  ],
  providers: [
    { provide: MAT_COLOR_FORMATS, useValue: NGX_MAT_COLOR_FORMATS } ]

```

  hexToRgb(hex: any) {
    const shorthandRegex = /^#?([a-f\d])([a-f\d])([a-f\d])$/i;
    hex = hex.replace(shorthandRegex, (m: any, r: any, g: any, b: any) => {
      return r + r + g + g + b + b;
    });
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    return result ? {
      r: parseInt(result[1], 16),
      g: parseInt(result[2], 16),
      b: parseInt(result[3], 16)
    } : null;
  }



```html
<div class="form-group mb-2">
  <label class="mb-2">Pickup a background colour</label> <span style="color: red;"> *</span>
  <input class="form-control" matInput formControlName="colour" [ngxMatColorPicker]="picker" style=" background-color: rgb(202, 204, 204) !important;">
  <ngx-mat-color-toggle matSuffix [for]="picker"  style="position: absolute; left:420px; bottom:177px;"></ngx-mat-color-toggle>
  <ngx-mat-color-picker #picker [touchUi]="touchUi" [color]="color" ></ngx-mat-color-picker>
</div>
```
````
{% endcode %}



{% code fullWidth="true" %}
```
 
<td align="center">{{dataItem.colour}} <i class="fa fa-square-o" aria-hidden="true"  [style.background-color]=dataItem.colour [style.color]=dataItem.fontColour></i></td>
 
```
{% endcode %}
