# Datatables save state using localStorage

{% code fullWidth="true" %}
```typescript
/// auto save and restore
this.dtOptions = {
  。。。
  
  stateSave: true,  // Save the state of the table(pagination, sorting, filtering, etc.) 
  stateSaveCallback: function (settings: any, data: any) {
     localStorage.setItem('DataTables_weighing', JSON.stringify(data));
  },
  stateLoadCallback: function (settings: any) {
    let savedState =  localStorage.getItem('DataTables_weighing') || '{}';
    // If no saved state, return empty state to prevent errors
    if (savedState) {
      try {
        return JSON.parse(savedState);
      } catch (e) {
        console.error('Error loading saved DataTable state:', e);
        return {}; // Return an empty state if parsing fails
      }
    } else {
      return {}; // If no saved state, return empty object
  }
  },
  stateDuration: 0, // 1:no time limit for localStorage,  (1day）= 60 * 60 * 24, -1:for sessionStorage,

}
```
{% endcode %}

{% code fullWidth="true" %}
```
// manual processing

import { Api } from 'datatables.net';
 
dtInstance!: Api;


ngOnInit() {
   this.buildDtOptions();
   this.dtOptions.initComplete = (settings: any) => {
       // get instance
       this.dtInstance = settings.oInstance.api();
       const savedState =  localStorage.getItem('datatableState');

       if (savedState) {
            let jsonState = JSON.parse(savedState);
           // this.dtInstance.state.loaded(() => jsonState);   //not work
            this.dtInstance.draw(false);
       }
    
       // save state on draw event
       this.dtInstance.on('draw', () => {
            this.saveDataTableState();
        });
   }
}


saveDataTableState() {
  const visibleColumns = this.getVisibleColumns();
  console.log('showing columns:', visibleColumns);

  let page = this.dtInstance.page.len();
  console.log('current selected page number:', page);
  if (this.dtInstance) {
    const currentState = this.dtInstance.state();
    let jsonState = JSON.stringify(currentState);
    console.log(`Current state: ${jsonState}`);
     localStorage.setItem('datatableState', jsonState);
  }
}


saveVisibleColumns() {
  const allColumns = this.dtInstance.columns();
  const visibleColumns: string[] = [];
  allColumns.every((columnIndex) => {
    const column = this.dtInstance.column(columnIndex);
    if (column.visible()) {
      const headerText = column.header()?.textContent ?? '';
      visibleColumns.push(headerText);
    }
    return true;
  });
   localStorage.setItem('columnVisibility', JSON.stringify(visibleColumns));
  return visibleColumns;
}

restoreVisibleColumns() {
  const savedVisibleColumns =  localStorage.getItem('columnVisibility');
  if (savedVisibleColumns) {
    const visibilityArray = JSON.parse(savedVisibleColumns);
    visibilityArray.forEach((visible: boolean, index: number) => {
      this.dtInstance.column(index).visible(visible);
    });
  }
}


<button (click)="saveDataTableState()">Save State</button> 
```
{% endcode %}
