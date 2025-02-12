# Upload file to Blob



{% code fullWidth="true" %}
```

"ConnectionStrings": {
    "AzureBlobConnectionString": "DefaultEndpointsProtocol=https;AccountName=mooreroad;AccountKey=I4ap+8CbAEsNb3ozn2xK87vsisJVK7RvXTLgq0xRSJywc8ov6tggOFp5TamPTTuuRDaP4pgnRiMj+AStdmuy7g==;EndpointSuffix=core.windows.net"
  },
  
  "BlobConfiguration": {
    "BlobContainerName": "document-location",
    "BlobUrl": "https://mooreroad.blob.core.windows.net/"
    //"BlobContainerSystemSetting": "systemsetting-web",
  },
```
{% endcode %}

* Microsoft Azure>Storage accounts&#x20;
* "AzureBlobConnectionString": _name_ ->Security + networking ->Access keys->key1\
  &#x20;-》Connection string
* "BlobContainerName": _name_ ->Data storage->Containers->_container name_
* "BlobUrl": _container name_-> Settings->Properties



{% code fullWidth="true" %}
```
// Repository

 public class BlobRepository : IBlobRepository
 {
     private readonly BlobServiceClient _blobServiceClient;
     private readonly IAppSettingService _appSettingService;
     private readonly List<string> ImageExtensions = new List<string> { "JPG", "JPEG", "PNG" };
     private readonly string blockSectionName = "BlobConfiguration";

     public BlobRepository(BlobServiceClient blobServiceClient, IAppSettingService appSettingService)
     {
         _blobServiceClient = blobServiceClient;
         _appSettingService = appSettingService;
     }

     public async void DeleteBlob(string url, string blobContainerKey)
     {
         BlobContainerClient client = getBlobContainerClient(blobContainerKey);
         var fileName = new Uri(url).Segments.LastOrDefault();
         var blobClient = client.GetBlobClient(fileName);
         await blobClient.DeleteIfExistsAsync();
     }

     public string GetBlobURL(string blobContainerKey)
     {
         string blobContainerValue = _appSettingService.GetAppSettingChildrenValueBySectionKey(blockSectionName, blobContainerKey);
         string url = _appSettingService.GetAppSettingChildrenValueBySectionKey(blockSectionName, "BlobUrl");
         return url + '/' + blobContainerValue + "/";
     }

     public string GetLocalBlobURL(string blobContainerKey)
     {
         string blobContainerValue = _appSettingService.GetAppSettingChildrenValueBySectionKey(blockSectionName, blobContainerKey);
         string url = _appSettingService.GetAppSettingChildrenValueBySectionKey(blockSectionName, "BlobUrl");
         return url + blobContainerValue + "/";
     }

     public async Task<BlobObject> GetBlobFile(string url, string blobContainerKey)
     {
         BlobContainerClient client = getBlobContainerClient(blobContainerKey);
         var fileName = new Uri(url).Segments.LastOrDefault();

         try
         {
             var blobClient = client.GetBlobClient(fileName);
             if (await blobClient.ExistsAsync())
             {
                 BlobDownloadResult content = await blobClient.DownloadContentAsync();
                 var downloadedData = content.Content.ToStream();
                 string fileExtensionName = Path.GetExtension(fileName.ToUpperInvariant()).Replace(".", "");

                 if (ImageExtensions.Contains(fileExtensionName))
                 {
                     var extension = Path.GetExtension(fileName);
                     return new BlobObject { Content = downloadedData, ContentType = "image/" + extension.Remove(0, 1) };
                 }
                 else
                 {
                     return new BlobObject { Content = downloadedData, ContentType = content.Details.ContentType };
                 }
             }
             else
             {
                 return null;
             }
         }
         catch (Exception ex)
         {
             Console.WriteLine(ex.Message);
             return null;
         }
     }

     public async Task<List<string>> ListBlobs(string blobContainerKey)
     {
         BlobContainerClient client = getBlobContainerClient(blobContainerKey);
         List<string> list = new List<string>();

         await foreach (var blobItem in client.GetBlobsAsync())
         {
             list.Add(blobItem.Name);
         }

         return list;
     }

     public async Task<string> UploadBlobFile(string filePath, string fileName, string blobContainerKey)
     {
         BlobContainerClient client = getBlobContainerClient(blobContainerKey);
         var blobClient = client.GetBlobClient(fileName);
         var status = await blobClient.UploadAsync(filePath);
         Console.WriteLine(status);

         return blobClient.Uri.AbsoluteUri;
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


     public async Task<string> UploadBase64ImageToBlob(string base64Image, string blobContainerKey)
     {

         // Generate a unique file name
         string guIdString = Guid.NewGuid().ToString();
         string fileSuffix = base64Image.Substring(base64Image.IndexOf("/") + 1, base64Image.IndexOf(";base64") - base64Image.IndexOf("/") - 1);

         if (ImageExtensions.Contains(fileSuffix.ToUpper()))
         {
             string imageFileName = guIdString + '.' + fileSuffix;

             var result = await UploadBlobImage(imageFileName, base64Image, blobContainerKey);
             if (result != null)
             {
                 /*
                   var returnData = new { link = result };
                   string jsonData = JsonConvert.SerializeObject(returnData);
                   string url = JObject.Parse(jsonData)["link"].ToString();
                 */
                 return imageFileName;
             }
         }

         return null;
     }

     public async Task<string> UploadBlobImage(string fileName, string image, string blobContainerKey)
     {
         BlobContainerClient client = getBlobContainerClient(blobContainerKey);

         var encodedImage = image.Split(',')[1];
         var decodedImage = Convert.FromBase64String(encodedImage);

         var blobClient = client.GetBlobClient(fileName);

         using (var fileStream = new MemoryStream(decodedImage))
         {
             // upload image stream to blob
             var status = await blobClient.UploadAsync(fileStream, true);
             Console.WriteLine(status);
             if (status.GetRawResponse().Status == 201)
             {
                 return blobClient.Uri.AbsoluteUri;
             }
         }

         return null;
     }


     private BlobContainerClient getBlobContainerClient(string blobContainerKey)
     {
         string blobContainerValue = _appSettingService.GetAppSettingChildrenValueBySectionKey(blockSectionName, blobContainerKey);
         BlobContainerClient client = _blobServiceClient.GetBlobContainerClient(blobContainerValue);

         return client;
     }
 }
```
{% endcode %}



