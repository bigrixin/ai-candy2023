# Autofac in .NET Core



{% code fullWidth="true" %}
```

add Package:  Autofac.Extensions.DependencyInjection

// Startup.cs
      public ILifetimeScope AutofacContainer { get; private set; }
      
      public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
      {
       .....
          //Autofac container
          AutofacContainer = app.ApplicationServices.GetAutofacRoot();
      }


// Main(string[] args)

      // Auofac service
      var host = Host.CreateDefaultBuilder(args)
                .UseServiceProviderFactory(new AutofacServiceProviderFactory())
                .ConfigureWebHostDefaults(webHostBuilder => {
                    webHostBuilder
                          .UseContentRoot(Directory.GetCurrentDirectory())
                          .UseIISIntegration()
                          .UseStartup<Startup>();
                })
                .Build();
      host.Run();



//AutofacModule

    public class AutofacModule : Autofac.Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            var assembly = Assembly.Load("WWWebAuth");
            builder.RegisterAssemblyTypes(assembly)
                .Where(x => x.Name.EndsWith("Repository") && !x.Name.StartsWith('I') || x.Name.Equals("UnitOfWork"))
                .AsImplementedInterfaces().PropertiesAutowired();
        }
    }
    
    
//


public interface ISettingRepository: IBaseRepository<Setting>
{

}



public class SettingRepository : BaseRepository<Setting>, ISettingRepository
{

    public SettingRepository(AppDbContext context) : base(context)
    {

    }

}    
```
{% endcode %}
