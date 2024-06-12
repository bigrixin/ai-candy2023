---
description: total hours between two datetime
---

# Datetime processing in AutoMapping



{% code fullWidth="true" %}
```
1. Get total hours between two datetime
 .ForMember(desc => desc.TotalDowntime, opt => opt.MapFrom(src => src.BeginTimestamp.Value.Subtract((DateTime)src.ErrorResolvedTimestamp).Hours))


2. Datetime to string

   .ForMember(desc => desc.BeginDate, opt => opt.MapFrom(src => src.BeginTimestamp.Value.ToString("dd/MM/yyyy", CultureInfo.InvariantCulture)))

   .ForMember(desc => desc.ErrorOccurrenceTime, opt => opt.MapFrom(src => src.BeginTimestamp.Value.ToString("hh:mm:ss", CultureInfo.InvariantCulture)));

3. String to Datetime

    .ForMember(desc => desc.ErrorDateTime, opt => opt.MapFrom(src => srcDateTime.ParseExact("24/01/2013", "dd/MM/yyyy", CultureInfo.InvariantCulture)));
```
{% endcode %}
