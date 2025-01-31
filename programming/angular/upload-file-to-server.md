# Upload file to Server



{% code fullWidth="true" %}
```
//Server API

public class BlobFileModel
{
    public string FileName { get; set; }
    public IFormFile File { get; set; }
    public string BlobContainerKey { get; set; }
}


[HttpPost]
[Route("api/blob/upload/file")]
public async Task<IActionResult> UploadBlobFile(BlobFileModel model)
{
    if (model.File == null || model.File.Length == 0)
    {
        return BadRequest("No file uploaded.");
    }

    var result = await _blobRepository.UploadFileStreamToBlob(model.FileName, model.File, model.BlobContainerKey);
    if (result != null)
    {
        var returnData = new { link = result };
        string jsonData = JsonConvert.SerializeObject(returnData);
        return Ok(jsonData);
    }

    return StatusCode(500, "Failed to upload file.");
}


public async Task<string> UploadFileStreamToBlob(string fileName, IFormFile file, string blobContainerKey)
{
    var containerClient = getBlobContainerClient(blobContainerKey);
    var blobClient = containerClient.GetBlobClient(fileName);

    using (var stream = file.OpenReadStream())
    {
        // upload file stream to blob
        var status = await blobClient.UploadAsync(stream, overwrite: true);
        Console.WriteLine(status);
        if (status.GetRawResponse().Status == 201)
        {
            return blobClient.Uri.AbsoluteUri;
        }
    }

    return null;
}


Startup.cs

  services.AddSingleton(x => new BlobServiceClient(_configuration.GetConnectionString("AzureBlobConnectionString")));
```
{% endcode %}



{% code fullWidth="true" %}
````
// Angular UI
```html

<div class="controls">
  @if(isLoading){
  <mat-progress-bar mode="indeterminate" color="primary" aria-label="Loading content"> </mat-progress-bar>
  }
</div>

<form [formGroup]="frm" (ngSubmit)="onSubmit(frm)">
  <mat-dialog-content>
    <div class="form-group mb-3">
      <input type="file" class="file-input" name="input" id="txtFileUpload" (change)="uploadListener($event)"
        accept=".doc, .pdf, .docx" />
    </div>

    <div class="form-group" hidden>
      <label>BlobContainerKey</label>
      <input class="form-control" [attr.disabled]="true" formControlName="blobContainerKey">
    </div>
    <div class="form-group mb-2">
      <label>Name</label> <span style="color: red;"> *</span>
      <input class="form-control" formControlName="name">
    </div>
    <div class="form-group mb-3">
      <label>Description</label>
      <textarea rows="2" class="form-control" formControlName="description"></textarea>
      </div>
     <hr>
    <div><span style="color: red;">*</span> is required</div>
  </mat-dialog-content>
  <div mat-dialog-actions align="center">
    <button class="btn btn-block btn-outline-secondary btn-sm" mat-button mat-dialog-close>Cancel</button>
    <span>&nbsp; &nbsp; </span> <span>&nbsp; &nbsp; </span>
    <button type="submit" [disabled]="!frm.valid || submitClicked" class="btn btn-outline-warning btn-sm"
      mat-button>Upload</button>
  </div>
</form>
```


```typescript

  ngOnInit(): void {
    this.frm = this.fb.group({
      name: ['', Validators.required],
      file: [],
      description: [],
      blobContainerKey:[{ value: constants.blobContainerKey, disabled: !this.is_edit }],
    });
  }

  uploadListener($event: any): void {
    let files = $event.srcElement.files;
    this.canImport = true;
    if (this.isValidFile(files[0])) {
      this.isLoading = true;
      this.selectedFileName = files[0].name;
    //  let input = $event.target;
      let reader = new FileReader();
      reader.readAsArrayBuffer(files[0])
      reader.onload = () => {
        this.frm.controls['name'].setValue(this.selectedFileName);
        this.frm.controls['file'].setValue(files[0]);
        this.isLoading = false;
      };

      reader.onerror = function () {
        console.log('error is occured while reading file!');
      };

    } else {
      this.selectedFileName = 'No file selected';
      alert("Please import valid .csv file.");
      this.fileReset();
    }
  }

```

```typescript
  async onSubmit(frm: FormGroup) {
    this.submitClicked = true;
    this.isLoading = true;
    this.blobService.uploadFileToBlob(frm.value)
      .subscribe({
        next: data => {
          if (data.status == 200) {
            this.delayReLoad(this.router.url, 2000);  // 400 ms
          }
        },
        error: error => {
          console.error('Error uploading file', error);
          this.toastr.error('Error uploading file', 'Error')
          this.isLoading = false;
        }
      });
  }
```


```typescript
  uploadFileToBlob(formGroupData: any): Observable<any> {
    let header = new HttpHeaders().append('Content-Type', 'multipart/form-data');
    var formData: any = new FormData();
    formData.append("FileName", formGroupData['name']);
    formData.append("File", formGroupData['file']);
    formData.append("BlobContainerKey", formGroupData['blobContainerKey']);

    let endPoint = this.rootURL + 'api/blob/upload/file';
    return this.httpClient.post<any>(endPoint, formData, { observe: 'response' })
      .pipe(catchError(error => {
        console.error('Upload failed', error);
        return error;
      }));
  }

```
````
{% endcode %}
