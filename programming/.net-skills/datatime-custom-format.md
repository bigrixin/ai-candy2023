# Datatime custom format

```
public class CustomDateTimeConverter : IsoDateTimeConverter
{
    public CustomDateTimeConverter()
    {
        base.DateTimeFormat = "yyyy'-'MM'-'dd'T'HH':'mm':'ssZ";
    }
}
```

using Newtonsoft.Json;

```
    [JsonConverter(typeof(CustomDateTimeConverter))]
    public DateTime? TimestampTare { get; set; }
```
