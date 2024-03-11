# Print and save to PDF

```
// Some code

npm install jspdf --save
npm install @types/jspdf --save-dev


<div #dataToExport class="container" id="htmlData">
... print content ...
</div
<div mat-dialog-actions align="center"  >
    <button class="btn btn-outline-info btn-sm" (click)='print()'><i class="fa fa-print" title="Print"></i></button>&nbsp; &nbsp;
    <button class="btn btn-outline-danger btn-sm" (click)='savePDF()'><i class="fa fa-file-pdf-o" title="PDF"></i></button> &nbsp; &nbsp;
    <button class="btn btn-outline-info btn-sm" mat-dialog-close> <i class="fa fa-sign-out"></i></button> &nbsp; &nbsp;
</div>



@ViewChild('dataToExport', { static: false })
export class PrintPageComponent implements OnInit {
  pdfName = "test.pdf";

  print() {
    window.print();
  }

  savePDF(): void {
    let DATA: any = document.getElementById('htmlData');
    html2canvas(DATA).then((canvas) => {
      let fileWidth = 208;
      let fileHeight = (canvas.height * fileWidth) / canvas.width;
      const FILEURI = canvas.toDataURL('image/png');
      let PDF = new jsPDF('p', 'mm', 'a4');
      let position = 0;
      PDF.addImage(FILEURI, 'PNG', 0, position, fileWidth, fileHeight);
      PDF.save(this.pdfName);
    });
  }

}
```
