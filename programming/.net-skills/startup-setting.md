# Startup setting



{% code fullWidth="true" %}
```
//  no dependency injection 

method 1:
  services.AddAutoMapper(typeof(Startup).Assembly);

method 2:
  services.AddAutoMapper(typeof(AutoMapProfile));

method 3:
    var mappingConfiguration = new MapperConfiguration(config => config.AddProfile(new AutoMapProfile()));
    IMapper mapper = mappingConfiguration.CreateMapper();
    services.AddSingleton(mapper);

```
{% endcode %}



{% code fullWidth="true" %}
```
// Have dependency injection in AutoMapping

public class AutoMapProfile : Profile
{
  private readonly ITimeZoneService _timeZoneService;
  
  public AutoMapProfile(ITimeZoneService timeZoneService)
  {
  	_timeZoneService = timeZoneService;
     UserProfileMappers();
  }
  
  
  private void UserProfileMappers()
  {
    int offsetHours = _timeZoneService.GetAppSettingTimeZoneOffsetHours();
  
     	  CreateMap<WeighReportViewModel, Weighing>();
    CreateMap<Weighing, WeighReportViewModel>()
      .ForMember(dest => dest.WeighDate, opt => opt.MapFrom(src => String.Format("{0:dd/MM/yyyy}", src.WeighDateTime.HasValue ? src.WeighDateTime.Value.AddHours(offsetHours).ToLocalTime() : "none")))
      .ForMember(dest => dest.WeighTime, opt => opt.MapFrom(src => String.Format("{0:HH:mm:ss}", src.WeighDateTime.HasValue ? src.WeighDateTime.Value.AddHours(offsetHours).ToLocalTime() : "none")));
  }
}

Startup.cs
 
services.AddScoped<ITimeZoneService, TimeZoneService>();
			
services.AddAutoMapper(AppDomain.CurrentDomain.GetAssemblies());
services.AddSingleton(provider => new MapperConfiguration(cfg =>
{
  cfg.AddProfile(new AutoMapProfile(provider.GetService<ITimeZoneService>()));
}).CreateMapper());
```
{% endcode %}