{% code fullWidth="true" %}
```
// Controller

  [ApiController]
  [Authorize]
  public class BlobStorageController : BaseController<BlobStorageController>
  {
      private readonly IBlobRepository _blobRepository;

      public BlobStorageController(IBlobRepository blobRepository)
      {
          _blobRepository = blobRepository;
      }

      [HttpGet("api/blob/image/{fileName}/{blobContainerKey}")]
      public async Task<IActionResult> GetBlobFile(string fileName, string blobContainerKey)
      {
          string url = _blobRepository.GetBlobURL(blobContainerKey);
          BlobObject result = await _blobRepository.GetBlobFile(url + fileName, blobContainerKey);
          if (result != null)
          {
              var fileStreamResult = File(result.Content, result.ContentType);
              return fileStreamResult;
          }

          return BadRequest();
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

      [HttpPost]
      [Route("api/blob/upload/image")]
      public async Task<IActionResult> UploadBlobImage(BlobImageModel model)
      {
          var result = await _blobRepository.UploadBlobImage(model.FileName, model.Image, model.BlobContainerKey);

          if (result != null)
          {
              var returnData = new { link = result };
              string jsonData = JsonConvert.SerializeObject(returnData);
              return Ok(jsonData);
          }

          return BadRequest();
      }

      [HttpPost]
      [Route("api/blob/upload/64image")]
      public async Task<IActionResult> UploadBlob64Image(BlobImageModel model)
      {
          var result = await _blobRepository.UploadBase64ImageToBlob(model.Image, model.BlobContainerKey);

          if (result != null)
          {
              var returnData = new { link = result };
              string jsonData = JsonConvert.SerializeObject(returnData);
              return Ok(jsonData);
          }

          return BadRequest();
      }


      [HttpDelete("api/blob/delete/{blobContainerKey}")]
      public IActionResult DeleteBlob(string url, string blobContainerKey)
      {
          _blobRepository.DeleteBlob(url, blobContainerKey);
          return Ok();
      }

      [HttpGet("api/blob/list/{blobContainerKey}")]
      public async Task<IActionResult> ListBlobs(string blobContainerKey)
      {
          var result = await _blobRepository.ListBlobs(blobContainerKey);
          return Ok(result);
      }
  }
```
{% endcode %}



{% code fullWidth="true" %}
````
```html
<h2 mat-dialog-title align="center" class="text-success pb-0 ">Upload file </h2>
<h5 align="center">(PDF or MS Word document only)</h5>
<div><b class="ps-4 mb-2"> Location: </b> <span class="text-info"> {{data.name}}</span></div>
<hr>

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
````
{% endcode %}



{% code fullWidth="true" %}
````
```typescript
import { Component, Inject, ViewChild } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { MAT_DIALOG_DATA, MatDialog, MatDialogRef } from '@angular/material/dialog';
import { Router } from '@angular/router';
import { ToastrService } from 'ngx-toastr';
import { Subject } from 'rxjs';
import { BlobService } from 'src/app/services/blob.service';
import { LocationService } from 'src/app/services/location.service';
import { constants } from '../../../constants';

@Component({
  selector: 'app-location-document-add',
  templateUrl: './location-document-add.component.html',
  styleUrl: './location-document-add.component.scss'
})
export class LocationDocumentAddComponent {
  @ViewChild('csvReader') csvReader: any;
  public records: any[] = [];
  dtOptions: any = {};
  dtTrigger: Subject<any> = new Subject<any>();
  isLoading = false;
  submitClicked = false;
  headerTitle: any = [];
  frm!: FormGroup;
  csvRecordsArray: any;
  failedArray: any = [];
  successedCount = 0;
  Locations: any;
  selectedTenant: any;
  canImport = true;
  selectedFileName = '';
  is_edit: boolean = true;
  maxColumn = 4;   ///<<<<

  constructor(
    @Inject(MAT_DIALOG_DATA) public data: any,
    private locationService: LocationService,
    private blobService: BlobService,
    private toastr: ToastrService,
    public router: Router,
    public fb: FormBuilder,
    private dialog: MatDialog,
    private dialogRef: MatDialogRef<LocationDocumentAddComponent>
  ) { }

  ngOnInit(): void {
    this.frm = this.fb.group({
      name: ['', Validators.required],
      file: [],
      description: [],
      blobContainerKey: [{ value: constants.blobContainerKey, disabled: !this.is_edit }],
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


  isValidFile(file: any) {
    if (file.name.endsWith(".pdf") || file.name.endsWith(".doc") || file.name.endsWith(".docx"))
      return true;
    else
      return false;
  }


  fileReset() {
    this.csvReader.nativeElement.value = "";
    this.records = [];
  }

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

  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('weighing', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }

  // delay ms
  async delayReLoad(url: string, ms: any) {
    setTimeout(() => {
      this.isLoading = false;
      this.dialogRef.close();
      this.toastr.success('File uploaded successfully !', 'Success', { timeOut: 2000 });
      this.reload(url);
    }, ms);
  }


}

```
````
{% endcode %}
