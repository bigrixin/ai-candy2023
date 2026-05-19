---
description: .NET API + Auth + Angular
---

# Three-layer structure

\WWWebApp -- .NET Core 5.0 Web API

#### &#x20;Back-end / .NET Core API

1. Add a model using Scaffold-Dbcontext (database first ET)

Open Package Manager Console:

Scaffold-DbContext "Server=.\SQLEXPRESS2019; User ID=sa; Password=mrpresident12; database=SquibWeb;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables Flexpoint.Customer, Flexpoint.Driver, Flexpoint.Product, Flexpoint.Product\_Warehouse, Flexpoint.Warehouse, Flexpoint.RFID, Flexpoint.Contract, Flexpoint.VehicleType, Flexpoint.Vehicle, Flexpoint.Email, Flexpoint.PermitType, Flexpoint.Settings

Models\Customer -- Company Models\Driver Models\Product Models\Product\_Warehouse -- Poduct\_Warehouse Models\Warehouse\
Models\RFID Models\Contract Models\VehicleType Models\Vehicle

Models\Email Models\PermitType Models\Settings

Create model from table: Scaffold-DbContext "Server=.\SQLEXPRESS2019; User ID=sa; Password=mrpresident12; database=SquibWeb;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables Flexpoint.BARLEY\_PRODUCT\_INFO, Flexpoint.CANOLA\_PRODUCT\_INFO, Flexpoint.WHEAT\_PRODUCT\_INFO

class SquibWebContext: connect move to class WWWebContext.cs delete class SquibWebContext.cs

1.  **Add Base Repository**

    Web Server->Controller->Repository->Unit of Work(DbContext)->EF database

    Add: 1. Repository: BaseRepository and UnitOfWork 2. IRepository: IBaseRepository and IUnitOfWork

    Add SampleRepository first, then use refoctory (Ctrl .) to generate ISampleRepository

    {% code fullWidth="true" %}
    ```
    public class SampleRepository : BaseRepository<Weighing>, ISampleRepository
    ```
    {% endcode %}

    Add AutoMapProfile

    {% code fullWidth="true" %}
    ```
     public class AutofacModule: Autofac.Module
     {
     	protected override void Load(ContainerBuilder builder)
     	{
     		var assembly = Assembly.Load("WWWebApp");
     		builder.RegisterAssemblyTypes(assembly)
     			.Where(x => x.Name.EndsWith("Repository") && !x.Name.StartsWith('I') || x.Name.Equals("UnitOfWork"))
     			.AsImplementedInterfaces().PropertiesAutowired();
     	}
     }	
    ```
    {% endcode %}

    Startup.cs public IServiceProvider ConfigureServices(IServiceCollection services) {\
    builder.RegisterModule(); builder.Populate(services); AutofacContainer = builder.Build(); }
2. **Add Controller**

BarleyProductInfo, CanolaProductInfo, WheatProductInfo

New Scaffolded / API / API Controller with read/write actions Add IRepositories

{% code fullWidth="true" %}
```
    private readonly IUnitOfWork _unitOfWork;
    private readonly IDisplayWeightRepository _repository;
    private readonly IMapper _mapper;

    public DisplayWeightController(IUnitOfWork unitOfWork, IDisplayWeightRepository repository, IMapper mapper)
    {
        this._unitOfWork = unitOfWork;
        this._repository = repository;
        this._mapper = mapper;
    }
	
```
{% endcode %}

3\. **Add ViewModel**

{% code fullWidth="true" %}
```
// Create sampleViewModel


 public class AutoMapProfile : Profile
 {
    //Model to ViewModel
    public AutoMapProfile()
    {
        CreateMap<sampleViewModel, sampleModel>();
        CreateMap<sampleModel, sampleViewModel>();
    }
 }

Startup.cs
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
	    var mappingConfiguration = new MapperConfiguration(config => config.AddProfile(new AutoMapProfile()));
        IMapper mapper = mappingConfiguration.CreateMapper();
        services.AddSingleton(mapper);	
    }
```
{% endcode %}

4\. **add repository (interface and service)**

a. Create DriverRepository.cs in \Repositories\Repository b. Ctrl . Generate a IDriverRepository public interface IDriverRepository : IBaseRepository { } c. move to \IRepository

*   Add mapping in \MapProfileFormapper\AutoMapProfile.cs

    CreateMap\<DriverViewModel, Driver>().ReverseMap();

