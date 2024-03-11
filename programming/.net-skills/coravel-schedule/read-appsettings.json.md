# Read appsettings.json



```
// Some code

appsettings.json
{  
 ...
  "ScheduledReportTime": {
      "daily-report-time1": "00 06 * * *",
      "daily-report-time2": "00 18 * * *",
      "weekly-report-time": "05 08 * * 1" //"15 08 * * 1"
  },
}  

string default_weekly_report_time = "10 8 * * 1";
string weekly_report_time = readAppSetting("ScheduledReportTime:weekly-report-time", default_weekly_report_time);

private string readAppSetting(string key, string defaultValue)
{
    try
    {
        string newValue = defaultValue;
        var path = Directory.GetCurrentDirectory();
        var filePath = Path.Combine(path, "appSettings.json");
        if (global::System.IO.File.Exists(filePath))
        {
            string json = global::System.IO.File.ReadAllText(filePath);
            dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(json);

            var sectionPath = key.Split(":")[0];
            if (!string.IsNullOrEmpty(sectionPath))
            {
                // used for has subkey
                var keyPath = key.Split(":")[1];
                newValue = jsonObj[sectionPath][keyPath];
            }
        }
        return newValue;
    }
    catch (ConfigurationErrorsException)
    {
        Console.WriteLine("Error writing app settings");
        return null;
    }
}
```
