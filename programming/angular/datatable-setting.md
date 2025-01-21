# Datatable setting

* Column display using ajax

{% code fullWidth="true" %}
```
Data with two decimal places and align right:
   return '<span  class="text-info" style="float: right;">' + formatNumber(Number(row.weight2P1), locale, '1.2-2') + '</span>';

Data align: center (div)
   return '<div class="text-success" style="text-align: center;"> Yes</div>';

Datetime:
   return '<span  class="text-success" >'+ formatDate(new Date(data), 'dd/MM/yyyy hh:mm:ss a', 'en-AU'
input.ng-invalid.ng-touched {
  border: rgb(233, 168, 168) 1px solid;
  box-shadow: 0px 0px 6px  #9191ee;
}


```
{% endcode %}



* input colour, field focus color

{% code fullWidth="true" %}
```

input.ng-invalid.ng-touched {
  border: rgb(233, 168, 168) 1px solid;
  box-shadow: 0px 0px 6px  #9191ee;
}

```
{% endcode %}

* <pre data-full-width="true"><code><strong>/input, select field focus colour
  </strong>-----------------------------------
      datatables.net@2.1.6
  -----------------------------------
  .dt-container .dt-length {
    select:focus {
      outline: none !important;
      border: 1px solid rgb(213, 213, 245);
      box-shadow: 0px 0px 6px  #9191ee;
    }
  }

  .dt-container .dt-search {
    input:focus {
      outline: none !important;
      border: 1px solid rgb(213, 213, 245);
      box-shadow: 0px 0px 6px  #9191ee;
    }
  }

  -----------------------------
    datatables.net@1.13.4
  -----------------------------

  .dataTables_length {
    select:focus {
      outline: none !important;
      border: 1px solid rgb(213, 213, 245);
      box-shadow: 0px 0px 6px  #9191ee;
    }
  }

  .dataTables_filter {
    input:focus {
      outline: none !important;
      border: 1px solid rgb(213, 213, 245);
      box-shadow: 0px 0px 6px  #9191ee;
    }
  }

  </code></pre>
*   Table line-height\


    {% code fullWidth="true" %}
    ```scss
    ::ng-deep td {
      padding: 5px !important;
      line-height: 36px !important;
    }


    table.table tr {
     line-height: 26px !important;
    }

    ```
    {% endcode %}
*   Button position\


    {% code fullWidth="true" %}
    ```typescript
    dom:
          "<'row'<'col-sm-3'l><'col-sm-6 text-center'B><'col-sm-3 pull-right'f>>" +
          "<'row'<'col-sm-12'tr>>" +
          "<'row'<'col-sm-5'i><'col-sm-7 text-right'p>>",
    buttons: [
      {
        extend: 'createState',
        config: {
          creationModal: true,
          toggle: {
            columns: {
              search: true,
              visible: true,
            },
            length: true,
            order: true,
            paging: true,
            scroller: true,
            search: true,
            searchBuilder: true,
            searchPanes: true,
            select: true,
          },
        },
      },
      'savedStates',
      'removeAllStates',
      'colvis', 'copy', 'csv', 'pdf', 'excel', 'print',
    ],

    ```
    {% endcode %}

    \


    {% code fullWidth="true" %}
    ```scss
    ::ng-deep.dt-type-numeric.sorting_1{
      padding: 4px!important;
    }

    th{
      line-height: 30px !important;
    }

    ::ng-deep.dt-info {
      padding-top: 12px!important;
    }

    ::ng-deep .dt-paging {
      padding-top: 5px!important;
    }

    ::ng-deep.dt-search{
      text-align: right!important;
    }

    div{
      overflow-x: hidden;
    }

    ```
    {% endcode %}
