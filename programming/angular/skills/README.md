# 15 Skills



1. Add a Component and Module&#x20;
2. Add a Service and a Model class&#x20;
3. Add Routing&#x20;
4. Add the Component name into \[name].module.ts&#x20;
5. Add content to model class&#x20;
6. Add \[name]Module in the \app\app-routing.module.ts&#x20;
7. Add url and method in the Service&#x20;
8. Add method in a Component&#x20;
9. Environment setting Change&#x20;
10. Background colour&#x20;
11. Input pattern: password, phone, email&#x20;
12. Two decimal places&#x20;
13. Date Format&#x20;
14. Bind to 'formGroup' using MatDialog
15. Reload page



{% code fullWidth="true" %}
```
// 1. Add a Component and Module
 goto .\src\app\components (Select Components->right mouse key->Open in integrated Interminal)

   1) ng g module newComponentName --routing      // add a module with rounting.modules.ts
        *** if create the module first, do not to do section 4 ***

   2) ng g c newComponentName  --skip-tests        // add a component

   3) ng g c newComponentName\newComponentName-add-update   --skip-tests

   4) ng g c newComponentName\newComponentName-delete   --skip-tests
```
{% endcode %}

{% code fullWidth="true" %}
```
//2. Add a Service and a Model class
goto .\src\app (Select services->right mouse key->Open in integrated Interminal)

    ng g service newServiceName  --skip-tests                 //  ***name***  will be created


goto .\src\app (Select models->right mouse key->Open in integrated Interminal)

    ng g class newClassName   --skip-tests         // add a model class  
```
{% endcode %}

{% code fullWidth="true" %}
```
//3. Add Routing
A:
src\app\components\newComponentName\newComponentName-routing.module.ts
    const routes: Routes = [
      {
          path: '',
          component: NewComponentNameComponent,
          data: {
          title: $localize`newComponentName` //display on head-middle rounting
          }
      }
    ];

    B:
    src\app\app-routing.module.ts
      {
          path: 'NewComponentName',       //same url in _nav.ts
          loadChildren: () =>
            import('./components/NewComponentName/NewComponentName.module').then((m) => m.NewComponentNameModule)
      },

    C:
    src\app\containers\default-layout\_nav.ts
    children: [
        {
          name: '***Name***',       //display on left side menu
          url: '/***name***',        //same path in app-routing.module.ts
        },
   ]
```
{% endcode %}

{% code fullWidth="true" %}
```
//4. Add the Component name into [name].module.ts (optional)


*** Delete created component name in app.module.ts ***

      import { MatDialogModule } from '@angular/material/dialog';   //if use it

      declarations: newComponentName

            imports: [
      MatDialogModule,
      ReactiveFormsModule,
      DataTablesModule,
      MatProgressBarModule]       //if use it
```
{% endcode %}

{% code fullWidth="true" %}
```
//5. Add content to model class (optional)
ng g class wastetype --skip-tests (refer to section 2)

src\app\models\***name***.ts

    export class ***name*** {
        Location!: number;
        Name!: string;      
    }
```
{% endcode %}

{% code fullWidth="true" %}
```
//6. Add [name]Module in the \app\app-routing.module.ts 
        path: '/***name*** ',

        loadChildren: () =>

              import('./components/***name***//***name*** .module').then((m) => m.****Name***Module)

        *** components have been added in app.module.ts, it needs to delete ****

        *** only need add [component]Component in one place
```
{% endcode %}

{% code fullWidth="true" %}
```
//7. Add url and method in the Service
url = 'http://localhost:5000/api/';
// url = 'https://localhost:5001/api/';
constructor(private httpClient: HttpClient) { }

getAll***Name***s(): Observable {
    var data = this.httpClient.get<***Name***[]>(this.url + 'display***Name***');
    // .pipe(map((res: ***Name***[]) => res));
    return data;
}
```
{% endcode %}

{% code fullWidth="true" %}
```
//8. Add method in a Component
constructor(private ***Name***Service: ***Name***Service) { }
ngOnInit(): void {
    this.***Name***Service.getAll***Name***s().subscribe((res:any) => {
       this.***Name***List = res;
    }, error => {
       console.log("retrive ***Name*** list error");
    });
}
```
{% endcode %}

{% code fullWidth="true" %}
```
//9. Environment
src\environments\environment.prod.ts
export const environment = {
    production: true,
    backEndAPIEndpoint: 'https://****-api-dev.azurewebsites.net/',
    identifyEndpoint: 'https://****-auth-dev.azurewebsites.net/',
};

src\environments\environment.ts
export const environment = {
production: false,
    backEndAPIEndpoint: 'https://localhost:5001/',
    identifyEndpoint: 'https://localhost:5002/'
};

src\app\constants.ts
export const constants = {
    powerBI_reportBaseURL: 'https://app.powerbi.com/reportEmbed',
    groupId : '******-****-****-****-******',
    weight_chart_Id : '******-****-****-****-******',
    weight_report_Id : '******-****-****-****-******',
};

Call in services     import { environment } from 'src/environments/environment';
    import { Inject, Injectable } from '@angular/core';

    rootURL = environment.backEndAPIEndpoint;
```
{% endcode %}

{% code fullWidth="true" %}
````
//10. Change background colour

```typescript
  styles:['::ng-deep div { background: white}']
```
````
{% endcode %}

{% code fullWidth="true" %}
```
//11. input pattern

password or phone:
    <input class="form-control" formControlName="pin" pattern="^-?[0-9]\d*(\9)?$" placeholder="0-9 number only">

email
    <input type="email" class="form-control" formControlName="email" pattern="^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" placeholder="name@example.com">
      
```
{% endcode %}

{% code fullWidth="true" %}
```
//12. Two decimal places

   <td >${{dataItem.Amount | number:'1.2-2'}}</td>
```
{% endcode %}

{% code fullWidth="true" %}
```
//13. Date Format

   <td style="color:green;"> {{dataItem.timestamp |date:'MM/dd/yyyy hh:mm:ss'}}</td>
```
{% endcode %}

{% code fullWidth="true" %}
```
//14. Bind to 'formGroup' using MatDialog
 
>>>Can't bind to 'formGroup' since it isn't a known property of 'form'
import { MatDialogModule } from '@angular/material/dialog';
imports: [ MatDialogModule, ReactiveFormsModule, ]
```
{% endcode %}



{% code fullWidth="true" %}
```
// 15. Reload page
 
  constructor(
    public router: Router,
    }
 
  this.reload(this.router.url);
 

  async reload(url: string): Promise<boolean> {
    await this.router.navigateByUrl('home', { skipLocationChange: true });
    return this.router.navigateByUrl(url);
  }

 
```
{% endcode %}
