# Auto address use Azure Maps



{% code fullWidth="true" %}
````
// Some code
```html
<h2 mat-dialog-title><i class="bi bi-info-circle text-success"> </i> {{data.action}} Location</h2>
<hr>
<form [formGroup]="frm" (ngSubmit)="onSubmit(frm)">
  <mat-dialog-content>
    <div class="form-group" hidden>
      <label>Id</label>
      <input class="form-control" [attr.disabled]="true" formControlName="id">
    </div>

    <div class="form-group mb-2">
      <label>Full Address</label> <span style="color: red;"> *</span>
      <input type="text" class="form-control" (input)="searchAddress()" placeholder="Enter an address"
        formControlName="name">
      <div class="search-results" (mouseleave)="clearSearchResults()">
        <div *ngFor="let result of searchResults" (click)="selectAddress(result)">
          {{ result.address.freeformAddress }}
        </div>
      </div>
    </div>

    <div class="form-group mb-2">
      <label>Street Address</label>
      <input class="form-control auto-filling" type="text" formControlName="streetName">
    </div>
    <div class="form-group mb-2">
      <label>City</label>
      <input class="form-control auto-filling" type="text" formControlName="city">
    </div>
    <div class="form-group mb-2">
      <label>Region</label>
      <input class="form-control auto-filling" type="text" formControlName="region">
    </div>
    <div class="form-group mb-2">
      <label>State</label> <label style="margin-left: 218px;">Postcode</label>
      <div class="row">
        <input class="form-control oneline2-3 auto-filling" type="text" formControlName="state">
        <input class="form-control oneline2-3 auto-filling" type="text" formControlName="postalCode">
      </div>
    </div>
    <div class="form-group mb-2 ">
      <label>Country</label>
      <input class="form-control auto-filling" type="text" formControlName="country">
    </div>

    <div class="form-group mb-3 ">
      <div>
        <label>Source</label><span style="color: red;"> *</span>
        <label style="margin-left: 194px;">Destination</label> <span style="color: red;"> *</span>
      </div>
      <div class="row">
        <div class="form-check form-switch oneline2-3">
          <input class="form-check-input" type="checkbox" style="width:35px;height:20px;" formControlName="source"
            value="0">
        </div>

        <div class="form-check form-switch oneline2-3">
          <input class="form-check-input" type="checkbox" style="width:35px;height:20px;" formControlName="destination"
            value="0">
        </div>
      </div>
    </div>

    <hr>
    <div><span style="color: red;">*</span> is required</div>
  </mat-dialog-content>
  <div mat-dialog-actions align="center">
    <button class="btn btn-block btn-outline-secondary btn-sm" mat-button mat-dialog-close>Cancel</button>
    <span>&nbsp; &nbsp; </span>
    <button type="submit" [disabled]="!frm.valid || submitClicked" class="btn btn-outline-success btn-sm"
      mat-button>Save</button>
  </div>
</form>

```
````
{% endcode %}



{% code fullWidth="true" %}
````
// Some code
```scss
.search-results {
  position: absolute;
  width: 90%;
  background-color: rgb(231, 240, 248);
 // border: 1px solid #ccc;
  z-index: 100;
}
.search-results div {
  padding: 2px 6px;
  cursor: pointer;
}
.search-results div:hover {
  background-color: #93def5;
}
.auto-filling {
  background-color: rgb(241, 241, 241);
}

```
````
{% endcode %}



