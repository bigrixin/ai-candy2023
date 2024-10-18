---
description: 阻止嵌套 mapping
---

# Ignore Nesting



{% code fullWidth="true" %}
```
// Some code
    CreateMap<Vehicle, VehicleResponse>()
        .ForMember(dest => dest.CTP, opt => opt.MapFrom(src => src.Ctp.ToString()))
        .ForMember(dest => dest.Company, opt => opt.Ignore())  //不map company
        
        
        
         //建立新的，阻止VehicleType下的嵌套
        .ForMember(dest => dest.VehicleType, opt => opt.MapFrom(src=>new VehicleType
        {
            Vehicletypeid = src.Vehicletype.Vehicletypeid,  
            
        }
```
{% endcode %}
