# \*ngIf else

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

```
// *ngIF else

  <td>
    <span *ngIf="dataItem.state=='Failed'; else sussess" style="color: red"> {{dataItem.state}} </span>
    <ng-template #sucssess> {{dataItem.state}} </ng-template>
  </td>

```

