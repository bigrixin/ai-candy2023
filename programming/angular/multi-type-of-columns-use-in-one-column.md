---
description: yes-no , dropdown and text
---

# Multi-type of columns use in one column



{% code fullWidth="true" %}
```
// Some code

    <div class="form-group mb-3" hidden>
      <label>Setting Content</label>
      <input class="form-control" formControlName="settinglist">
    </div>

    <div class="form-group mb-3">
      <label>Name</label> <span style="color: red"> *</span>
      <input class="form-control" formControlName="settingname" readonly>
    </div>

    <div class="form-group mb-3">
      <label class="form-check-label" for="flexSwitchCheckDefault">Value</label> <span style="color: red"> *</span>
      <span *ngIf="DisplayType=='text'">
        <input class="form-control" formControlName="settingvalue">
      </span>

      <span *ngIf="DisplayType=='dropdown'">
        <select class="form-select form-control" formControlName="dropdown_value" id="inputGroupSelect03">
          <option *ngFor="let item of DropdownList" [ngValue]="item.id">{{item.name}}</option>
        </select>
      </span>

      <span *ngIf="DisplayType=='yesno'">
        <div class="form-check form-switch">
          <input class="form-check-input" type="checkbox" style="width:35px;height:20px;" value="0" formControlName="yesno_value"/>
        </div>
      </span>
    </div>
```
{% endcode %}



{% code fullWidth="true" %}
```

initialForm(data: any) {
    this.frm = this.fb.group({
      settingid: [{ value: 0, disabled: !this.is_edit }],
      settinglist: [''],
      settingname: ['', Validators.required],
      settingvalue: [''],
      yesno_value: false,
      dropdown_value: [''],
    });


if (data.action == 'Edit') {
  this.frm = this.fb.group({
    settingid: data.globalSetting.settingid,
    settinglist: data.globalSetting.settinglist,
    settingname: data.globalSetting.settingname,
    settingvalue: [''],
    yesno_value: false,
    dropdown_value: [''],
  });

  let setting_list = data.globalSetting.settinglist;
  let setting_value = data.globalSetting.settingvalue;


  if (setting_list == null) {
    this.DisplayType = 'text';
    this.frm.patchValue({ settingvalue: setting_value });
  }

  else {
    let value_array = setting_list.split(',');
    if (value_array.length > 2) {

      let index = 0;
      let old_value_id = 0;
      value_array.forEach((element: any) => {
        if (element == setting_value)
          old_value_id = index;

        let item = { id: index, name: element };
        this.DropdownList.push(item);
        index++;
      })
      this.DisplayType = 'dropdown'
      this.frm.patchValue({ dropdown_value: old_value_id });
    }
    else {
      this.DisplayType = 'yesno';
      this.frm.patchValue({ yesno_value: setting_value=='Yes'?true:false });
    }
  }
}
```
{% endcode %}

}
