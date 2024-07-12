---
description: >-
  Generate the current local time from the server, using local time zone name in
  appsetting.json
---

# 🕕 C# Time Zone

### **TimeZoneInfo cities**

{% code fullWidth="true" %}
```
        var timeZoneCities = TimeZoneInfo.GetSystemTimeZones();
        string combinedString = string.Join(Environment.NewLine, timeZoneCities);
```
{% endcode %}

### **TimeZoneInfo ids**

{% code fullWidth="true" %}
```
        string timeZoneIdsCombinedString = "";

        foreach (TimeZoneInfo z in TimeZoneInfo.GetSystemTimeZones())
        {
            Console.WriteLine(z.Id);
            timeZoneIdsCombinedString = timeZoneIds + Environment.NewLine+ z.Id;
        }
```
{% endcode %}



{% code fullWidth="true" %}
```
// Australia Time Zone List

private List<Tuple<string, string, string>> AustraliaTimeZoneList()
{
    // JS name, JS City, .NET id
    var list = new List<Tuple<string, string, string>>{
        new Tuple<string,string,string>("Australian Western Standard Time", "Perth", "W. Australia Standard Time"),                     // +8
        new Tuple<string,string,string>("Australian Central Western Standard Time", "Eucla", "Aus Central W. Standard Time"),           // +8:45
        new Tuple<string,string,string>("Australian Central Standard Time", "Darwin", "AUS Central Standard Time"),                     // +9:30/+10:30
        new Tuple<string,string,string>("Australian Eastern Standard Time", "Brisbane", "E. Australia Standard Time"),                  // +10
        new Tuple<string,string,string>("Australian Central Daylight Time", "Adelaide", "Cen. Australia Standard Time"),                // +10:30
        new Tuple<string,string,string>("Australian Eastern Daylight Time", "Canberra/Melbourne/Sydney", "AUS Eastern Standard Time"),  // +11/+10
        new Tuple<string,string,string>("Lord Howe Daylight Time", "Lord Howe Island", "Lord Howe Standard Time")};                     // +11/+10

    return list;
}
```
{% endcode %}

{% code fullWidth="true" %}
```
// Display time zone name

  Windows Id:   AUS Eastern Standard Time
     .NET Id:   AUS Eastern Standard Time
Display name:   (UTC+10:00) Canberra, Melbourne, Sydney


console.log(localTimeZoneName);                      //  Australian Eastern Daylight Time
console.log(Intl.DateTimeFormat().resolvedOptions().timeZone);   //  Australia/Sydney
console.log(new Date().toTimeString().slice(9));     //  GMT+1100 (Australian Eastern Daylight Time)
console.log(Intl.DateTimeFormat().resolvedOptions().timeZone);   // Australia/Sydney
console.log(new Date().getTimezoneOffset() / -60);   //  11
console.log(new Date().toTimeString());              //  12:14:31 GMT+1100 (Australian Eastern Daylight Time)
```
{% endcode %}

{% code fullWidth="true" %}
```
// Generate the current local time from the server

appsetting.json
{
	"ScheduledReportTime": {
		"TimeZoneId": "AUS Eastern Standard Time",
		"TimeZoneLocation": "Canberra/Sydney/Melbourne",
	},
}

public interface ITimeZoneService
{
	DateTime GetCurrentTimeFromSettingTimeZone();
}

public class TimeZoneService : ITimeZoneService
{
    private string sectionName = "ScheduledReportTime";
		
	public DateTime GetCurrentTimeFromSettingTimeZone()
	{
		string timeZoneId = GetAppSettingTimeZone(sectionName);
		TimeZoneInfo tzi = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);

		DateTime settingTimeZoneTime = DateTime.Now;
		if (tzi != null)
		{
			DateTime timeUtc = DateTime.UtcNow;

			//convert UTC to destination timezone time
			settingTimeZoneTime = TimeZoneInfo.ConvertTimeFromUtc(timeUtc, tzi);
		}
		_logger.LogInformation($"setting timezone time - {settingTimeZoneTime}");
		return settingTimeZoneTime;
	}
	
	
	public string GetAppSettingTimeZone(string sectionName)
	{
		var jsonStr = GetAppSettingJsonStr(sectionName);
		return (string)jsonStr["TimeZoneId"];
	}


	public JToken GetAppSettingJsonStr(string sectionName)
	{
		// get value via configuration
		var section = this.Configuration.GetSection(sectionName);
		var newDic = new Dictionary<string, string>();

		foreach (var item in section.GetChildren())
		{
			newDic.Add(item.Key, item.Value);
		}

		var jsonStr = JObject.Parse(JsonConvert.SerializeObject(newDic));
		return jsonStr;
	}
}


```
{% endcode %}

{% code fullWidth="true" %}
```
// Get local time from setting timezone
DateTime currentLocalTimeFromServer = _timeZoneService.GetCurrentTimeFromSettingTimeZone();
```
{% endcode %}
