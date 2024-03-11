# Select checkbox disable

{% code fullWidth="true" %}
````html

 disabled="true"    <<<<<<<<
​ (click)="$event.preventDefault()"    <<<<<<<<
​
​
​
```html
        <div class="form-group mb-2 tab-content" >
          <section class="select-checkbox">
            <div class="select-all-checkbox">
              <mat-checkbox [checked]="allComplete" [color]="LocationTypesCheckBox.color"
                [indeterminate]="someComplete()" (change)="setAll($event.checked)"  disabled="true" >
                <span class="select-all-content"> {{LocationTypesCheckBox.name}}</span>
              </mat-checkbox>
            </div>
​
            <div *ngFor="let item of LocationTypesCheckBox.types let i=index" >
              <input class="select-item-checkbox" type="checkbox" formArrayName="selectedTypes" [value]="item.id"
                [checked]="item.selected"  (change)="onCheckboxChange(item.id, $event)"   (click)="$event.preventDefault()" />
              <span class="select-item-content"> {{item.id}} - {{item.name | titlecase}} </span>
            </div>
          </section>
        </div>
```
Markup



```html
        <div class="form-group mb-2 tab-content" >
          <section class="select-checkbox">
            <div class="select-all-checkbox">
              <mat-checkbox [checked]="allComplete" [color]="LocationTypesCheckBox.color"
                [indeterminate]="someComplete()" (change)="setAll($event.checked)"  disabled="true" >
                <span class="select-all-content"> {{LocationTypesCheckBox.name}}</span>
              </mat-checkbox>
            </div>

            <div *ngFor="let item of LocationTypesCheckBox.types let i=index" >
              <input class="select-item-checkbox" type="checkbox" formArrayName="selectedTypes" [value]="item.id"
                [checked]="item.selected"  (change)="onCheckboxChange(item.id, $event)"   (click)="$event.preventDefault()" />
              <span class="select-item-content"> {{item.id}} - {{item.name | titlecase}} </span>
            </div>
          </section>
        </div>
```
````
{% endcode %}
