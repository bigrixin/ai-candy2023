# Switch button and event





{% code fullWidth="true" %}
```
// html

<div class="form-group mb-2">
  <label class="mb-1">Select Item</label> <span style="color: red;"> *</span>
  <select class="form-select form-control" formControlName="item"  (change)="itemsOnChange($event)">
    <option [ngValue]=0>-- All Items --</option>
    <option *ngFor="let item of ItemLocationList" [ngValue]="item.itemNumber">
        {{item.itemNumber}}
    </option>
  </select>
</div>

<div class="form-group mb-2" *ngIf="selectedItem">
  <label class="form-check-label">Summary Report</label>
  <div class="form-check form-switch">
    <input class="form-check-input" type="checkbox" style="width:35px;height:20px;" 
        formControlName="isSummary" value="0">
  </div>
</div>


```
{% endcode %}



{% code fullWidth="true" %}
```
// Some code


selectedItem = false;   

  initialForm() {
    this.frm = new FormGroup({
      item: new FormControl('', [Validators.required]),
      isSummary: new FormControl(true),            /// switch component
      start: new FormControl('', [Validators.required]),
      end: new FormControl('')
    });
  }


  itemsOnChange(event: any) {
    if (event.target.value == "0: 0")   //  -- All Items --
    {
      this.selectedItem = true;
      this.frm.controls['isSummary'].setValue(true);
    }
    else {
      this.selectedItem = false;
      this.frm.controls['isSummary'].setValue(false);
    }
  }
```
{% endcode %}
