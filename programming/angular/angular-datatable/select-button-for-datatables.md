# Select button for datatables



{% code fullWidth="true" %}
````
// Some code
```html
  <table #table class="table table-bordered table-striped table-hover table-wrapper-scroll-y" datatable [dtOptions]="dtOptions"
    [dtTrigger]="dtTrigger" style="width:100%" >
    <thead class="table-dark">
      <tr align="center" class="align-middle center">
        <th>Id</th>
        <th>QR Code </th>
        <th>Download</th>
        <th class="text-warning" style="width: 60px;">Print<br>
          <!-- <a href="#/qrcode/batch-print">  -->
            <button type="button" mat-button class="btn btn-outline-info btn-sm mb-1" (click)="BatchPrint()">
            <i class="fa fa-print" title='Batch print QrCode'></i>
           </button><br>
           
           <button style="cursor:pointer;" mat-button class="btn  btn-sm mb-1"  >
            <input style="cursor:pointer;" type="checkbox"
            (change)="onCheckBoxPrintAll($event, isPrintAll?true:false)"  />

           </button>
          <!-- </a> -->
        </th>
```

```html
 <tbody>
      <tr *ngFor="let dataItem of QrcodeList" scope="row" align="center">
        <td>{{dataItem.id}}</td>
 
        <td>
          <input style="cursor:pointer;" type="checkbox" [value]="dataItem.guId"
            (change)="onCheckboxChange(dataItem.guId, dataItem.name, $event)"  />

        </td>
        
        ....
```
````
{% endcode %}

{% code fullWidth="true" %}
```
// Select all button

  onCheckBoxPrintAll(e: any, isprint: any) {
    this.isPrintAll = !isprint;
    let rows = document.getElementsByTagName('tbody')[0]?.childNodes;
    if (this.isPrintAll) {
      this.selectedQrCodeArray = [];
      if (rows) {
        rows.forEach((row) => {
          if (row.childNodes.length > 0) {
            let guiId = row.childNodes[1].textContent?.trim();
            let name =
              row.childNodes[5].textContent?.trim() +',' +
              row.childNodes[6].textContent?.trim() +',' +
              row.childNodes[7].textContent?.trim() +',' +
              row.childNodes[8].textContent?.trim();
            if (guiId && name) {
              this.selectedQrCodeArray.push({
                guId: guiId,
                name: name,
              });
            }
            let checkBox = row.childNodes[3].childNodes[0] as HTMLInputElement;
            checkBox.checked = true;
          }
        });
      }
    } else {
      if (rows) {
        rows.forEach((row) => {
          if (row.childNodes.length > 0) {
            let checkBox = row.childNodes[3].childNodes[0] as HTMLInputElement;
            checkBox.checked = false;
          }
        });
      }

      for (let i = 0; i < this.selectedQrCodeArray.length; i++)
        this.selectedQrCodeArray.splice(i, 1);
    }
    this.qrcodeService.setSelectedQrCodeList(this.selectedQrCodeArray);
  }

// Select button

  onCheckboxChange(guiId: string, name: string, event: any) {
    const index = this.selectedQrCodeArray.findIndex((x) => x.guId === guiId);
    if (event.target.checked) {
      if (index == -1) {
        this.selectedQrCodeArray.push({
          guId: guiId,
          name: name.replace(/-/gi, ', '), ///replace all '-' to ','
        });
      }
    } else {
      this.selectedQrCodeArray.splice(index, 1);
    }

    this.qrcodeService.setSelectedQrCodeList(this.selectedQrCodeArray);
  }

```
{% endcode %}
