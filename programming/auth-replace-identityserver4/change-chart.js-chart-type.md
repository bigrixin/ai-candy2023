---
description: CoreUI chart.js
---

# Change chart.js chart type

```
//Change chart type by clicking the radio button

-- mychart.html
<form>
  <mat-radio-group aria-label="Select an option" (change)="radioChange($event)">
    <mat-radio-button value="bar" checked="{{ barChecked }}">Bar Chart</mat-radio-button>
    <mat-radio-button value="line" checked="{{ lineChecked }}">Line Chart</mat-radio-button>
  </mat-radio-group>
</form>

<c-card class="mb-1" style="width: 100%; text-align: center">
  <c-card-body>
    <c-row>
      <c-col sm="12">
        <h4 class="card-title mb-0" id="traffic">NET (t) - last 20 records</h4>
        <div class="small text-medium-emphasis"></div>
      </c-col>
    </c-row>
    <c-chart
      [data]="chart.data"
      [height]="760"
      [ngStyle]="{ 'marginTop.px': 20 }"
      [type]="chart.type"
    >
      Main chart
    </c-chart>
  </c-card-body>
</c-card>
```

```
// mychart.module.ts

 
  import { FormsModule, ReactiveFormsModule } from '@angular/forms';
  import { MatRadioModule } from '@angular/material/radio';


```

```
// charts-data.ts

        backgroundColor: [
          'rgba(255, 99, 132, 0.2)',
          'rgba(54, 162, 235, 0.2)',
          'rgba(255, 206, 86, 0.2)',
          'rgba(75, 192, 192, 0.2)',
          'rgba(153, 102, 255, 0.2)',
          'rgba(255, 159, 64, 0.2)'
      ],
      borderColor: [
          'rgba(255, 99, 132, 1)',
          'rgba(54, 162, 235, 1)',
          'rgba(255, 206, 86, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(153, 102, 255, 1)',
          'rgba(255, 159, 64, 1)'
      ],

```

```
//  mychart.ts

 radioOptions: string = 'bar';
 barChecked!: boolean;
 lineChecked!: boolean;


  ngOnInit(): void {
    this.initCharts();
  }


  radioChange($event: MatRadioChange): void {
    this.chart = this.chartsData.mainChart;
    let chart_type = $event.value;
    localStorage.setItem('ChartType', chart_type);
    this.radioOptions = chart_type;
    this.reload(this.router.url);
  }


  initCharts(): void {
    this.chart = this.chartsData.mainChart;
    let chartType = localStorage.getItem('ChartType');

    if (chartType != null) {
      if (chartType == 'bar') {
        this.barChecked = true;
        this.lineChecked = false;
      }
      else {
        this.lineChecked = true;
        this.barChecked = false;
      }
      this.chart.type = chartType;
    }
    else {
      this.chart.type = 'bar';
      this.barChecked = true;
      this.lineChecked = false;
    }
    this.chart.options = [{ responsive: true }];
  }


  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('weighing', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }
```

