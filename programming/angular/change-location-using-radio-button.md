# Change location using radio button

{% code fullWidth="true" %}
```
// Some code


-------------------------------------
  .ts
-------------------------------------
import { Router } from '@angular/router';
import { constants } from 'src/app/constants';
import { CurrentLocation } from '../../models/current-location';

  CurrentLocationId: any;
  ClientLocations = constants.locations;

  constructor(
    private router: Router,
   }


  radioChange(data: any) {
    this.CurrentLocationId = data.value;
    localStorage.setItem('SelectedLocation', data.value);
    this.isLoading = true;
    let index = Number(data.value);

    if (Array.isArray(this.ClientLocations)) {
      this.ClientLocations.forEach(
        function (element: any, index: number) {
          if (element.id == data.value)
            element.selected = true;
          else
            element.selected = false;
        });
    }

    let location = this.ClientLocations[index - 1];
    CurrentLocation.SetCurrentLocation(location);

    let currentRouter = this.router.url.substring(1);
    this.reload(currentRouter);
    this.isLoading = false;
  }

  async reload(url:string): Promise<boolean> {
    await this.router.navigateByUrl('email', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }



-------------------------------------
  .html
-------------------------------------
<div style="padding: 12px; background-color: rgb(224, 224, 224); font-size: small;" class="text-success" align="center">
  <mat-radio-group color="primary" (change)="radioChange($event)" >
    <mat-radio-button  *ngFor="let location of ClientLocations" value="{{location.id}}" class="pe-3"
      checked="{{ location.selected }}">{{location.name}}</mat-radio-button>
  </mat-radio-group>
</div>

-------------------------------------
  module.ts:
-------------------------------------
import { MatRadioModule } from '@angular/material/radio';

    MatRadioModule,
 
```
{% endcode %}
