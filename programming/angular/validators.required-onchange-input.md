# Validators.required OnChange input



{% code fullWidth="true" %}
```
// splitWeigh == true && truckStoredTare !=null, trailerStoredTare change to Validators.required


import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
})
export class YourComponent {
  form: FormGroup;

  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      splitWeigh: [false],
      truckStoredTare: ['', [Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]],
      trailerStoredTare: ['', [Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]],
    });

    // 监听 splitWeigh 和 truckStoredTare 的变化
    this.form.get('splitWeigh')?.valueChanges.subscribe(() => {
      this.updateTrailerStoredTareValidators();
    });

    this.form.get('truckStoredTare')?.valueChanges.subscribe(() => {
      this.updateTrailerStoredTareValidators();
    });

    // 初始化时检查
    this.updateTrailerStoredTareValidators();
  }

  updateTrailerStoredTareValidators() {
    const truckStoredTare = this.form.get('truckStoredTare')?.value;
    const splitWeigh = this.form.get('splitWeigh')?.value;

    // 如果 truckStoredTare 不为空并且 splitWeigh 为 true，则 trailerStoredTare 为必填
    if (truckStoredTare && splitWeigh) {
      this.form.get('trailerStoredTare')?.setValidators([Validators.required, Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]);
    } else {
      // 否则， trailerStoredTare 的验证不需要强制要求
      this.form.get('trailerStoredTare')?.setValidators([Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]);
    }

    // 触发验证
    this.form.get('trailerStoredTare')?.updateValueAndValidity();
  }

  truckStoreOnChange(event: any) {
    // 这里调用 updateTrailerStoredTareValidators 来确保 trailerStoredTare 的验证被正确更新
    this.updateTrailerStoredTareValidators();
  }

  onSubmit() {
    if (this.form.valid) {
      console.log(this.form.value);
    }
  }
}

```
{% endcode %}

{% code fullWidth="true" %}
````
// Some code
```typescript
  RateTypes = [
    { id: 1, name: "UNIT RATE" },
    { id: 2, name: "FLAT RATE" },
  ];


  ngOnInit(): void {
    this.form = this.fb.group({
      rateType: [null, Validators.required],
      price: [null],
      flatRate: [null],
    });

    // 初始化时，根据 rateType 选择进行动态验证
    this.onRateTypeChange();
  }

  onRateTypeChange(event?: any): void {
    const rateType = this.form.get('rateType')?.value;

    if (rateType === 'FLAT RATE') {
      this.form.get('flatRate')?.setValidators([Validators.required, Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]);
      this.form.get('price')?.clearValidators();
    } else if (rateType === 'UNIT RATE') {
      this.form.get('price')?.setValidators([Validators.required, Validators.pattern(/^(?:[0-9]+(?:\.[0-9]{0,2})?)?$/)]);
      this.form.get('flatRate')?.clearValidators();
    }

    // 重新检测字段的有效性
    this.form.get('price')?.updateValueAndValidity();
    this.form.get('flatRate')?.updateValueAndValidity();
  }
}



```html
    <div class="form-group mb-3">
      <div>
        <label>Rate Type</label> <span style="color: red;"> *</span>
      </div>
      <div class="row">
        <select class="form-select form-control oneline2-3" formControlName="rateType" placeholder="Please select" (change)=onRateTypeChange($event)>
          @for(rateType of RateTypes; track rateType.id) {
          <option [ngValue]="rateType.name">{{rateType.name}}</option>
          }
        </select>

      </div>
    </div>

    <div class="form-group mb-2">
      <div>
        <label>Unit Price</label>
        <label style="margin-left: 186px;">Flat Rate</label>
      </div>
      <div class="row">
        <input class="form-control oneline2-3" formControlName="price" pattern="^(?:[0-9]+(?:\.[0-9]{0,2})?)?$"
          placeholder="2 decimal places" />
        <input class="form-control oneline2-3" formControlName="flatRate" pattern="^(?:[0-9]+(?:\.[0-9]{0,2})?)?$"
          placeholder="2 decimal places" />
      </div>
    </div>
```
````
{% endcode %}
