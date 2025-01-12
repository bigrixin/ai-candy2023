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
