---
description: >-
  Azure SQL server saved time is UTC time, so web portal view and report need to
  convert
---

# Offset hour

<pre data-full-width="true"><code><strong>// UI
</strong><strong>  
</strong>data: 'DateTime', render: function (data: any, type: any, row: any) {
  return '&#x3C;span style="color:green">' +
    formatDate(new Date(data), 'dd/MM/yyyy hh:mm:ss a', 'en-AU')
},

{
  data: 'value', render: function (data: any, type: any, row: any) {
    if (Number(data) != 0)
      return '&#x3C;span style="color:purple; display: inline-block; float: right;">' + formatNumber(Number(data), 'en-AU', '1.2-2') + '&#x3C;/span>'
    else
      return ''
}
  
</code></pre>



{% code fullWidth="true" %}
```
// API

method1: 
   var curtureInfo = CultureInfo.GetCultureInfo("en-AU");
    .ForMember(desc => desc.ErrorDate, opt => opt.MapFrom(src => src.ErrorTimestamp.HasValue ? src.ErrorTimestamp.Value.ToString("dd/MM/yyyy", curtureInfo) : string.Empty))



method2:

TimeZoneService:

    public int GetAppSettingTimeZoneOffsetHours()
    {
        string timeZoneId = GetAppSettingTimeZone(sectionName);
        int offsetHours = GetTimeZoneOffsetHours(timeZoneId);
        return offsetHours;
    }

AutoMapping:
int offsetHours = _timeZoneService.GetAppSettingTimeZoneOffsetHours();

.ForMember(dest => dest.WeighDate, opt => opt.MapFrom(src => String.Format("{0:dd/MM/yyyy}", src.WeighDateTime.HasValue ? src.WeighDateTime.Value.AddHours(offsetHours).ToLocalTime() : "none")))

```
{% endcode %}
