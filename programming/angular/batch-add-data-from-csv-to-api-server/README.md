# Batch add data from csv to API server

UI modules: MatProgressBarModule, MatCheckboxModule

{% code fullWidth="true" %}
````
```html

<h2 mat-dialog-title align="center">Batch add <b class="text-info">Product</b> records from csv file</h2>
<div style="padding-left: 10px;">
  <input type="file" #csvReader name="Upload CSV" id="txtFileUpload" (change)="uploadListener($event)" accept=".csv" />
</div>

<div class="controls" *ngIf="isLoading">
  <mat-progress-bar mode="indeterminate" color="primary" aria-label="Loading content"> </mat-progress-bar>
</div>
<hr>
<mat-dialog-content>
  <table class="table table-bordered table-striped table-hover" >
    <thead class="table-dark">
      <tr align="center" class="align-middle center">
        <th *ngFor="let title of headerTitle">
          {{title}}
        </th>
        <th class="text-info">Result</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let record of records" scope="row">
        <td>{{record.Id}}</td>
        <td>{{record.Description}}</td>
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

<mark style="color:green;">Preview batch data, and display result (success/failed) after connecting to API</mark>

{% code fullWidth="true" %}
````
// .ts
import { Component, OnInit, ViewChild } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { MatDialog } from '@angular/material/dialog';
import { Router } from '@angular/router';
import { ToastrService } from 'ngx-toastr';
import { Subject } from 'rxjs';
import { ProductService } from 'src/app/services/product.service';

@Component({
  selector: 'app-product-batch-add',
  templateUrl: './product-batch-add.component.html',
  styleUrls: ['./product-batch-add.component.scss']
})
export class ProductBatchAddComponent implements OnInit {
  dtOptions: any = {};
  dtTrigger: Subject<any> = new Subject<any>();
  isLoading = false;
  submitClicked = false;
  public records: any[] = [];
  @ViewChild('csvReader') csvReader: any;

  headerTitle: any = [];
  frm!: FormGroup;
  csvRecordsArray: any;

  failedArray: any = [];
  successedCount = 0;


  constructor(
    private productService: ProductService,
    private toastr: ToastrService,
    private dialog: MatDialog,
    public router: Router,
    public fb: FormBuilder,
  ) { }

  ngOnInit(): void {

  }

  uploadListener($event: any): void {
    let text = [];
    let files = $event.srcElement.files;

    if (this.isValidCSVFile(files[0])) {
      this.isLoading = true;
      let input = $event.target;
      let reader = new FileReader();
      reader.readAsText(input.files[0]);
      reader.onload = () => {
        let csvData = reader.result;
        this.csvRecordsArray = (<string>csvData).split(/\r\n|\n/);
        let headerRow = this.getHeaderArray(this.csvRecordsArray);

        this.records = this.getDataRecordsArrayFromCSVFile(this.csvRecordsArray, headerRow.length);
        this.isLoading = false;

        if (this.headerTitle.length > 0 && this.records.length > 0)
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

  getDataRecordsArrayFromCSVFile(csvRecordsArray: any, headerLength: any) {
    let csvArray: any = [];

    for (let i = 1; i < csvRecordsArray.length; i++) {
      let currentRecord = (<string>csvRecordsArray[i]).split(',');
      if (currentRecord.length == headerLength) {
        let csvRecord: any = []; // = new Product();
        csvRecord.Id = Number(currentRecord[0].trim());
        csvRecord.Description = currentRecord[1].trim();

        csvRecord.Result = null;
        csvArray.push(csvRecord);
      }
    }
    return csvArray;
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

  //addBatchProducts
  async onImportClick() {
    this.isLoading = true;

    this.productService.addBatchProducts(this.records)
      .subscribe({
        next: data => {
          if (data.status == 200) {
            let newRecords: any[] = [];
            let failedList = data.body;

            this.records.forEach(element => {
              let csvRecord: any = [];
              csvRecord.Id = Number(element.Id);
              csvRecord.Description = element.Description;

              if (failedList.indexOf(element.Description) < 0)
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
          this.toastr.error('Add product fail !', 'Error')
        }
      })
  }

  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('weighing', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }

}

```
````
{% endcode %}

<mark style="color:green;">Convert to JSON and post to API</mark>

{% code fullWidth="true" %}
````
// Service

```typescript

  addBatchProducts(batchData: any): Observable<any> {
    //   var formData = this.buildFormData(formGroupData);
    let endPoint = this.rootURL + 'api/product/batch/create';
    let jsonOption = { 'content-type': 'application/json', 'charset': 'utf-8' };
    let header = new HttpHeaders(jsonOption);

    //to JSON -- use FromBody on .NET API
    var jsonData: any = [];

    // Iterate over each inner array
    for (var i = 0; i < batchData.length; i++) {
      var innerArray = batchData[i];

      // Convert the JSON array to a string
      var obj = Object.assign({}, innerArray);

      // Add the object to the JSON array
      jsonData.push(obj);
    }

    return this.httpClient
      .post<HttpResponse<Object>>(endPoint, jsonData, { observe: 'response', headers: header })
      .pipe((resp) => resp);
  };

```
````
{% endcode %}

<mark style="color:green;">API add record, return failed record</mark>

{% code fullWidth="true" %}
```
// API

[HttpPost]
[Route("api/product/batch/create")]
public async Task<IActionResult> CreateUpdateProducts([FromBody] List<ProductViewModel> productVMs)
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
      foreach (var productVM in productVMs)
      {
          // check Product name is unique
          string productName = productVM.Description;
          var oldRecord = await _repository.Find(i => i.Description == productVM.Description);

          if (oldRecord != null)
          {
              //update
              int productId = (int)oldRecord.Id;
              Product product = new Product();
              product = _mapper.Map(productVM, oldRecord);
              product.Id = productId;
              _repository.Update(product);

              failedArray.Add(oldRecord.Description);   // only for no other columns
          }
          else
          {
              //insert
              Product product = new Product();
              product = _mapper.Map(productVM, product);
              product.Id = 0;
              await _repository.AddAsync(product);
          }
          var result = await _unitOfWork.SaveAsyc();

          if (result != 1)
          {
              _logger.LogInformation($"Product Name: {productName} can not be created");
              failedArray.Add(oldRecord.Description);
          }

      }
      return Ok(failedArray);
  }
  catch (Exception ex)
  {
      _logger.LogInformation($"Exception: " + ex.Message);
  }
  _logger.LogInformation($"Could not create product.");
  return
      BadRequest("Could not create product");
}
```
{% endcode %}
