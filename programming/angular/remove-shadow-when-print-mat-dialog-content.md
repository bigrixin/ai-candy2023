# Remove shadow when print mat-dialog content

````
// Some code

```scss
@media print {
  #printButton {
    display: none;
  }
}


.mat-dialog-content {
  max-height: initial;
}

::ng-deep.mat-dialog-container {
  box-shadow: none !important;
}

```


```html
<div #dataToExport class="container" id="htmlData">
  <div style="font-size: medium;  font-weight: bold;" align="center">Mainstream Recycling</div><br><br>
  <table style="width:100%">
    <tr>
      <td> Ticket No.</td>
      <td class="table-width-m"></td>
      <td>{{ data.weighingData.transactionno }}</td>
    </tr>
  .....
  
 
  </table>

</div>
<div mat-dialog-actions align="center" id="printButton">
  <button class="btn btn-outline-info btn-sm" (click)='print()'><i class="fa fa-print" title="Print"></i></button>&nbsp;
  &nbsp;
  <button class="btn btn-outline-danger btn-sm" (click)='savePDF()'><i class="fa fa-file-pdf-o"
      title="PDF"></i></button> &nbsp; &nbsp;
  <button class="btn btn-outline-info btn-sm" mat-dialog-close> <i class="fa fa-sign-out"></i></button> &nbsp; &nbsp;
</div>

```

````