{% code fullWidth="true" %}
````
// Some code
```typescript
//import { Appearance, Location } from '@angular-material-extensions/google-maps-autocomplete';
import { Component, Inject, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { MAT_DIALOG_DATA, MatDialog } from '@angular/material/dialog';
import { Router } from '@angular/router';
import { ToastrService } from 'ngx-toastr';

import { LocationService } from 'src/app/services/location.service';
import { AzureMapService } from 'src/app/services/share/azure-map.service';


@Component({
  selector: 'app-location-add-update',
  templateUrl: './location-add-update.component.html',
  styleUrl: './location-add-update.component.scss'
})
export class LocationAddUpdateComponent implements OnInit {
  frm!: FormGroup;
  submitClicked = false;
  errorMessage: any;
  is_edit: boolean = true;
  LocationList: any;
  Role: any;

  searchQuery: string = '';
  searchResults: any[] = [];

  constructor(
    @Inject(MAT_DIALOG_DATA) public data: any,
    public router: Router,
    public fb: FormBuilder,
    private dialog: MatDialog,
    public toastr: ToastrService,
    private locationService: LocationService,
    private azureMapService: AzureMapService

  ) { }

  ngOnInit(): void {
    this.initialForm(this.data);
  }


  initialForm(data: any) {
    this.frm = this.fb.group({
      id: [{ value: 0, disabled: !this.is_edit }],
      name: ['', Validators.required],
      streetName: [],
      city: [],
      region: [],
      state: [],
      postalCode: [],
      country: [],

      source: [false, Validators.required],
      destination: [false, Validators.required],
    });

    if (data.action == 'Edit') {
      this.frm = this.fb.group({
        id: data.location.id,
        name: [data.location.name, Validators.required],
        streetName: [data.location.streetName],
        city: [data.location.city],
        region: [data.location.county],
        state: [data.location.state],
        postalCode: [data.location.postalCode],
        country: [data.location.country],

        source: [data.location.source, Validators.required],
        destination: [data.location.destination, Validators.required],
      });
    }

  }

  //#region
  async onSubmit(frm: FormGroup) {
    this.submitClicked = true;
    if (this.data.action == 'New') {
      this.locationService.addNewLocation(frm.value)
        .subscribe({
          next: data => {
            if (data.status == 200) {
              this.toastr.success('A new record has added successfully !', 'Success', { timeOut: 1000 });

              this.dialog.closeAll();
              this.reload(this.router.url);
            }
          },
          error: error => {
            console.error('There was an error in update!', error);
            this.toastr.error('Add Fail', 'Error')
          }
        })
      // console.log(result);
    }
    else if (this.data.action == 'Edit') {
      this.locationService.editLocation(frm.value)
        .subscribe({
          next: data => {
            if (data.status == 200) {
              this.toastr.success('Update Success', 'Success');

              this.dialog.closeAll();
              this.reload(this.router.url);
            }
          },
          error: error => {
            console.error('There was an error in update!', error);
            this.toastr.error('Update Fail', 'Error')
          }
        })
    }
  }

  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('weighing', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }

  searchAddress(): void {
    this.searchQuery = this.frm.get('name')?.value;
    if (this.searchQuery.length > 2) {
      this.azureMapService.searchAddress(this.searchQuery)
        .then(data => {
          this.searchResults = data.results;
        })
        .catch(error => {
          console.error('Error searching address:', error);
        });
    } else {
      this.clearSearchResults();
    }
  }

  selectAddress(result: any) {
    this.clearSearchResults();
    this.populateForm(result.address);
  }

  // Populate the form with the selected address
  populateForm(address: any) {
    this.frm.controls['name'].setValue(address.freeformAddress);
    this.frm.controls['streetName'].setValue(address.streetNumber ? address.streetNumber + ' ' + address.streetName : address.streetName);
    this.frm.controls['city'].setValue(address.localName);
    this.frm.controls['region'].setValue(address.countrySecondarySubdivision);
    this.frm.controls['state'].setValue(address.countrySubdivisionCode);
    this.frm.controls['postalCode'].setValue(address.postalCode);
    this.frm.controls['country'].setValue(address.country);
  }

  clearSearchResults() {
    this.searchResults = [];
  }

  //#endregion


}

```

````
{% endcode %}



{% code fullWidth="true" %}
````
```typescript
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { constants } from 'src/app/constants';

@Injectable({
  providedIn: 'root'
})
export class AzureMapService {
  azureMapsKey = constants.AzureMapsKey;
  searchUrl: string = 'https://atlas.microsoft.com/search/fuzzy/json';
  http: any;

  constructor() {
  }


  searchAddress(query: string): Promise<any> {
    const params = new URLSearchParams({
      query: query,
      countrySet: 'AU',
      typeahead: 'true',
      limit: '5',
      'subscription-key': this.azureMapsKey
    });

    return fetch(this.searchUrl + '?' + params.toString())
      .then(response => response.json())
      .catch(error => {
        console.error('Error fetching data:', error);
        return Promise.reject(error);
      });
  }


  // Get address suggestions from Azure Maps
  getAddressSuggestions(query: string): Observable<any> {
    const params = new URLSearchParams({
      query: query,
      countrySet: 'AU',
      typeahead: 'true',
      limit: '5',
      'subscription-key': this.azureMapsKey
    });

    //let a= 'https://atlas.microsoft.com/search/address/json?typeahead=true&api-version=1&query=${query}&language=en&countrySet=AU&view=Auto';
    const url = this.searchUrl + '?' + params.toString();
    return this.http.get(url);
  }
}

```
````
{% endcode %}
