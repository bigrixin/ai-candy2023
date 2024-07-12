# ⏰ Get data by local time (UI, API)



{% code fullWidth="true" %}
```
--- UI ---

function convertUTCToLocalTime(utcTime:any)
{
  let dateTime =new Date(utcTime);
  //returns the difference in minutes.
  let offsetMinutes = dateTime.getTimezoneOffset();              
  //returns the number of milliseconds  since January 1, 1970 00:00:00.
  let numberOfMilliseconds = dateTime.getTime();                 
  let offsetMilliseconds = - offsetMinutes  * 60 * 1000;
  dateTime.setTime(numberOfMilliseconds + offsetMilliseconds);
  return dateTime;
}


--- API ---> send Email

 1. get local time by appsetting timezone name
 
	DateTime settingTimeZoneCurrentTime = _timeZoneService.GetCurrentTimeFromSettingTimeZone();
	string nowTimeString = settingTimeZoneCurrentTime.ToString("yyyy-MM-dd");   // local time
	
	
 2. set start-end time by local time
 
    // set send Email time, for example: 01:00:00
	DateTime today0am = DateTime.Parse(nowTimeString);    
	DateTime today1am = today0am.AddHours(1); 
	// start from last day
	DateTime yesterday1am = today1am.AddDays(-1);        
	
	
 3. local time to UTC time
 
	int offsetHours = _timeZoneService.GetAppSettingTimeZoneOffsetHours();
	DateTime UTCBeginTime = yesterday1am.AddHours(-offsetHours);
	DateTime UTCEndTime = today1am.AddHours(-offsetHours);
	
	
 4. get data from Azure SQL	by UTC time
 
    _repository.GetAllDataByDateAsync(startTime, endTime);
   
   
 5. mapping to local time
 
  .ForMember(dest => dest.Date, opt => opt.MapFrom(src => String.Format("{0:dd/MM/yyyy}", src.DateTime.HasValue ? src.DateTime.Value.AddHours(offsetHours).ToLocalTime() : string.Empty)))
  .ForMember(dest => dest.Time, opt => opt.MapFrom(src => String.Format("{0:HH:mm:ss}", src.DateTime.HasValue ? src.DateTime.Value.AddHours(offsetHours).ToLocalTime() : string.Empty)));
  
  
--- API --> export csv report by local time

GetDataByLocalTime(DateTime beginDate, DateTime endDate)
{

1. local time to UTC time
 
	int offsetHours = _timeZoneService.GetAppSettingTimeZoneOffsetHours();
	DateTime UTCBeginTime = beginDate.AddHours(-offsetHours);
	DateTime UTCEndTime = endDate.AddHours(-offsetHours);

2. get data by UTC time

	var data = _repository.GetAllWeighingsByLocationIdAndDateAsync(beginDate, endDate));

3. AutoMapping UTC time to local time
 
  .ForMember(dest => dest.Date, opt => opt.MapFrom(src => String.Format("{0:dd/MM/yyyy}", 
       src.DateTime.HasValue ? src.DateTime.Value.AddHours(offsetHours).ToLocalTime() : string.Empty)))
  .ForMember(dest => dest.Time, opt => opt.MapFrom(src => String.Format("{0:HH:mm:ss}", 
       src.DateTime.HasValue ? src.DateTime.Value.AddHours(offsetHours).ToLocalTime() : string.Empty)));
}


```
{% endcode %}
