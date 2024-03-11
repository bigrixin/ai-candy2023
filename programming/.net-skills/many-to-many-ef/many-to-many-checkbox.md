---
description: Company 1--* CompanyWastetype *--1 WasteType
---

# Many to Many CheckBox



```
// Angular Html

    <div class="form-group mb-2">
      <label>Waste Type</label> <span style="color: red;"> *</span>
    </div>

    <div class="form-group mb-2">
      <section
        style="height: 200px; overflow:hidden; overflow-y:scroll;background-color: rgb(245 245 245); padding: 10px 0 10px 0; border: 1px solid rgb(197 187 187);border-radius: 0.25rem;">
        <span style="padding-left: 10px;">
          <mat-checkbox [checked]="allComplete" [color]="WasteTypeCheckBox.color" [indeterminate]="someComplete()"
            (change)="setAll($event.checked)">
            {{WasteTypeCheckBox.name}}
          </mat-checkbox>
        </span>

        <div *ngFor="let item of WasteTypeCheckBox.wastetypes let i=index"
          style="list-style-type: none; margin-top: 6px; margin-left: 20px; " >
          <input type="checkbox" formArrayName="selectedWasteTypes" [value]="item.wastetypeid" [checked]="item.selected"
            (change)="onCheckboxChange(item.wastetypeid, $event)" style="cursor: pointer;" />
          {{item.wastetypeid}} - {{item.wastetypename | titlecase}}
        </div>
      </section>
    </div>
	
```



```
// API View Model
   public class CompanyViewModel
    {
        public long Companyid { get; set; }
        public long[] SelectedWasteTypes { get; set; }
        public ICollection<CompanyWastetype> CompanyWastetypes { get; set; }
    }
       
```



```
await insertCompanyWastetype(company, companyVM.SelectedWasteTypes);

// add record in CompanyWasteType
private async Task<IActionResult> insertCompanyWastetype(Company company, long[] selectedWasteTypes)
{
    foreach (int wastetypeid in selectedWasteTypes)
    {
        CompanyWastetype companyWastetype = new CompanyWastetype();
        companyWastetype.Companyid = company.Companyid;
        companyWastetype.Wastetypeid = wastetypeid;
        await _companyWasteTypeRepository.AddAsync(companyWastetype);

        if (await _unitOfWork.SaveAsyc() < 0)

            return BadRequest("Could not create CompanyWastetype");

    }

    return Ok();  //company
}

// remove all record in CompanyWasteType
private async Task<IActionResult> deleteCompanyWastetype(Company company, long[] selectedWasteTypes)
{
    foreach (int wastetypeid in selectedWasteTypes)
    {

        var record = await _companyWasteTypeRepository.GetWhereAsync(a => a.Companyid == company.Companyid && a.Wastetypeid == wastetypeid);
        if (record == null)
            return NotFound();

        try
        {
            if (record.Count() > 0)
            {
                _companyWasteTypeRepository.Remove((CompanyWastetype)record);

                if (await _unitOfWork.SaveAsyc() <= 0)
                {
                    _logger.LogWarning($"Company id = {wastetypeid} has deleted ");
                    //  return NoContent();
                    return BadRequest($"Could not update company id= {company.Companyid}");
                }
            }
        }
        catch (Exception ex)
        {
            _logger.LogInformation($"Exception: " + ex.Message);
        }

    }
    return Ok();

}

```

```
// Angular .ts UI


  wastetypeList?: Wastetype[];
  allComplete: boolean = false;

  WasteTypeCheckBox: WastetypeList = {
    name: 'Select All',
    completed: false,
    color: 'primary',
    indeterminate: false,
    wastetypes: this.wastetypeList
  };

 	
  initialForm(data: any) {
    this.submitClicked = false;

    this.frm = this.fb.group({
      companyid: [{ value: 0, disabled: !this.is_edit }],
       selectedWasteTypes: new FormArray([])
    });

    if (data.action == 'Edit') {
      this.frm = this.fb.group({
        companyid: data.company.companyid,
        selectedWasteTypes: new FormArray([])
      });
    }
    this.getWastetypeList(data);
  }	
	
	
  getWastetypeList(data: any) {
    return this.wastetypeService.getAllWastetypes().subscribe({
      next: (res) => {
        this.WasteTypeCheckBox.wastetypes = res;
        if (data.action == 'Edit') {
          this.setSelectedWasteTypes(data);   《《《《《《
        }
      },
      error: (error) => {
        console.log('Error: retrive Wastetype list error - ' + error);
        this.toastr.warning(error.status + '- retrive data error', 'Fail');
      },
    });
  }


  setSelectedWasteTypes(data: any) {
    const selectedWasteTypesArray = (this.frm.controls['selectedWasteTypes'] as FormArray);
    let wasteTypeArray = data.company.selectedWasteTypes;

    for (let i = 0; i < wasteTypeArray.length; i++) {
      let wasteTypeId = wasteTypeArray[i];

      selectedWasteTypesArray.push(new FormControl(wasteTypeId));
      let wastetype = this.WasteTypeCheckBox.wastetypes?.find(x => x.wastetypeid === Number(wasteTypeId));
      if (wastetype != undefined && wastetype != null) {
        wastetype!.selected = true;
      }
    }

    if (this.WasteTypeCheckBox.wastetypes?.length != wasteTypeArray.length) {
      // this.allComplete = true;
      this.someComplete();
    }
    else {
      //  this.allComplete = false;

      let completed = false;
      if (wasteTypeArray.length > 0)
        completed = true;
      this.setAll(completed);
    }
  }

  someComplete(): boolean {
    if (this.WasteTypeCheckBox.wastetypes == null) {
      return false;
    }
    return this.WasteTypeCheckBox.wastetypes.filter(t => t.selected == true).length > 0 && !this.allComplete;
  }

  setAll(completed: boolean) {
    this.allComplete = completed;

    if (this.WasteTypeCheckBox.wastetypes == null) {
      return;
    }

    const selectedWasteTypesArray = (this.frm.controls['selectedWasteTypes'] as FormArray);
    selectedWasteTypesArray.clear();

    this.WasteTypeCheckBox.wastetypes.forEach(t => {
      t.selected = completed;
      if (completed) {
        selectedWasteTypesArray.push(new FormControl(t.wastetypeid));
      }
    })
  }

  onCheckboxChange(wastetypeid: Number, event: any) {
    const selectedWasteTypesArray = (this.frm.controls['selectedWasteTypes'] as FormArray);
    const index = selectedWasteTypesArray.controls.findIndex(x => x.value === wastetypeid);

    if (event.target.checked) {
      if (index == -1) {
        selectedWasteTypesArray.push(new FormControl(wastetypeid));
      }
    } else {
      selectedWasteTypesArray.removeAt(index);
    }

    let wastetype = this.WasteTypeCheckBox.wastetypes?.find(x => x.wastetypeid === wastetypeid);
    wastetype!.selected = event.target.checked;

    if (this.WasteTypeCheckBox.wastetypes!.filter(t => t.selected).length != this.WasteTypeCheckBox.wastetypes!.length)
      this.allComplete = false;
    else
      this.allComplete = true;
  }

```
