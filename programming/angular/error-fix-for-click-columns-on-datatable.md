# Error fix for click columns on Datatable

{% code fullWidth="true" %}
```
 //------------------ UI --------------------------------------------------------------------------------
   columnDefs: [{ orderable: false, targets: [this.disableOrderColumn,2] }],     ///<<<<<<< disable order by
   order: [[1, 'asc']],

      ajax: (dtParameters: any, callback: any) => {
        this.isLoading = true;
        if (!dtParameters.order || dtParameters.order.length === 0) {    
          dtParameters.order = [{ column: 0, direction: 'asc' }];                           /// <<<<<<< Reset missing parameters
        }


//----------------- API ---------------------------------------------------------------------------------
        public PagedList<Vehicle> GetAllVehiclePagedListRecords(PageParameters pageParameters)
        {
            string orderBy = pageParameters.Order[0].Dir;
            int orderByNumber = pageParameters.Order[0].Column;
            string columnName = pageParameters.Columns[orderByNumber].Data;

            switch (columnName.ToLower())     ///<<<<<<< change name to id
            {
                case "product":
                    columnName = "ProductId";
                    break;

                case "vehiclezone":
                    columnName = "ZoneId";
                    break;
                default:
                    break;
            }

            return PagedList<Vehicle>.ToPagedList(FindAll()
                .Include(c => c.Carrier)
                .Where(x => x.Rego.Contains(pageParameters.SearchText)

                        || x.VehicleCode.Contains(pageParameters.SearchText)
                        || x.Id.ToString().Contains(pageParameters.SearchText)
                        || x.Tare.ToString().Contains(pageParameters.SearchText)
                        || x.Carrier.Name.ToString().Contains(pageParameters.SearchText)   ///<<<<<<< for child table
                        )
               .AsQueryable()
               .OrderBy(columnName + " " + orderBy),
                pageNumber,
                pageParameters.Length);

}
```
{% endcode %}
