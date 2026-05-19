---
description: 'Get data by paged list: 1. Angular Datatable Server Side Query, 2. .Net Core'
---

# Angular DataTable server side big data query

├── @types/datatables.net-buttons@1.4.7 \
├── @types/datatables.net@1.10.21\
├── angular-datatables@13.0.1 \
├── datatables.net-buttons-dt@2.2.2 \
├── datatables.net-buttons@2.2.2 \
├── datatables.net-dt@1.12.1 \
├── datatables.net-staterestore-dt@1.1.1 \
├── datatables.net@1.11.4

{% code fullWidth="true" %}
```
// Html
	<div align="right" class="pb-1" >
	  <div class="controls" *ngIf="isLoading">
		<mat-progress-bar mode="indeterminate" color="primary" aria-label="Loading content"> </mat-progress-bar>
	  </div>
	</div>
	<div class="container-fluid h-100">
	  <table id ="recordtable" class="table table-bordered table-striped table-hover" datatable [dtOptions]="dtOptions"
		 style="width:100%">
		<thead class="table-dark">
		  <tr align="center" class="align-middle center">
			<th>Tech Name</th>
			<th>Customer</th>
			<th>Job No</th>
			<th>Record No</th>

		  </tr>
		</thead>
		<tbody>
		  <tr *ngFor="let dataItem of RecordList" scope="row">
			<td>{{dataItem.techName}}</td>
			<td>{{dataItem.customerName}}</td>
			<td>{{dataItem.jobNo}}</td>
			<td><span class="badge bg-info" > {{dataItem.natano}} </span></td>
		  </tr>
		</tbody>
	  </table>
	</div>
```
{% endcode %}

{% code fullWidth="true" %}
```
// Component.ts
 buildDtOptions() {
    ///https://datatables.net/reference/option/dom
   // var colSettings = [];
    this.dtOptions = {
      scrollY: 750,
      scrollX: true,
      scrollCollapse: true,
      order: [[0, 'desc']],
      ordering: true,
      pagingType: 'full_numbers',
      pageLength: 20,
      processing: true,
      autoWidth: false,
      lengthMenu: [
        [10, 20, 50, 100, 1000, -1],
        [10, 20, 50, 100, 1000, 'All'],
      ],
      columnDefs: [
        {
          width: "auto",
          targets: [1],
          visible: false
        }],
      fixedColumns: true,
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
        'removeAllStates',
        'colvis',
        'copy', 'csv', 'pdf', 'excel','print',
      ],
      serverSide: true,
      responsive: true,

      ajax: (dtParameters: any, callback: any) => {
        this.isLoading = true;
        this.weighingService.getDataTablesDataPagedList(dtParameters)
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
            },
            error: error => {
              this.toastr.error(error)
            }
          });

      },
      columns: [
        { data: 'no' },
        { data: 'customer' },
        { data: 'amount' },
        { data: 'rego' },
        { data: 'status' },
        { data: 'timestamp' },
      ],

    };

  }
```
{% endcode %}

{% code fullWidth="true" %}
```
// Service.ts

	//dtParameters: DataTablesOptions
	public getDataTablesDataPagedList(dtParameters: any): Observable<DataTableResponse> {
	const body=JSON.stringify(dtParameters);
	const headers = { 'Content-Type': 'application/json'};
	return this.httpClient.post<DataTableResponse>(this.rootURL + 'api/natarecord/pagelist', body,{ headers}).pipe(map(res => res));
	}

```
{% endcode %}

{% code fullWidth="true" %}
```
// Model

	export class DataTableResponse {
		data!: any[];
		draw!: number;
		recordsFiltered!: number;
		recordsTotal!: number;
	}

```
{% endcode %}

{% code fullWidth="true" %}
```
// angular.json
            "styles": [
              "./node_modules/@angular/material/prebuilt-themes/indigo-pink.css",
              "src/scss/styles.scss",

              "node_modules/ngx-toastr/toastr.css",
              "node_modules/datatables.net-dt/css/jquery.dataTables.min.css",
              "node_modules/datatables.net-buttons-dt/css/buttons.dataTables.min.css",
              "node_modules/datatables.net-staterestore-dt/css/stateRestore.dataTables.min.css"
            ],
            "scripts": [
              "node_modules/jquery/dist/jquery.js",
              "node_modules/jszip/dist/jszip.js",
              "node_modules/datatables.net/js/jquery.dataTables.min.js",
              "node_modules/datatables.net-buttons/js/dataTables.buttons.min.js",
              "node_modules/datatables.net-buttons/js/buttons.colVis.min.js",
              "node_modules/datatables.net-buttons/js/buttons.flash.min.js",
              "node_modules/datatables.net-buttons/js/buttons.html5.min.js",
              "node_modules/datatables.net-buttons/js/buttons.print.min.js",
              "node_modules/datatables.net-staterestore/js/dataTables.stateRestore.min.js"
            ],

```
{% endcode %}

