---
description: example
---

# Angular read csv and upload to Server



{% code fullWidth="true" %}
```
// Angular .csv reader

https://alberthaff.dk/projects/ngx-papaparse/docs/v7/introduction

npm install ngx-papaparse@7 --save  //Angular 15+   ngx-papaparse@7.0.0
npm install ngx-papaparse@8 --save  //Angular 16+


angular.json

"build": {
    "builder": "@angular-devkit/build-angular:browser",
    "options": {
        "allowedCommonJsDependencies": [
            "papaparse"              <<<<<< add this line
        ],
        ...
    }
}


import { Papa } from 'ngx-papaparse';

constructor(
   private papa: Papa
 )
```
{% endcode %}

{% code fullWidth="true" %}
````
// Some code

```typescript

  uploadListener($event: any): void {
    let text = [];
    let files = $event.srcElement.files;

    if (this.isValidCSVFile(files[0])) {
      this.isLoading = true;
      let input = $event.target;
      let reader = new FileReader();
      reader.readAsText(input.files[0]);
      reader.onload = () => {
        let csvData = (<string>reader.result).trim();
        this.csvRecordsArray = csvData.split(/\r\n|\n/);
        let headerRow = this.getHeaderArray(this.csvRecordsArray);

        let recordColumn = 0;
        let options = {
          complete: (results: any, file: any) => {
            // console.log('Parsed: ', results, file);

            let csvArray = [];
            for (let key in results.data) {
              let csvRecord: any = [];
              let row = results.data[key];
              recordColumn = (Object.keys(row)).length;

              if (recordColumn == headerRow.length) {
                csvRecord.Id = Number(row.id);
                csvRecord.Name = row.name;
                csvRecord.Description = row.description;
                csvArray.push(csvRecord);
              }
            }
            //    console.log(csvArray); // array
            this.records = csvArray;
          },
          header: true
        };

        let jsonData = this.papa.parse(csvData, options);
        this.isLoading = false;

        if (this.headerTitle.length > 0 && recordColumn == this.headerTitle.length)
          this.submitClicked = true;
      };

      reader.onerror = function () {
        console.log('error is occured while reading file!');
      };

    } else {
      alert("Please import valid .csv file.");
      this.fileReset();
    }
  }


  isValidCSVFile(file: any) {
    return file.name.endsWith(".csv");
  }

  getHeaderArray(csvRecordsArr: any) {
    let headers = (<string>csvRecordsArr[0]).split(',');
    let headerArray: any = [];

    for (let j = 0; j < headers.length; j++) {
      headerArray.push(headers[j]);
      if (headers[j] != '')
        this.headerTitle.push(headers[j]);
    }
    return headerArray;
  }

  fileReset() {
    this.csvReader.nativeElement.value = "";
    this.records = [];
  }

  //add Batch Data
  async onImportClick() {
    this.isLoading = true;
    this.wasteTypeService.addBatchWasteTypes(this.records)
      .subscribe({
        next: data => {
          if (data.status == 200) {
            let newRecords: any[] = [];
            let failedList = data.body;

            this.records.forEach(element => {
              let csvRecord: any = [];
              csvRecord.Id = Number(element.Id);   //<<<<
              csvRecord.Name = element.Name;
              csvRecord.Description = element.Description;

              if (failedList.indexOf(element.Name) < 0)
                csvRecord.Result = true;
              else
                csvRecord.Result = false;
              newRecords.push(csvRecord);
            });

            //order by id
            //   this.newRecords.sort((a, b) => (a.Id > b.Id) ? 1 : ((b.Id > a.Id) ? -1 : 0));
            this.records = newRecords;
            this.isLoading = false;
            let failedLength = failedList.length;
            if (failedLength == 0)
              this.toastr.success('All new records (' + (this.records.length).toString() + ') have added successfully !', 'Success');
            else if (this.records.length != failedLength)
              this.toastr.info((this.records.length - failedLength).toString() + '/' + (failedLength).toString() + ' records have been successfully added !', 'Success');
            else
              this.toastr.warning('No records have been added !', 'Warning');

            this.submitClicked = false;
            this.reload(this.router.url);
          }
        },
        error: error => {
          console.error('There was an error in update !', error);
          this.toastr.error('Add customer fail !', 'Error')
        }
      })
  }

  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('weighing', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }
```
````
{% endcode %}



