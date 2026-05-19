# \*ngIf else &&  change to @if

{% code fullWidth="true" %}
```
// Some code
ts:
  if ... 
    isABC = true;
  else
    isABC = false;


HTML:
      <span *ngIf="isABC; else notABC">
        <input class="form-control" [attr.disabled]="true"  formControlName="name" />
      </span>
      
      <ng-template #notABC>
        <input class="form-control" formControlName="name" />
      </ng-template>

```
{% endcode %}

{% code fullWidth="true" %}
```
// *ngIF else

  <td>
    <span *ngIf="dataItem.state=='Failed'; else sussess" style="color: red"> {{dataItem.state}} </span>
    <ng-template #sucssess> {{dataItem.state}} </ng-template>
  </td>

```
{% endcode %}



{% code fullWidth="true" %}
```
// *ngIf, *ngFor change to @if, @for

    <div class="form-group mb-3" *ngIf="data.action==='Edit' && data.emailSetting.emailName.includes('weekly')">
      <label class="mb-2">Day of week</label>
      <select class="form-select form-control" formControlName="dayOfWeek" id="inputGroupSelect01">
        <option *ngFor="let day of dayOfWeekList" [ngValue]="day.id" >{{day.name}}</option>
      </select>
    </div>


    <div class="form-group mb-3">
      @if(data.action==='Edit' && data.emailSetting.emailName.includes('weekly'))
      {
      <label class="mb-2">Day of week</label>
      <select class="form-select form-control" formControlName="dayOfWeek" id="inputGroupSelect01">
        @for(day of dayOfWeekList; track day.id) {
        <option [ngValue]="day.id">{{day.name}}</option>
        }
      </select>
      }
    </div>
```
{% endcode %}
