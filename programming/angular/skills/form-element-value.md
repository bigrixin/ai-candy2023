# Form element value



{% code fullWidth="true" %}
````
// Get value from Form element

<div class="form-group mb-3">
  <label class="form-check-label" for="flexSwitchCheckDefault">Input Manual Price?</label> <span style="color: red">
	*</span>
  <div class="form-check form-switch">
	<input class="form-check-input" type="checkbox" style="width:35px;height:20px;" formControlName="isText"
	  (change)="onChange($event)" value=false id="flexSwitchCheckDefault" />
  </div>
</div>


let textValue = this.frm.controls['isText'].value;

let customerId = this.frm.value['customerid'];
let beginDate = new Date(frm.value['start']);


```typescript
 let endDateStr = formatDate(endDate, 'MM-dd-yyyy', "en-AU");
```



// set value to Form element
<div class="form-group mb-2">
  <label>Product</label> <span style="color: red;"> *</span>
  <select class="form-select form-control" formControlName="price" id="inputGroupSelect02">
	<option *ngFor="let product of ProductList" [ngValue]="product.productid">{{product.description}}</option>
  </select>
</div>

this.frm.controls['price'].setValue('');


// get check box group value as array from Form
<div class="form-group mb-2 tab-content">
  <section class="select-checkbox" style="background-color: rgb(232 245 234)">
	<div class="select-all-checkbox">
	  <mat-checkbox [checked]="allComplete" [color]="TypesCheckBox.color" [indeterminate]="someComplete()"
		(change)="setAll($event.checked)">
		<span class="select-all-content"> {{TypesCheckBox.name}}</span>
	  </mat-checkbox>
	</div>

	<div *ngFor="let item of TypesCheckBox.types let i=index">
	  <input class="select-item-checkbox" type="checkbox" formArrayName="SelectedTypes" [value]="item.id"
		[checked]="item.selected" (change)="onCheckboxChange(item.id, $event)" />
	  <span class="select-item-content"> - {{item.name }} </span>
	</div>
  </section>
</div>


const selectedTypesArray = (this.frm.controls['SelectedTypes'] as FormArray);
````
{% endcode %}
