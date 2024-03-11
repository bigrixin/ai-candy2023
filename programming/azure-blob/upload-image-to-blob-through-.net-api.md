---
description: >-
  Angular+ngx-dropzone send base64 image string to .NET API, encode, send image
  to Blob, get Url
---

# Upload image to Blob through .NET API

[https://www.base64encoder.io/image-to-base64-converter/](https://www.base64encoder.io/image-to-base64-converter/)\
\
&#x20; \-- copy "Base64 Image source" to upload using API\


````
// upload.module.ts

``` 
    NgxDropzoneModule,      ///  ngx-dropzone
    ReactiveFormsModule,
    MatProgressBarModule
```
````

```
// upload.ts
 
import { throwDialogContentAlreadyAttachedError } from '@angular/cdk/dialog';
import { Component, OnInit } from '@angular/core';
import { UntypedFormControl, UntypedFormGroup, Validators } from '@angular/forms';
import { ToastrService } from 'ngx-toastr';
import { UploadService } from 'src/app/services/upload.service';

@Component({
  selector: 'app-upload-to-blob',
  templateUrl: './upload-to-blob.component.html',
  styleUrls: ['./upload-to-blob.component.scss']
})
export class UploadToBlobComponent implements OnInit {
  shortLink: string = "";
  loading: boolean = false; // Flag variable
  file: any; // Variable to store file
  imageValid: boolean = false;
  files: File[] = [];
  base64Image: any;

  constructor(
    private fileUploadService: UploadService,
    private toastr: ToastrService,

  ) { }

  ngOnInit(): void {

  }

  onSubmit() {
    this.loading = !this.loading;
    var jsonData = {
      "FileName": this.files[0].name,
      "Image": this.base64Image
    }

    this.fileUploadService.uploadImageToBlob(jsonData)
      .subscribe({
        next: (data) => {
          if (data.status == 200) {
            this.toastr.success('Upload image complate', 'Success');
            this.shortLink = data.body.link;
            this.loading = false; // Flag variable
          }
        },
        error: (error) => {
          //  console.log(error);
          this.toastr.warning(error.status + ' - upload image Fail', 'Fail');
        },
      });

  }

  onSelect(event: any) {
    console.log(event);
    this.files.length = 0;
    this.files.push(...event.addedFiles);
    let arrayLength = this.files.length;
    // keep one file
    this.files.splice(1, arrayLength - 1);

    if (arrayLength === 1) {
      this.imageValid = true;
      this.readFile(this.files[0]).then(fileContents => {
        // Put this string in a request body to upload it to an API.
        //  console.log(fileContents);
        this.base64Image = fileContents;
      })

    }
  }

  onRemove(event: any) {
    console.log(event);
    this.files.splice(this.files.indexOf(event), 1);
    this.imageValid = false;
  }


  private async readFile(file: File): Promise<string | ArrayBuffer> {
    return new Promise<string | ArrayBuffer>((resolve, reject) => {
      const reader = new FileReader();

      reader.onload = (e) => {
        return resolve((e.target as FileReader).result || '');
      };

      reader.onerror = e => {
        console.error(`FileReader failed on file ${file.name}.`);
        return reject(null);
      };

      if (!file) {
        console.error('No file to read.');
        return reject(null);
      }

      reader.readAsDataURL(file);
    });
  }

}

 
```

```
// upload.html

<ng-container>
  <div class="card border-success">
    <div class="card-header">
      Feature: Upload file to the Azure blob storage
    </div>
    <div class="card-body text-success">
      <div class="py-5 shadow">

        <div class="row justify-content-center">
          <div class="col-sm-10 col-10 col-lg-6">
            <h1 class="card-title">Upload image</h1>

            <div class="form-group mb-3 text-center">
              <!--droped image preview-->
              <div class="custom-dropzone" ngx-dropzone [accept]="'image/jpeg,image/jpg,image/png,image/gif'"
                (change)="onSelect($event)" >
                <ngx-dropzone-label>
                  <div>
                    <h2>Drop image here or press to select</h2>
                  </div>
                </ngx-dropzone-label>
                <ngx-dropzone-image-preview ngProjectAs="ngx-dropzone-preview" *ngFor="let f of files" [file]="f"
                  [removable]="true" (removed)="onRemove(f)">
                  <ngx-dropzone-label style="position:relative; padding-left: 10px;"><span> &nbsp; &nbsp; </span> {{ f.name }} ({{ f.type }})</ngx-dropzone-label>
                </ngx-dropzone-image-preview>
              </div>

              <!--droped image preview-->
            </div>

            <div class="form-group mb-3">
              <div class="text-center">
                <button (click)="onSubmit()"  class="btn btn-info" [disabled]="!imageValid">Submit</button>
              </div>
            </div>

            <!-- Shareable short link of  uploaded file -->
            <div class="container text-center jumbotron" *ngIf="shortLink">
              <h2> Visit Here</h2>
              <a href="{{shortLink}}">{{shortLink}}</a>
            </div>

            <!--Flag variable is used here-->
            <div class="mb-3 text-center text-danger" *ngIf="loading">
              <h3>Loading ...</h3>
            </div>
          </div>
        </div>

      </div>
    </div>

  </div>
</ng-container>



<!-- <section class="progress-bar" [hidden]="hideprogressBar">
  <mat-progress-bar color="primary" mode="determinate" [value]="percentDone">
  </mat-progress-bar>
</section>
<mat-card>
  <mat-card-title>File Uploader</mat-card-title>
  <mat-card-content>
      <mat-form-field class="full-width" appearance="outline">
          <mat-label>name</mat-label>
          <ng-template #newFile>
              <mat-label>Choose file</mat-label>
          </ng-template>
          <input matInput disabled>
          <button mat-icon-button matSuffix (click)="fileInput.click()">
              <mat-icon>attach_file</mat-icon>
          </button>
          <input hidden (change)="onSelectFile($event)" #fileInput name="uploadfile" type="file" id="file">
      </mat-form-field>
  </mat-card-content>
  <mat-card-actions>
      <button mat-raised-button color="primary" (click)="onUploadClick()">Upload</button>
  </mat-card-actions>
</mat-card>  -->


```

```
// .NET 7

using Microsoft.AspNetCore.Mvc;
using Newtonsoft.Json;
using NWIWasteWebAPI.Repositories.IRepository;
using NWIWasteWebAPI.Repositories.Models.BlobStorage;
using System.Linq;
using System.Threading.Tasks;
using System.Xml.Linq;

namespace NWIWasteWebAPI.System.API.Controllers
{
 
    [ApiController]
    public class BlobStorageController : ControllerBase
    {
        private readonly IBlobRepository _blobRepository;

        public BlobStorageController(IBlobRepository blobRepository)
        {
            _blobRepository = blobRepository;
        }

        [HttpGet("api/blob/{url}")]
        public async Task<IActionResult> GetBlobFile(string url)
        {
            BlobObject result = await _blobRepository.GetBlobFile(url);

            return File(result.Content, result.ContentType);
        }


        [HttpPost]
        [Route("api/blob/upload/file")]
        public async Task<IActionResult> UploadBlobFile(BlobContentModel model)
        {
            // "C:\\Users\\tset\\Pictures\\a.png
            var result = await _blobRepository.UploadBlobFile(model.FileName, model.FilePath);
            return Ok(result);
        }

        [HttpPost]
        [Route("api/blob/upload/image")]
        public async Task<IActionResult> UploadBlobImage(BlobImageModel model)
        {
            var result = await _blobRepository.UploadBlobImage(model.FileName, model.Image);

            if (result != null)
            {
                var returnData = new { link = result };
                string jsonData = JsonConvert.SerializeObject(returnData);
                return Ok(jsonData);

            }

            return BadRequest();
        }

        [HttpDelete("api/blob/delete")]
        public IActionResult DeleteBlob(string url)
        {
            _blobRepository.DeleteBlob(url);
            return Ok();
        }

        [HttpGet("api/blob/list")]
        public async Task<IActionResult> ListBlobs()
        {
            var result = await _blobRepository.ListBlobs();
            return Ok(result);
        }
 
    }
}

```

```
// Some code

    public class BlobImageModel
    {
        public string FileName { get; set; }
        public string Image { get; set; }
    }
    
    public class BlobImageModel
    {
        public string FileName { get; set; }
        public string Image { get; set; }
    }
    
    public class BlobObject
    {
        public Stream Content { get; set; }
        public string ContentType { get; set; }
    }
```

```
// Some code

using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs;
using NWIWasteWebAPI.Repositories.Models.BlobStorage;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using System;
using NWIWasteWebAPI.Repositories.IRepository;
using System.Linq;
using Microsoft.Extensions.Configuration;

namespace Repositories.Repository
{
    public class BlobRepository : IBlobRepository
    {
        private readonly BlobServiceClient _blobServiceClient;
        private BlobContainerClient client;
        private IConfiguration _configuration { get; }
        public static readonly List<string> ImageExtensions = new List<string> { ".JPG", ".JPEG", ".PNG" };

        public BlobRepository(BlobServiceClient blobServiceClient, IConfiguration iConfig)
        {
            _blobServiceClient = blobServiceClient;
            _configuration = iConfig;
            var blobContainer = iConfig.GetSection("BlobContainer").Value;
            client = _blobServiceClient.GetBlobContainerClient(blobContainer);   /////// Container name
        }
        public async void DeleteBlob(string url)
        {
            var fileName = new Uri(url).Segments.LastOrDefault();
            var blobClient = client.GetBlobClient(fileName);
            await blobClient.DeleteIfExistsAsync();
        }

        public async Task<BlobObject> GetBlobFile(string url)
        {
            var fileName = new Uri(url).Segments.LastOrDefault();

            try
            {
                var blobClient = client.GetBlobClient(fileName);
                if (await blobClient.ExistsAsync())
                {
                    BlobDownloadResult content = await blobClient.DownloadContentAsync();
                    var downloadedData = content.Content.ToStream();

                    if (ImageExtensions.Contains(Path.GetExtension(fileName.ToUpperInvariant())))
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

        public async Task<List<string>> ListBlobs()
        {
            List<string> lst = new List<string>();

            await foreach (var blobItem in client.GetBlobsAsync())
            {
                lst.Add(blobItem.Name);
            }

            return lst;
        }

        public async Task<string> UploadBlobFile(string filePath, string fileName)
        {
            var blobClient = client.GetBlobClient(fileName);
            var status = await blobClient.UploadAsync(filePath);
            Console.WriteLine(status);

            return blobClient.Uri.AbsoluteUri;
        }

        public async Task<string> UploadBlobImage(string fileName, string image)
        {
            var encodedImage = image.Split(',')[1];
            var decodedImage = Convert.FromBase64String(encodedImage);

            var blobClient = client.GetBlobClient(fileName);

            using (var fileStream = new MemoryStream(decodedImage))
            {
                // upload image stream to blob
                var status = await blobClient.UploadAsync(fileStream, true);
                Console.WriteLine(status);
            }

            return blobClient.Uri.AbsoluteUri;
        }
    }
}

```

