# Datatables fixed title

scrollY: this.getScrollY(),

private getScrollY(): string {\
// A CSS calc() value (not a computed px number) so the scroll body height\
// tracks the viewport on resize automatically, with no JS resize listener needed.\
// 380px reserves room for the sticky top nav plus DataTables' own toolbar/pagination rows.\
return 'calc(100vh - 300px)';\
}

```
// Some code

  buildDtOptions(config?: Partial<any>, disableOrderColumn: any[] = [], customProps?: any): any {
    let component = this.getCurrentComponent();
    let defaultPageLength = 15;
    let component_name = '';

    if (!component) {
      console.error('Component name not found. Please ensure the component is properly configured.');
      // Return minimal valid configuration instead of empty object
      component_name = 'default_table_state';
    } else {
      component_name = component.name.toLowerCase() + '_table_state'; // Ensure the name is in lowercase and ends with '_table_state'
    }

    return {
      scrollY: this.getScrollY(),   ///<<<<<<<<<<<<<<<<<<<<<<
      scrollX: true,
      scrollCollapse: true,
      autoWidth: true,
      columnDefs: [
        { orderable: false, targets: disableOrderColumn, className: 'text-center' },
        { orderable: false, targets: '.non-orderable' }, // Disable ordering for .non-orderable columns
        { targets: '_all', className: 'text-center' }, // Center align all columns
      ],
```
