# Update from v15 to v18

{% code fullWidth="true" %}
```
----------------------------------- 
  Clean cache
----------------------------------- 
ng cache clean
npm cache verify

----------------------------------- 
  Check current outdated packages
----------------------------------- 
npm outdated

==================================
  Update global annular-cli:
==================================
ng version
npm i @angular-devkit/build-angular@18.1.2 --force
npm i @angular/cli@18


```
{% endcode %}

<pre data-full-width="true"><code><strong>// not sure--------------------
</strong><strong>
</strong>Update angular-cli global:

npm uninstall -g @angular/cli
npm cache clean --force

ng version
Using following commands to re-install :

npm install -g @angular/cli


----------------------------------- 
  Update packages
----------------------------------- 
ng update @angular/core@16 @angular/cli@16 
ng update @angular/core@17 @angular/cli@17  
ng update @angular/core@18 @angular/cli@18 

ng update datatables.net-dt@2.1.0 tslint@5.20.1 --allow-dirty --force

ng update @angular/material@16 --allow-dirty --force
ng update @angular/material@17 --allow-dirty --force
ng update @angular/material@18 --allow-dirty --force
----------------------------------- 
 
// Update others
ng update @types/jasmine@5.1.4 @types/node@22.0.0 jasmine-core@5.2.0 --allow-dirty --force

ng update karma@6.4.4 karma-coverage@2.2.1  karma-jasmine@5.1.0  karma-jasmine-html-reporter@2.1.0 --allow-dirty --force

ng update ngx-toastr@19.0.0 rxjs@7.8.1 typescript@5.5.4 zone.js@0.14.8  --allow-dirty --force

ng update powerbi-client-angular@3.0.5 --allow-dirty --force



ng update @coreui/angular@5.2.11 @coreui/angular-chartjs@5.2.11 @coreui/chartjs@4.0.0 @coreui/coreui@5.1.0 @coreui/icons@3.0.1 @coreui/icons-angular@5.2.11 @coreui/utils@2.0.2 --allow-dirty --force


ng update karma-jasmine-html-reporter --allow-dirty --force


</code></pre>

{% code fullWidth="true" %}
```
----------after update--------------------------------------------------------

1. change @ to &#64;

2. change *ngIf, *ngFor,  to @if, @for 

3. remove 'ngx-perfect-scrollbar'

4. change HttpClientModule to provideHttpClient 

5. change scss\styles.scss

6. others
update default-layout.component.html
import { Component } from '@angular/core';

update app.module.ts

import { provideHttpClient, withInterceptorsFromDi} from '@angular/common/http';

    provideHttpClient(withInterceptorsFromDi()),


add  "polyfills": ["src/polyfills.ts", "@angular/localize/init"] in angular.json
```
{% endcode %}