*   ajax\


    {% code fullWidth="true" %}
    ```typescript
      ajax: (dtParameters: any, callback: any) => {
        this.isLoading = true;
        this.weighingService.getAllWeighingPagedList(dtParameters, customerName)
          .pipe(
            finalize(() => {
              this.isLoading = false;
            })
          )
          .subscribe({
            next: resp => {
              callback({
                recordsTotal: resp.recordsTotal,
                recordsFiltered: resp.recordsFiltered,
                data: resp.data  // set data
              });

              this.isLoading = false;
              // setTimeout(() => {
              //   $($.fn.dataTable.tables(true)).DataTable().columns.adjust();
              // }, 200);
            },
            error: error => {
              this.toastr.error(error)
            }
          });
      },

      columns: [{
        data: 'transactionno', render: function (data: any, type: any, row: any) {
          if (row.status != 'F')
            return '<span style="color:red; display: flex; justify-content: center; align-items: center; ">' + data + '</span> '
          else
            return '<span style="color:blue; display: flex; justify-content: center; align-items: center; ">' + data
              + ' <button mat-button class="btn btn-sm" (click)="print("print",dataItem)> <span class="pointer" style="font-size:larger"><i class="fa fa-print text-success" title="Print docket"></i></span></button> ' +
              '</span>'
        }
      },
      { data: 'binInfo' },
      
      {
        data: 'carrier', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-info" style="float: right;">' + row.carrier + '</span>';
          else
            return ''
        }
      },
      { data: 'rego' },
      {
        data: 'product', render: function (data: any, type: any, row: any) {

          return '<span  class="text-warning">' + row.product + '</span>';
        }
      },
      {
        data: 'unitPrice', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-success" style="float: right;">' + formatNumber(Number(row.unitPrice), locale, '1.2-2') + '</span>';
          else
            return ''
        }
      },
      { data: 'weighType' },
      {
        data: 'net', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-success" style="float: right;">' + formatNumber(Number(row.net), locale, '1.2-2') + '</span>';
          else
            return ''
        }
      },
      {
        data: 'tare', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span style="float: right;">' + formatNumber(Number(row.tare), locale, '1.2-2') + '</span>';
          else
            return ''
        }
      },
      {
        data: 'gross', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-success" style="float: right;">' + formatNumber(Number(row.gross), locale, '1.2-2') + '</span>';
          else
            return ''
        }

      },
      {
        data: 'timestamp1', render: function (data: any, type: any, row: any) {
          return '<span class="text-success" >' +
            formatDate(new Date(data), 'dd/MM/yyyy hh:mm:ss a', 'en-AU')
        }
      },
      {
        data: 'weight1', render: function (data: any, type: any, row: any) {
          return '<span class="text-success" style="float: right;">' + formatNumber(Number(row.weight1), locale, '1.2-2') + '</span>';
        }
      },
      {
        data: 'weight1P1', render: function (data: any, type: any, row: any) {
          if (data != null && data != 0)
            return '<span class="text-success"  style="float: right;">' + formatNumber(Number(row.weight1P1), locale, '1.2-2') + '</span>';
          else
            return ''
        }
      },
      {
        data: 'timestamp2', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-info">' +
              formatDate(new Date(data), 'dd/MM/yyyy hh:mm:ss a', 'en-AU');
          else
            return ''
        }
      },
      {
        data: 'weight2', render: function (data: any, type: any, row: any) {
          if (data != null)
            return '<span  class="text-info" style="float: right;">' + formatNumber(Number(row.weight2), locale, '1.2-2') + '</span>';
          else return ''
        }
      },
      {
        data: 'weight2P1', render: function (data: any, type: any, row: any) {
          if (data != null && data != 0)
            return '<span class="text-info" style="float: right;">' + formatNumber(Number(row.weight2P1), locale, '1.2-2') + '</span>';
          else return ''
        }
      },
      { data: 'bookingNo' },
      {
        data: 'status', render: function (data: any, type: any, row: any) {
          return '<span class="text-info" style="text-align: center">' + data + '</span>';
        }
      },
      ],
      rowCallback: (row: Node, data: any[] | Object, index: number) => {
        const self = this;
        // Unbind first in order to avoid any duplicate handler
        // (see https://github.com/l-lin/angular-datatables/issues/87)
        // Note: In newer jQuery v3 versions, `unbind` and `bind` are
        // deprecated in favor of `off` and `on`

        $('button', row).off('click');
        $('button', row).on('click', () => {
          self.printClickHandler(data);
        });
        return row;
      }
    };
    }

    printClickHandler(weighingData: any): void {
    this.printDialog(weighingData);
    }

    printDialog(weighingData: any) {
    this.dialog.open(PrintticketComponent, {

      width: '780px',
      height: 'auto',
      data: { weighingData }
    })
      .afterClosed().subscribe(result => {
        console.log(`Dialog result: ${result}`);
      });
    }
    ```
    {% endcode %}
*   sticky column

    ```scss
    .stickyColumn {
      position: sticky !important;
      left: 0 !important;
      background-color: rgb(215, 219, 214);
      // overflow: hidden;
      z-index: 1;
    }
    ```



{% code fullWidth="true" %}
```
//  it be used for paged list

::ng-deep .sticky-col {
  position: sticky;
  left: 0;
  background-color:rgb(231, 233, 230) !important;
  z-index: 12;
}



columns: [
  {
    data: 'id', render: function (data: any, type: any, row: any) {
      if (role === 'Administrator' || role === 'Supervisor') {
        let content = '<div style="text-align: center;" ><button mat-button (click)="editDialog("Edit",dataItem)" class="btn btn-outline-warning btn-sm edit-btn">' +
          '<i class="fa fa-edit" title="Edit"></i></button> <span class="pe-1"></span>'
        if (!isForCustomer) {
          content = content + ' <button mat-button (click)="deleteDialog(dataItem.id, dataItem.rego)" class="btn btn-outline-danger btn-sm delete-btn">' +
            '<i class="fa fa-trash" title="Delete"></i></button></div>'
        }
        return '<div style=" width:80px; background-color: rgb(230, 233, 230)!important">' + content + '</div></div>'
      }
      else
        return ''
    },
   className: 'sticky-col'   ///<<<<<<<<<
  },


```
{% endcode %}
