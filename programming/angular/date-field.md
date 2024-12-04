# Date field



{% code fullWidth="true" %}
```
// Some code
UI display:       
 <td class="text-info">{{dataItem.toDate | date: 'dd/MM/yyyy'}}</td>


Update:

   toDate: [data.season.toDate,Validators.required],

      <div class="form-group mb-3">
        <label>To Date</label> <span style="color: red;"> *</span>
        <input type="date" class="form-control" formControlName="toDate" >
      </div>


ViewModel:
        public string ToDate { get; set; }


Mapping:
   .ForMember(dest => dest.ToDate, opt => opt.MapFrom(src => String.Format("{0:yyyy-MM-dd}", src.ToDate)));

```
{% endcode %}
