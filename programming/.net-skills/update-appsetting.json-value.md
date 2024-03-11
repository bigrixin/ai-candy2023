# Update appsetting.json value



{% code fullWidth="true" %}
```
// update appsetting.json value by key

appsettings.json

{
  "AllowedHosts": "*",
  "applicationId": "************",
  "tenant": "************",
  "applicationSecret": "MkM8************",   /// one level
  
  "ScheduledReportTime":                     /// two level
  {                    
    "TimeZoneId": "AUS Eastern Standard Time",
  }
}



Controller:

        [HttpPut]
        [Route("api/appsetting/edit/secret/{oldsecret}/{newsecret}")]
        public IActionResult UpdateAppSettingSecret(string oldsecret, string newsecret)
        {
            string key = "applicationSecret";
            string currentsecret = _appSettingService.GetAppSettingValueByKey(key);
            if (!string.IsNullOrEmpty(currentsecret) && currentsecret == oldsecret)
            {
                try
                {
                    _appSettingService.UpdateAppSettingValue(key, newsecret);
                    return Ok();
                }
                catch (Exception ex)
                {
                    _logger.LogInformation(ex, ex.Message + "\n\n" + ex.StackTrace);

                    return BadRequest();     //400 status code 
                }
            }
            return BadRequest();     //400 status code 
        }



AppSettingService.cs

//one level
        public void UpdateAppSettingValue(string key, string value)
        {
            string sectionPath = "ScheduledReportTime" // two level
            try
            {
                var path = Directory.GetCurrentDirectory();
                var filePath = Path.Combine(path, appsettingName);
                if (global::System.IO.File.Exists(filePath))
                {
                    string json = global::System.IO.File.ReadAllText(filePath);
                    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(json);
                    jsonObj[key] = value;   /// one level
                    
                    // jsonObj[sectionPath][key] = value;     // two level
                    
                    
                    string output = Newtonsoft.Json.JsonConvert.SerializeObject(jsonObj, Newtonsoft.Json.Formatting.Indented);
                    global::System.IO.File.WriteAllText(filePath, output);
                }
            }
            catch (ConfigurationErrorsException)
            {
                _logger.LogInformation($"Error writing app settings");
                Console.WriteLine("Error writing app settings");
            }
        }

```
{% endcode %}