#### &#x20;Front-end / Angular

1.  Add (Generate) Service and Model class

    \---- goto .\src\app ----

    {% code fullWidth="true" %}
    ```
     ng g s services/[name]      --skip-tests         // add service
     
     
     ng g class models/[name]    --skip-tests         // add a model class
     
     example:
       ng g service auth-interceptor --skiptests     ////  AuthInterceptorService will be created
    ```
    {% endcode %}
2.  Add component and module

    \---- goto .\src\app\components ----

    {% code fullWidth="true" %}
    ```
     ng g module [name] --routing        // add a module with rounting.modules.ts
     
     ng g c [name]  	                    // add a component  (app.module.ts  check component has added)

     ng g c driver/driver-delete   --skip-tests     // skip generate the test file
     ng g c driver/driver-add-update
     ng g c driver/driver-info   (or under the folder driver: ng g c /driver-info)
    ```
    {% endcode %}
3.  Add the Component name into \[name].module.ts (app.module.ts, it need to delete component)

    import { MatDialogModule } from '@angular/material/dialog'; //if use it

    declarations: \[WeighingComponent]

    {% code fullWidth="true" %}
    ```
        imports: [MatDialogModule,]
    ```
    {% endcode %}
4.  Add route in the \[name]-routing.module.ts

    const routes: Routes = \[ { path: '', component: WeighingComponent, data: { title: $localize`Weighing` } } ];
5.  Add content in model class

    export class Weighing { Location!: number; Carrier!: string; }
6.  Add \[name]Module in the \app\app-routing.module.ts

    {% code fullWidth="true" %}
    ```
     path: 'customer',
     loadChildren: () =>
       import('./components/customer/customer.module').then((m) => m.CustomerModule)

     *** components has add in app.module.ts, it need to delete ****
     *** only need add [component]Component in one place
    ```
    {% endcode %}
7.  Add url and method in the \[name].service.ts

    url = 'http://localhost:5000/api/'; // url = 'https://localhost:5001/api/';

    constructor(private httpClient: HttpClient) { }

    getAllWeighings(): Observable\<any\[]> { var data = this.httpClient.get\<Weighing\[]>(this.url + 'displayweight'); // .pipe(map((res: Weighing\[]) => res)); return data; }
8.  Add method in the \[name].components.ts

    constructor(private weighingService: WeighingService) {

    }

    ngOnInit(): void { this.weighingService.getAllWeighings().subscribe((res:any) => { this.WeighingList = res; }, error => { console.log("retrive weighing list error"); }); }
9.  Add routing in app-routing.module.ts

    { path: 'weighing', loadChildren: () => import('./components/weighing/weighing.module').then((m) => m.WeighingModule) },
10. add Modul in \[name].module.ts if using MatDialog

    > > > Can't bind to 'formGroup' since it isn't a known property of 'form'

    import { MatDialogModule } from '@angular/material/dialog';

    ```
    imports: [ MatDialogModule, ReactiveFormsModule, ]
    ```
11. \[name].service.ts

    import { environment } from 'src/environments/environment'; import { Inject, Injectable } from '@angular/core';

    rootURL = environment.backEndAPIEndpoint;
12. add route in \containers\_nav.ts

    children: \[ { name: 'Customer', url: '/customer', },

***

```
  app-routing.module.ts    ---> point to 	 .\components\[name]\[name].module
   
  components\components.module.ts   (or app.modules.ts -- {opertion})   ??????
 
 .\models\
          -> [name].ts
		  
 .\services\
          -> [name].service.ts                   --> server url and get data from server using model

 .\components\[name]\
				  -> [name].module.ts            --> point to [name].component.ts
				  -> [name].component.ts         --> point to [name]Service
				  -> [name].component.html
				  -> [name]-routing.module.ts
 
 .\containers\_nav.ts     Change link
```

***

publish: https://www.c-sharpcorner.com/article/easily-deploy-angular-app-to-azure-from-visual-studio-code/

```
1. ng build

2. VS Code, left Icon, Azure Workloads->Pay-As-You-Go->NWIApps->select Squ
3. Azure -> APP SERVICE -> Squib (mouse right key) ->  Deploy to Web

*** The files in \dist and \dist\assets will be upload to Azure
```

***

npm install ngx-toastr --save npm install @angular/animations --save