{% code fullWidth="true" %}
```
// .Net Core Page Service

	public class PagedList<T> : List<T>
	{
		public int CurrentPage { get; private set; }
		public int TotalPages { get; private set; }
		public int PageSize { get; private set; }
		public int TotalCount { get; private set; }

		public bool HasPrevious => CurrentPage > 1;
		public bool HasNext => CurrentPage < TotalPages;

		public PagedList(List<T> items, int count, int pageNumber, int pageSize)
		{
			TotalCount = count;
			PageSize = pageSize;
			CurrentPage = pageNumber;
			TotalPages = (int)Math.Ceiling(count / (double)pageSize);

			AddRange(items);
		}

		public static PagedList<T> ToPagedList(IQueryable<T> source, int pageNumber, int pageSize)
		{
			var count = source.Count();
			var items = source.Skip((pageNumber - 1) * pageSize).Take(pageSize).ToList();

			return new PagedList<T>(items, count, pageNumber, pageSize);
		}
	}

```
{% endcode %}

{% code fullWidth="true" %}
```
// .Net Core Page Model

    public class PageParameters  
    {
        public int Draw { get; set; }
        public List<Column> Columns { get; set; }
        public List<Order> Order { get; set; }
        public int Start { get; set; }
        public int Length { get; set; }
        public Search Search { get; set; }
        public string SearchText => Search.Value ?? "";
    }

    public class Order
    {
        [JsonPropertyName("column")]
        public int Column { get; set; }

        [JsonPropertyName("dir")]
        public string Dir { get; set; }
    }

    public class Column
    {
        [JsonPropertyName("data")]
        public string Data { get; set; }

        [JsonPropertyName("name")]
        public string Name { get; set; }

        [JsonPropertyName("searchable")]
        public bool Searchable { get; set; }

        [JsonPropertyName("orderable")]
        public bool Orderable { get; set; }

        [JsonPropertyName("search")]
        public Search Search { get; set; }
    }

    public class DataTableResponse<T>
    {
        public List<T> Data { get; set; }
        public int draw { get; set; }
        public int recordsFiltered { get; set; }
        public int recordsTotal { get; set; }
    }

```
{% endcode %}

{% code fullWidth="true" %}
```
// .Net Core Page Controlar

	[HttpPost]
	[Route("api/natarecord/pagelist/{pageParameters?}")]
	public IActionResult GetPagelist([FromBody] PageParameters pageParameters)
 //   public  Task<ActionResult<IEnumerable<RecordViewModel>>> GetPagelist([FromBody] PageParameters pageParameters)
	{
		var records =  _repo.GetRecords(pageParameters);
		_logger.LogDebug($"Returned {records.TotalCount} records from database.");

		var result = _mapper.Map<List<RecordViewModel>>(records);

		DataTableResponse<RecordViewModel> dataResponse = new DataTableResponse<RecordViewModel>();
		dataResponse.Data = result;
		dataResponse.recordsTotal = records.TotalCount;
		dataResponse.recordsFiltered = records.TotalCount;

		return Ok(dataResponse);
	}
```
{% endcode %}

{% code fullWidth="true" %}
```
// .Net Core Page Repository

	//pagelist
	public PagedList<NATARecord> GetRecords(PageParameters pageParameters)
	{
		return PagedList<NATARecord>.ToPagedList(FindAll()
			.Where(x => x.CustomerName.Contains(pageParameters.SearchText) || x.JobNo.Contains(pageParameters.SearchText) || x.TechName.Contains(pageParameters.SearchText))
			.OrderByDescending(x => x.Id),
			pageParameters.Draw,
			pageParameters.Length);
	}
	
			//pagelist
	private IQueryable<Weighing> FindAll()
	{
		return this._context.Set<Weighing>()
			.AsNoTracking();
	}

```
{% endcode %}
