# A sample usage

{% code fullWidth="true" %}
```
// Some code for datatable
+-- angular-datatables@13.1.0

+-- @types/datatables.net-buttons@1.4.7
+-- @types/datatables.net@1.10.24
+-- datatables.net-buttons-dt@2.3.6
+-- datatables.net-buttons@2.3.6
+-- datatables.net-dt@1.13.4
+-- datatables.net-staterestore-dt@1.2.2
+-- datatables.net@1.13.4


-------------------------
  .ts
-------------------------
  @ViewChild(DataTableDirective, { static: false })
  dtElement!: DataTableDirective;
  dtOptions: any;
  dtTrigger: Subject<any> = new Subject<any>();


  ngOnInit(): void {
    this.buildDtOptions();
  }

-------------method 1----------------

  buildDtOptions() {
    this.dtOptions = {
      scrollY: 720,
      scrollX: true,   /// table title and content can move, update and delete buttons are fixed
      scrollCollapse: true,
      autoWidth: true,
      pagingType: 'full_numbers',
      pageLength: 15,
      lengthMenu:
        [10, 15, 20, 25, 50, 100],
    };
  }

-------------method 2----------------

  buildDtOptions() {
    this.dtOptions = {
      scrollY: 720,
      scrollCollapse: true,
      autoWidth: true,
      pagingType: 'full_numbers',
      pageLength: 15,
      lengthMenu: [
        [10, 15, 20, 50, 100, 500, -1],
        [10, 15, 20, 50, 100, 500, 'All'],
      ],
      stateSave: true,

      dom: 'Blfrtip',
      buttons: [
        {
          extend: 'createState',
          config: {
            creationModal: true,
            toggle: {
              columns: {
                search: true,
                visible: true
              },
              length: true,
              order: true,
              paging: true,
              scroller: true,
              search: true,
              searchBuilder: true,
              searchPanes: true,
              select: true
            }
          }
        },
        'savedStates',
        'colvis'
      ],
    };
  }


  <table class="table table-bordered table-striped table-hover " datatable [dtOptions]="dtOptions" [dtTrigger]="dtTrigger" style="width:100%">

-------------------------
  .html (method 1)
-------------------------
<div class="container-fluid h-100">
  <table class="table table-bordered table-striped table-hover table-wrapper-scroll-y" datatable [dtOptions]="dtOptions"
    [dtTrigger]="dtTrigger" style="width:100%">
    <thead class="table-dark">
      <tr align="center" class="align-middle center">
        <th>Id</th>
        <th>.. </th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let dataItem of WasteTypeList" scope="row">
        <td align="center">{{dataItem.id}}</td>

        <td>
          {{dataItem.name}}
        </td>
      </tr>
    </tbody>
  </table>
</div>
-------------------------
  Module.ts:
-------------------------
 imports: [DataTablesModule]



```
{% endcode %}
