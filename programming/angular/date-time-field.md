# Date, Time field



{% code fullWidth="true" %}
```
// Date

UI display:       
 <td class="text-info">{{dataItem.startDate | date: 'dd/MM/yyyy'}}</td>


Update:

     startDate: [data.toDate, Validators.required],

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



{% code fullWidth="true" %}
```
// time

UI display:     
     <td class="text-warning">{{dataItem.startTime | date: 'HH:mm'}}</td>


Update:
  startTime: [this.getFormattedTime(data.sampleCheckSetting.startTime), Validators.required],

  getFormattedTime(dateString: string): string | null {
    return this.datePipe.transform(dateString, 'HH:mm'); // 输出 "17:38"
  }

  <div class="form-group mb-2">
     <label>Start Time</label> <span style="color: red;"> *</span>
     <input type="time" class="form-control" formControlName="startTime" >
  </div>



ViewModel:
        public string StartTime { get; set; }


Mapping:
    .ForMember(dest => dest.StartTime, opt => opt.MapFrom(src => String.Format("{0:yyyy-MM-dd HH:mm:ss}", src.StartTime)))


```
{% endcode %}