{% code fullWidth="true" %}
````
// html
<h2 mat-dialog-title align="center">Batch add records from csv file</h2>
<div style="padding:10px 10px 5px 35px" *ngIf="!submitClicked">
  <span>The csv file is styled as follows, the red part cannot be changed.</span><br>
  <div style="padding:10px">
    <span class="text-danger">id,name,description</span><br>
    <span class="text-danger">0,</span>Waste Type 1,description 1<br>
    <span class="text-danger">0,</span>Waste Type 2,descrtiption 2<br>
    <span class="text-danger">0,</span>...<br>
  </div>
</div>

<br>
<div class="batch-ui">
  <input type="file" #csvReader name="Upload CSV" id="txtFileUpload" (change)="uploadListener($event)" accept=".csv" />
  <div class="controls" *ngIf="isLoading">
    <mat-progress-bar mode="indeterminate" color="primary" aria-label="Loading content"> </mat-progress-bar>
  </div>
</div>

<hr>
<mat-dialog-content>
  <table class="table table-bordered table-striped table-hover batch-table" style="max-height: 200px; overflow-y: scroll">
    <thead class="table-dark">
      <tr align="center" class="align-middle center">
        <th *ngFor="let title of headerTitle">
          {{title}}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let record of records; let i = index" scope="row" >
        <td class="text-secondary">{{i + 1}}</td>
        <td class="text-warning">{{record.Name}}</td>
        <td class="text-primary">{{record.Description}}</td>
        <td>
          <span class="text-success" *ngIf="record.Result; else failed">
            Success
          </span>
          <ng-template #failed>
            <span *ngIf="record.Result == false" class="text-danger">
              Failed
            </span>
          </ng-template>
        </td>
      </tr>
    </tbody>
  </table>
</mat-dialog-content>

<div mat-dialog-actions align="center">
  <button class="btn btn-block btn-outline-secondary btn-sm" mat-button mat-dialog-close>Cancel</button>
  <span>&nbsp; &nbsp; </span> <span>&nbsp; &nbsp; </span>
  <button class="btn btn-block btn-outline-warning btn-sm" [disabled]="!submitClicked" mat-button
    (click)="onImportClick()">Import</button>

</div>

```

````
{% endcode %}

{% code fullWidth="true" %}
```
// API
[HttpPost]
[Route("api/wasteType/batch/create")]
public async Task<IActionResult> CreateWasteTypes([FromBody] List<WasteTypeViewModel> wasteTypeVMs)
{
    List<string> failedArray = new List<string>();

    //Post JSON input using FromBody
    if (!ModelState.IsValid)
    {
        _logger.LogInformation($"Model invalid!");
        return BadRequest(ModelState);
    }

    try
    {
        foreach (var wasteTypeVM in wasteTypeVMs)
        {
            // check WasteType name is unique
            string wasteTypeName = wasteTypeVM.Name;
            var oldRecord = await _repository.Find(i => i.Name == wasteTypeVM.Name);

            if (oldRecord != null)
            {
                _logger.LogInformation($"WasteType Name: {wasteTypeName} has existed");
                failedArray.Add(oldRecord.Name);
            }
            else
            {
                WasteType wasteType = new WasteType();
                wasteType = _mapper.Map(wasteTypeVM, wasteType);

                await _repository.AddAsync(wasteType);

                var result = await _unitOfWork.SaveAsyc();

                if (result != 1)
                {
                    _logger.LogInformation($"WasteType Name: {wasteTypeName} can not be created");
                    failedArray.Add(oldRecord.Name);
                }
            }
        }
        return Ok(failedArray);
    }
    catch (Exception ex)
    {
        _logger.LogInformation($"Exception: " + ex.Message);
    }
    _logger.LogInformation($"Could not create wasteType.");
    return
        BadRequest("Could not create wasteType");
}

```
{% endcode %}
