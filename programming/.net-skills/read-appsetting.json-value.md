---
description: two methods
---

# Read appsetting.json value

{% code fullWidth="true" %}
```
// method one - read appsetting value by key

public IConfiguration Configuration { get; }

public string ReadAppSetting(string key, string value)
{
    // get value via configuration
    var section = this.Configuration.GetSection(sectionName);
    string result = null;

    foreach (var item in section.GetChildren())
    {
        if (item.Key == key)
        {
            result = item.Value;
            break;
        }
    }

    return result;
 }

```
{% endcode %}



{% code fullWidth="true" %}
```
// method two - read appsetting value by key

public string ReadAppSetting(string key, string value)
{
    try
    {
        string newValue = value;
        var path = Directory.GetCurrentDirectory();
        var filePath = Path.Combine(path, "appSettings.json");
        if (global::System.IO.File.Exists(filePath))
        {
            string json = global::System.IO.File.ReadAllText(filePath);
            dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(json);
            var sectionPath = key.Split(":")[0];

            if (!string.IsNullOrEmpty(sectionPath))
            {
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
{% endcode %}



{% code fullWidth="true" %}
```
//two methods

 string sectionName = "ScheduledTime";
  
public JToken GetAppSettingJsonStr_method1(string sectionName)
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
        
        
        
        
public JToken GetAppSettingJsonStr_method2(string sectionName)
{
    var path = Directory.GetCurrentDirectory();
    var filePath = Path.Combine(path, "appSettings.json");
    if (global::System.IO.File.Exists(filePath))
    {
        string json = global::System.IO.File.ReadAllText(filePath);
        var jsonStr = JObject.Parse(json)[sectionName];
        return jsonStr;
    }
    return null;
             
}
```
{% endcode %}



{% code fullWidth="true" %}
```
// Some code
string preKey = "ScheduledTime";
string default_daily_report_time = "00 18 * * *";
string daily_report_time = readAppSetting(preKey + ":daily-report-time", default_daily_report_time);

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
{% endcode %}
