---
description: https://docs.coravel.net/Installation/
---

# Coravel Schedule



{% code fullWidth="true" %}
```
// c# Add Coravel Parkage

In startup.cs add below code:

---- ConfigureServices -----
Services.AddScheduler();
Services.AddTransient<SampleInvocable>();

---- Configure ----
var provider = app.ApplicationServices;
provider.UseScheduler(scheduler =>
{
    scheduler.Schedule<SampleInvocable>();
    .EveryMinute()
    .Weekday();
});

```
{% endcode %}



{% code fullWidth="true" %}
```
// method 

public class SampleInvocable : IInvocable
{
    public Task Invoke()
    {
       Console.WriteLine("Every minute on weekday ..");
       return Task.CompletedTask;
     }
}

```
{% endcode %}

**Cron expression generator**: [https://crontab.cronhub.io/](https://crontab.cronhub.io/)

{% code fullWidth="true" %}
```
// cron expression

scheduler.Schedule(
.Cron("0 0 1 * *") // At midnight on the 1st day of each month.

0 1 * * 1 // At 01:00 AM, only on Monday
0 1 1 * * // At 01:00 AM, on day 1 of the month

scheduler.Schedule<SendDailyReportsEmailJob>()
.Cron("39 15 * * *") // run at 1:00 pm daily                 // .DailyAt(15, 29)
.Zoned(TimeZoneInfo.Local);


scheduler.Schedule<SendWeeklyReportsEmailJob>()
.Cron("0 1 * * 1")   // Cron expression - At 01:00 AM, only on Monday
.Zoned(TimeZoneInfo.Local);

scheduler.Schedule<SendMonthlyReportsEmailJob>()
.Cron("0 1 1 * *")   // Cron expression - At 01:00 AM, on day 1 of the month
.Zoned(TimeZoneInfo.Local);
               
```
{% endcode %}

## <mark style="color:green;">reloadOnChange for appsettings.json</mark>

{% code fullWidth="true" %}
```
// Coravel Schudler, Change time - read value from appsettings.json

// startup.cs
public IConfiguration GetAppssetingsConfig()
{
    return new ConfigurationBuilder()
               .SetBasePath(Directory.GetCurrentDirectory())
               .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
               .AddEnvironmentVariables()
               .Build();
}
 
public Startup(IConfiguration configuration)
{
    Configuration = configuration;
    // GetAppssetingsConfig();    ///not be sured
}
        
public IServiceProvider ConfigureServices(IServiceCollection services)
{
   // GetAppssetingsConfig();  ///not be used, not sure

    //--- schedule service
    services.AddScheduler();
    services.AddTransient<SendDailyReportsEmailJob>();
    services.AddTransient<SendWeeklyReportsEmailJob>();
    services.AddTransient<SendMonthlyReportsEmailJob>();
    ...

}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{

    // scheduler
    var provider = app.ApplicationServices;
    provider.UseScheduler(scheduler =>
    {
        // run at 1:00 AM daily     .DailyAt(1, 0)  or .Cron("0 1 * * *") 
        scheduler.Schedule<SendDailyReportsEmailJob>()
        .DailyAt(1, 0)
        .PreventOverlapping("SendDailyReportsEmailJob")
        .Zoned(TimeZoneInfo.Local);

        // Cron expression - At 01:15 AM, only on Monday
        scheduler.Schedule<SendWeeklyReportsEmailJob>()
        .Cron("15 1 * * 1")
        .Zoned(TimeZoneInfo.Local);

        // Cron expression - At 02:00 AM, on day 1 of the month
        scheduler.Schedule<SendMonthlyReportsEmailJob>()
        // .DailyAt(16, 0)
        .Cron("0 2 1 * *")
        .Zoned(TimeZoneInfo.Local);
    });

    // appsetings.json changed event
    ChangeToken.OnChange(() => this.Configuration.GetReloadToken(), () =>
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            IScheduler scheduler = scope.ServiceProvider.GetRequiredService<IScheduler>();
            IConfiguration config = scope.ServiceProvider.GetRequiredService<IConfiguration>();

            Console.WriteLine("A new scheduled task! ...");
            // Remove your past scheduled event/task
            (scheduler as Scheduler).TryUnschedule("SendDailyReportsEmailJob");
            
            //add new task
            scheduler.Schedule<SendDailyReportsEmailJob>()
                   .Cron(config["DailyReportTime:daily-report-time"])
                   .Zoned(TimeZoneInfo.Local)
                   .PreventOverlapping("SendDailyReportsEmailJob");
        };
    });

}


//SendDailyReportsEmailJob.cs
public class SendDailyReportsEmailJob : IInvocable
{
    // run when schedule time is coming
    public async Task Invoke()
    {
         var dateString = DateTime.Now.ToString("yyyy-MM-dd hh:mm:ss");
         Console.WriteLine("A Email has sent!" + dateString);

         await GetDailyReport();      //  

    }

    private async Task GetDailyReport()
    {
        //sent email
    }
}


```
{% endcode %}

<mark style="color:red;background-color:green;">Change</mark>  appsettings.json <mark style="color:red;background-color:green;">value when updating data in the database</mark>

{% code fullWidth="true" %}
```
// controller

[HttpPut]
[Route("api/setting/edit/{id}")]
public async Task<IActionResult> UpdateSetting(int id, [FromForm] SettingViewModel settingVM)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    try
    {
        var oldRecord = await _repository.GetById(id);

        if (oldRecord == null)
            return NotFound($"Couldn't find the EmailSetting id = {id}");

        var newRecord = _mapper.Map(settingVM, oldRecord);

        _repository.UpdateSetting(newRecord);

        if (await _unitOfWork.SaveAsyc() > 0)
        {
            _logger.LogInformation($"Setting id = {id} has updated !");
            string cronTime1 = getDailyCronTimeStr(newRecord.SendTime.Hour, newRecord.SendTime.Minute);      // daily-report-time1
            updateAppSetting<string>("DailyReportTime:daily-report-time1", cronTime1);

            string cronTime2 = getDailyCronTimeStr(newRecord.SendTime2?.Hour, newRecord.SendTime2?.Minute);  // daily-report-time2
            updateAppSetting<string>("DailyReportTime:daily-report-time2", cronTime2);

            return Ok(_mapper.Map<SettingViewModel>(oldRecord));
        }
        return new StatusCodeResult(StatusCodes.Status500InternalServerError);
    }
    catch (Exception ex)
    {
        _logger.LogInformation($"Exception: " + ex.Message);
    }
    return
        BadRequest($"Could not update EmailSetting id= {id}");
}
```
{% endcode %}

{% code fullWidth="true" %}
```
// appsettings.json
{

  "DailyReportTime": {
    "daily-report-time": "50 06 * * *",
    "daily-report-time2": "43 15 * * *"
  },
  ...
}



// EmailSetting Controller

private string getDailyCronTimeStr(int? hour, int? minute)
{
    var h = hour.ToString();
    if (h.Length == 1)
        h = "0" + h;

    var m = minute.ToString();
    if (m.Length == 1)
        m = "0" + m;

    string cronTimeStr = m + " " + h + " * * *";    // Cron("0 1 * * *") >> run at 1:00 AM daily 
    return cronTimeStr;
}

private  void updateAppSetting<T>(string key, T value)
{
   try
    {
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
                var oldvalue = jsonObj[sectionPath][keyPath];

                jsonObj[sectionPath][keyPath] = value;
            }
            else
            {
                jsonObj[sectionPath] = value;  
            }

            string output = Newtonsoft.Json.JsonConvert.SerializeObject(jsonObj, Newtonsoft.Json.Formatting.Indented);
            global::System.IO.File.WriteAllText(filePath, output);
        }
    }
    catch (ConfigurationErrorsException)
    {
        Console.WriteLine("Error writing app settings");
    }            
} 

```
{% endcode %}
