# Selection list (two ways)



{% code fullWidth="true" %}
```
// Some code

ngAfterViewInit(): void {
	let defaultLocationId = localStorage.getItem('DefaultLocationID');
	if (defaultLocationId) {
	   //default selection value
	  this.ngSelect = Number(defaultLocationId);
	}
}

onChange(value: any) {

}
  
  
<select id="location" class="form-select form-control" placeholder="Please select" style="font-size: x-large;"
  (change)="onChange($event)">
  <option *ngIf="ViewAllSite" value="0"></option>
  <option *ngFor="let location of LocationList" [ngValue]="location.id" [selected]="location.id === ngSelect">
	<b>{{location.name}}</b>
  </option>
</select>
```
{% endcode %}



{% code fullWidth="true" %}
```
//Angular Material selection


  ngAfterViewInit(): void {
    let defaultLocationId = localStorage.getItem('DefaultLocationID');
    if (defaultLocationId) {
      //default selection value
      this.selected = Number(defaultLocationId);
    }
  }


  <li *ngIf="GlobleUser">
	<mat-form-field>
	  <mat-select placeholder="Select Location" (selectionChange)="onChange($event)" [(value)]="selected">
		<mat-option *ngFor="let location of LocationList" [value]="location.id" >
		  {{location.name}}
		</mat-option>
	  </mat-select>
	</mat-form-field>
  </li>


  onChange(value: any) {
  }


import { MatSelectModule } from '@angular/material/select';
```
{% endcode %}
