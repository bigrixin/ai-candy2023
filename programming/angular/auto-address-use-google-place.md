# Auto address use Google place



{% code fullWidth="true" %}
```
// Some code
Google places API key. Ref - 

https://developers.google.com/maps/documentation/places/web-service/get-api-key

login to goolge account
Project name: Address Lookup
get API key


<div class="form-group mb-3">
  <input class="form-control" formControlName="name" #placesRef="ngx-places" ngx-gp-autocomplete [options]='options'
	(onAddressChange)="handleAddressChange($event)" />
</div>


```
{% endcode %}



{% code fullWidth="true" %}
```
// Some code

import { NgxGpAutocompleteModule } from "@angular-magic/ngx-gp-autocomplete";

imports:[
    NgxGpAutocompleteModule,
  ],
  providers: [
    {
      provide: Loader,
      useValue: new Loader({
        apiKey: '########',
        libraries: ['places']
      })
    },]


  formattedaddress=" ";
  options={
    componentRestrictions:{
      country:["AU"]
    }
  }
  @ViewChild('ngxPlaces') placesRef: NgxGpAutocompleteDirective | undefined;

  AddressChange(address: any) {
    //setting address from API to local variable
     this.formattedaddress=address.formatted_address
  }

  public handleAddressChange(place: google.maps.places.PlaceResult) {
    let address = place;
    // Do some stuff
  }
```
{% endcode %}
