# DataTable column re-order

{% code fullWidth="true" %}
```
// install colreorder

  datatables.net-colreorder (v.2.1.0)
 
		
├── datatables.net-colreorder-dt@2.1.0
├── datatables.net-colreorder@2.1.0
├── datatables.net-dt@2.3.1
├── datatables.net-staterestore-dt@1.4.1
├── datatables.net@2.3.1		
						
npm i datatables.net-colreorder@2.1.0 datatables.net@2.3.1 
npm i datatables.net-colreorder-dt@2.1.0 datatables.net-dt@2.3.1

angular.json
   "node_modules/datatables.net-colreorder/js/dataTables.colReorder.js"

```
{% endcode %}

{% code fullWidth="true" %}
```
// Some code

import { CurrentCustomer } from 'src/app/models/current-customer';
import { DataTableConfigService } from 'src/app/services/data-table-config.service';

1.   
  disableOrderColumn = [4];  // Disable ordering for the last column
  isForCustomer = true;
  enableEdit = false;
  constructor(
     private dtConfigService: DataTableConfigService
  ) { }
 
2. 
  ngOnInit(): void {
    let isForCustomer = CurrentCustomer.IsCustomer()
    this.enableEdit = this.dtConfigService.hasAdminOrSupervisorAccess();
    if (this.enableEdit) {
      this.disableOrderColumn = [5];
    }
    else {
      this.disableOrderColumn = [4];
    }

    if (!isForCustomer) {
	  this.getRfidList();
    } else {
     // this.getRfidListByCustomerId(CurrentCustomer.CustomerId());
    }
  }

  
3. 
  buildDtOptions() {
    this.dtOptions = this.dtConfigService.buildDtOptions(
      {
        // pageLength: 25
		// order: [[1, 'asc']],
	   // serverSide: true,   ///<<<<<<< Enable server-side processing for paged list
      },
      this.disableOrderColumn,
      {

 //---------------------------------------------------
        data: this.CarList,    //<--- custom property
        columns: [
          { data: 'id', name: 'id', },
          { data: 'rfidNo', name: 'rfidNo' },
          { data: 'serialNo', name: 'serialNo' },
          {
            data: 'isAdmin', name: 'isAdmin',
	   // orderable: false,    ///<<<<< disable orderable
            render: (data: any, type: any, row: any, meta: any) => {
              if (data) {
                return '<div class="text-center text-success"> Yes </div>';
              }
              else {
                return '<div class="text-center text-danger"> No </div>';
              }
            }
          },
          { data: 'rego', name: 'rego' },

  //---------------------------------------------------
          {
            data: null,
            render: (data: any, type: any, row: any, meta: any) => {
             // console.log('✅changed', this.RfidList.size);
              if (this.enableEdit) {
                return (
                  '<div class="text-center operation-3">' +
                  '<button mat-button class="btn btn-outline-warning btn-sm edit-btn">' +
                  '<i class="fa fa-edit" title="Edit"></i>' +
                  '</button><span class="pe-1"></span>' +
                  '<button mat-button class="btn btn-outline-danger btn-sm delete-btn">' +
                  '<i class="fa fa-trash" title="Delete"></i>' +
                  '</button>' +
                  '</div>'
                );
              } else return '<div></div>';
            },
            createdCell: (td, cellData, rowData, row, col) => {
              const self = this;
              $(td)
                .off('click')
                .on('click', '.edit-btn', (e) => {
                  e.stopPropagation(); // Optional: stop event bubbling
                  self.editDialog('Edit', rowData);
                })
                .on('click', '.delete-btn', function (e) {
                  self.deleteDialog(rowData.id, rowData.name);
                });
            },
			   className: 'sticky-col-right',
          },
        ],
     /// below code is only for paged list
	 extraButtons: this.enableEdit ? [
          'copy',
          {
            text: 'CSV',
            extend: 'csvHtml5',
            className: 'btn-success',
            exportOptions: {
              columns: ':visible:not(.noExport)'
            }
          }, 'pdf', 'excel', 'print',
        ] : [], // Add extra buttons only if the user has edit permissions
      }, // <--- custom property
    );
  }

4. 
  getCarList() {
    this.isLoading = true;
    return this.carrierService.getAllCarriers().subscribe({
      next: (res) => {
        this.CarList = res;
        this.buildDtOptions();   ///<<<<<<<<
        this.dtTrigger.next(this.dtOptions);
        this.isLoading = false;
      },
      error: (error) => {
        console.log('Error: retrive Carrier list error - ' + error);
        this.toastr.warning(error.status + '- retrive data error', 'Fail');

      },
    });
  }
```
{% endcode %}

