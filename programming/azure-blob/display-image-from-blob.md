# Display image from Blob

```
// TS
  getBlobImage() {
    return this.blobService.getBlobImageByName(imageUrl)
      .subscribe({
        next: res => {
          this.createImageFromBlob(res);
          this.isImageLoading = false;
        },
        error: error => {
          console.log("Retrive Operator list error: " + error);
          this.toastr.warning(error.status + "- retrive data error", 'Fail');
          this.isImageLoading = false;
        },
        complete: () => console.log('Observer got a complete notification'),
      });
  }

  createImageFromBlob(image: Blob) {
    let reader = new FileReader();
    reader.addEventListener("load", () => {
      this.imageToShow = reader.result;
    }, false);

    if (image) {
      reader.readAsDataURL(image);
    }
  }

```

```
// UI
<mat-dialog-content>
  <div>
    <img [src]="imageToShow" style="width:450px;height:350px;"
      *ngIf="!isImageLoading; else noImageFound">
    <ng-template #noImageFound>
      <i class="bi bi-hourglass-split fs-3 text-warning">&nbsp; &nbsp; Image loading ...</i>
    </ng-template>

  </div>
</mat-dialog-content>
```

````
// Services

```typescript
  getBlobImageByName(fileName: any): Observable<any> {
    let endPoint = this.rootURL + 'api/blob/image';
    let params = fileName;
    const options = {headers: this.headers,  responseType: 'blob' as 'blob'};
    return this.httpClient.get(endPoint+'/?fileName='+fileName,options).pipe(resp => resp);
  }
```
````

```
// API

[HttpGet("api/blob/image")]
public async Task<IActionResult> GetBlobFile(string fileName)
{
    string url = _blobRepository.GetBlobURL();
    BlobObject result = await _blobRepository.GetBlobFile(url + fileName);
    if (result != null)
    {
        var fileStreamResult = File(result.Content, result.ContentType);
        return fileStreamResult;
    }

    return BadRequest();
}
```

