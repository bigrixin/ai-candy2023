# Angular-datatables,   dd/MM/yyyy,  sorting

{% code fullWidth="true" %}
```
// Angular-datatables,   date format is dd/MM/yyyy,  sorting


ngOnInit(): void {
   // date type: dd/mm/yyyy
  ($.fn.dataTable.ext.type.order as any)['date-type-pre'] = (dateStr: string) => {
    if (!dateStr) return 0;
    const parts = dateStr.split('/');
    const day = parseInt(parts[0], 10);
    const month = parseInt(parts[1], 10) - 1;
    const year = parseInt(parts[2], 10);
    return new Date(year, month, day).getTime();
  };

  this.buildDtOptions();
}


buildDtOptions() {
  this.dtOptions = {
    pagingType: 'full_numbers',
    pageLength: 15,
    columnDefs: [
      {
        targets: 2,                 // index of the column to be sorted
        type: 'date-type'     // date type
      }
    ],
}
}


```
{% endcode %}