{% code fullWidth="true" %}
```
// Service

import { Injectable } from '@angular/core';
import { Router } from '@angular/router';
import { ToastrService } from 'ngx-toastr';


@Injectable({
  providedIn: 'root'
})
export class DataTableConfigService {
  constructor(
    private toastr: ToastrService,
    private router: Router
  ) { }

  buildDtOptions(config?: Partial<any>, disableOrderColumn: number[] = [], customProps?: any): any {
    let component = this.getCurrentComponent();
    if (!component) {
      console.error('Component name not found. Please ensure the component is properly configured.');
      return {};
    }
    let component_name = component.name.toLowerCase() + '_table_state'; // Ensure the name is in lowercase and ends with '_table_state'

    return {
      scrollY: 720,
      scrollCollapse: true,
      autoWidth: true,
      columnDefs: [
        { orderable: false, targets: disableOrderColumn, className: 'text-center' },
        { targets: '_all', className: 'text-center' } // Center align all columns
      ],
      pagingType: 'full_numbers',
      pageLength: 15,
      lengthMenu: [10, 15, 20, 25, 50, 100],
      colReorder: true,
      responsive: true,
      stateSave: true,
      fixedHeader: true,
      fixedColumns: true,
      ordering: true,
      processing: true,  ///<<<<<<< Enable processing indicator
      ...config,       // Allow override
      ...customProps,  // Add custom props like datatable_name

      stateSaveCallback: (settings: any, data: any) => {
        const { time, ...dataWithoutTime } = data;
        const newState = JSON.stringify(dataWithoutTime);
        const parsedState = localStorage.getItem(component_name);

        if (parsedState) {
          const existingState = JSON.parse(parsedState);
          const { time, ...existingWithoutTime } = existingState;
          const oldState = JSON.stringify(existingWithoutTime);
          // Compare the new state with the existing state

          if (oldState !== newState) {
            localStorage.setItem(component_name, JSON.stringify(data));
            // console.log('✅changed', newState);
          }
          else {
            // console.log('🔄 no changed');
          }
        } else {
          localStorage.setItem(component_name, JSON.stringify(data));
        }
      },
      stateLoadCallback: (settings: any) => {
        const state = localStorage.getItem(component_name);
        return state ? JSON.parse(state) : null;
      },
      stateDuration: 0, // 1:no time limit for localStorage,  (1day）= 60 * 60 * 24, -1:for sessionStorage,

      dom:
        "<'row'<'col-sm-3'l><'col-sm-6 text-center'B><'col-sm-3 pull-right'f>>" +
        "<'row'<'col-sm-12'tr>>" +
        "<'row'<'col-sm-5'i><'col-sm-7 text-right'p>>",
      buttons:  [
        {
          extend: 'createState',
          text: 'Create State', config: {
            creationModal: true,
            toggle: {
              columns: {
                name: true,
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
          }
        },
        'savedStates',
        {
          extend: 'removeAllStates',
          text: 'Remove All States',
          action: (e, dt: any, node, config) => {
            this.toastr.toastrConfig.positionClass = 'toast-top-center';
            this.toastr.clear();
            this.toastr.warning('Reset all statuses to default.', 'Warning');
            dt.colReorder.reset();
            localStorage.removeItem(component_name);
            // Remove all saved states
            const states = dt.stateRestore.states();
            if (states && typeof states === 'object') {
              for (const stateName in states) {
                if (states[stateName]?.remove instanceof Function) {
                  states[stateName].remove();
                }
              }
            }

            // Reset the initial columns visibility
            dt.columns().every((index) => {
              dt.column(index).visible(true);
            });

            // Reset the column reordering
            dt.colReorder.reset();
            localStorage.removeItem(component_name);
            dt.draw(); // Redraw after modification
            this.toastr.toastrConfig.positionClass = 'toast-top-right';
          },
        },
        //'colvis',   // Column visibility button
        {
          extend: 'colvis',
          text: 'Column Visibility',
          className: 'buttons-colvis',
          columns: ':not(.noVis)' // Exclude columns with the class 'noVis'
        },
         ...(customProps?.extraButtons || [])  // Allow additional buttons to be added dynamically
      ]


    };
  };

  getCurrentComponent(): any {
    let route = this.router.routerState.root;
    while (route.firstChild) {
      route = route.firstChild;
    }
    return route.routeConfig?.component ?? null;
  }

  getCurrentComponentName(): string {
    const component = this.getCurrentComponent();
    return component ? component.name.toLowerCase() : '';
  }

  hasAdminOrSupervisorAccess(): boolean {
    let currentRole = sessionStorage.getItem('Role');
    if (currentRole === 'Administrator' || currentRole === 'Supervisor') {
      return true
    }
    else {
      return false;
    }
  }

}


```
{% endcode %}
