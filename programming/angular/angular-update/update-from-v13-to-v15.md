# Update from v13 to v15

{% code fullWidth="true" %}
```
// Check the Package version

    npm outdated
```
{% endcode %}

{% code fullWidth="true" %}
```
// Update Angular from 13 to 15.2.3

	1. ng update @angular/core@14 @angular/cli@14 --allow-dirty --force
	2. ng update @angular/core@15 @angular/cli@15 --allow-dirty --force
```
{% endcode %}

{% code fullWidth="true" %}
```
// Update CoreUI from 4.0.5 to 4.3.6
   ng update @coreui/coreui@4.2.6 @coreui/icons-angular@4.3.16 @coreui/icons@3.0.1 @coreui/angular-chartjs@4.3.16 --allow-dirty --force
   
***  After the update, it needs to change below content  ***

   1. Delete below lines ~ in ./src/scss/styles.scss
      @import "~@coreui/coreui/scss/coreui";
      @import "~@coreui/chartjs/scss/coreui-chartjs";
   
   2. add below code in scss/styles.scss
         
      table.table tr th {
        background-color: rgb(217, 217, 217) !important;
        color: #676372;
        border: 1px solid rgb(204, 204, 204);
        border-collapse: collapse;
      }

```
{% endcode %}

{% code fullWidth="true" %}
```
// Update angular/material to the new version 

    ng update @angular/material@14 --allow-dirty --force	   //@13.3.9 to @14.2.7
    ng update @angular/material@15 --allow-dirty --force       //@14.2.7 to @13.2.3

***  After the update, it needs to change below content  ***
    
    1. In dashboard.module.ts, app.module.ts and other module.ts

       Old
       import { MatLegacyProgressBarModule as MatProgressBarModule } from '@angular/material/legacy-progress-bar';
    
       New
       import { MatProgressBarModule } from '@angular/material/progress-bar';
    
    2. Delete all DatePipe in dashboard-charts-data.ts and dashboard.module.ts
    
    3. In dashboard-charts-data.ts, change below content
       old      
          var date = this.datePipe.transform(weight_time, 'yyyy-MM-dd');
       new
          var date = formatDate(weight_time,'yyyy-MM-dd', 'en-US')


```
{% endcode %}

{% code fullWidth="true" %}
```
// Update others
   ng update @types/jasmine@4.3.1 @types/node@18.15.5 jasmine-core@4.5.0 --allow-dirty --force 
   ng update karma-coverage@2.2.0 karma-jasmine-html-reporter@2.0.0 karma-jasmine@5.1.0 karma@6.4.1 --allow-dirty --force
   ng update rxjs@7.8.0 typescript@4.9.5 zone.js@0.12.0 ngx-toastr@16.1.0  --allow-dirty --force
```
{% endcode %}

{% code fullWidth="true" %}
```
// Install angularx-qrcode if need it

 npm install angularx-qrcode@15.0.1
 npm install ngx-dropzone@3.1.0
```
{% endcode %}

{% code fullWidth="true" %}
```
// Not need to update
	==   chart.js@3.9.1                 ==	
	==   karma-chrome-launcher@3.1.1    ==	
	==   ngx-perfect-scrollbar@10.1.1   ==
	==   tslib@2.5.0                    ==	
// typescript@4.9.5      (*** do not update to @5.0 ***)
```
{% endcode %}
