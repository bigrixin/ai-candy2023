---
description: >-
  Azure SQL server saved time is UTC time, so web portal view and report need to
  convert
---

# 🕑 Add offset hours for local UI and report

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
// method1: 
var curtureInfo = CultureInfo.GetCultureInfo("en-AU");
  .ForMember(desc => desc.ErrorDate, opt => opt.MapFrom(src => src.ErrorTimestamp.HasValue ? src.ErrorTimestamp.Value.ToString("dd/MM/yyyy", curtureInfo) : string.Empty))

```
{% endcode %}



{% code fullWidth="true" %}
```
// method2:

TimeZoneService:

public int GetAppSettingTimeZoneOffsetHours()
{
    string timeZoneId = GetAppSettingTimeZone(sectionName);
    int offsetHours = GetTimeZoneOffsetHours(timeZoneId);
    return offsetHours;
}
    
public string GetAppSettingTimeZone(string sectionName)
{
    var jsonStr = GetAppSettingJsonStr(sectionName);
    return (string)jsonStr["TimeZoneId"];
}

[Obsolete]
public int GetTimeZoneOffsetHours(string timeZoneId)
{
  // timeZoneId == "AUS Eastern Standard Time";
  TimeZone currentTimeZone = TimeZone.CurrentTimeZone;
  DateTime currentDate = DateTime.Now;

  var currentTime = currentTimeZone.ToLocalTime(currentDate);
  var utcTime = currentTimeZone.ToUniversalTime(currentDate);

  _logger.LogInformation($"current time: " + currentTime.ToString("MM/dd/yyyy HH:mm:ss"));
  _logger.LogInformation($"utc time: " + utcTime.ToString("MM/dd/yyyy HH:mm:ss"));


  DateTime timeUtc = DateTime.UtcNow;

  //get current timezone name   -- Azure timezone: 'Coordinated Universal Time'
  string timeZoneName = CurrentTimeZoneName();

  _logger.LogInformation($"timezone: " + timeZoneName);

  if (timeZoneName != timeZoneId)
  {
      TimeZoneInfo tzi = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);
      if (tzi != null)
      {
          _logger.LogInformation($"find destination timezone!");

          //convert UTC to destination timezone time
          DateTime cstTime = TimeZoneInfo.ConvertTimeFromUtc(timeUtc, tzi);

          _logger.LogInformation($"sydney time: " + cstTime.ToString("MM/dd/yyyy HH:mm:ss"));

          TimeSpan offset = tzi.GetUtcOffset(timeUtc);

          _logger.LogInformation($"offset hours: " + offset.Hours.ToString());
          return offset.Hours;
      }

  }
  return 0;
}

------------------------------------------------------
AutoMapping:
int offsetHours = _timeZoneService.GetAppSettingTimeZoneOffsetHours();

.ForMember(dest => dest.Date, opt => opt.MapFrom(src => String.Format("{0:dd/MM/yyyy HH:mm:ss}", src.Date.HasValue ? src.Date.Value.AddHours(offsetHours).ToLocalTime() : "none")))

or

.ForMember(desc => desc.ErrorDate, opt => opt.MapFrom(src => src.ErrorTime.HasValue ? src.ErrorTime.Value.AddHours(offsetHours).ToString("dd/MM/yyyy") : string.Empty))

```
{% endcode %}
