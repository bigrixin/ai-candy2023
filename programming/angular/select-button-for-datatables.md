# Select button for datatables



{% code fullWidth="true" %}
````
// Some code
```typescript

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
````
{% endcode %}
