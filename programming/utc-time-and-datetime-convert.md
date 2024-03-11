# ⏱️ UTC time and Datetime convert



{% code fullWidth="true" %}
```
// UTC Time: https://www.utctime.net/

----------------------------------------	
Date Time Format
----------------------------------------	
     UTC	    2023-07-26T03:19:31Z    (Z -- UTC time)
     ISO-8601       2023-07-26T02:20:09+0000
     ISO-8601	    2023-07-26T03:19:31+00:00
     RFC 1123	    Wed, 26 Jul 2023 03:19:31 GMT
	 
----------------------------------------	
SQL Server's current Timezone
----------------------------------------	
    SELECT SYSDATETIMEOFFSET()
 
     example 1:  2023-07-26 03:05:00.4316665 +00:00
     example 2:  2023-07-26 13:03:57.8343909 +10:00  
	 
	 
---------------------------------------- 
UTC to local time in Angular
----------------------------------------
The way in ts:
   updateWeightTime = '2023-07-26T16:06:52.5268847'
   let localtime =  new Date(updateWeightTime+'Z');
   >>> Thu Jul 27 2023 02:06:52 GMT+1000 (Australian Eastern Standard Time)
 
 
   let localWeightTime = new Date(updateWeightTime).toLocaleString("en-AU", {
          dateStyle: "medium",
          timeStyle: "medium",
          timeZone: "Australia/Sydney"
        });
		
	>>>	27 July 2023, 2:06:52 am
	
	
   let localWeightTime = new Date(localtime).toLocaleString("en-AU", {
          dateStyle: "medium",
          timeStyle: "medium",
          timeZone: "Australia/Adelaide"
        });	
	>>> 27 July 2023, 1:36:52 am


The way in html:		
    <span class="text-muted">{{trans.Date | date:'yyyy-MM-dd HH:mm:ss Z' }}</span>	
    <span>{{CurrentTime+'Z' | date: 'dd/MM/yyyy hh:mm:ss a' }} </span>		
	
----------------------------------------	
C# Current Timezone
----------------------------------------	
     const string dataFmt = "{0,-30}{1}";
     TimeZone localZone = TimeZone.CurrentTimeZone;
     DateTime currentDate = DateTime.Now;

     TimeSpan currentOffset = localZone.GetUtcOffset( currentDate );
   
     Console.WriteLine( dataFmt, "UTC offset:", currentOffset );
	    UTC offset:  -08:00:00
		
	 Console.WriteLine( dataFmt, "Standard time name:", localZone.StandardName );
	    Standard time name:  Pacific Standard Time
	

				
----------------------------------------	 
C# timezone list
----------------------------------------	

using System;

namespace TimeZoneIds
{
  class Program
  {
    static void Main(string[] args)
    {
       foreach (TimeZoneInfo z in TimeZoneInfo.GetSystemTimeZones())
       {
	// For a Console App
    	Console.WriteLine(z.Id + "," + z.BaseUtcOffset + "," + z.StandardName + "," + z.DisplayName + "," + z.DaylightName);
	// For any other App
	System.Diagnostics.Debug.WriteLine(z.Id + "," + z.BaseUtcOffset + "," + z.StandardName + "," + z.DisplayName + "," + z.DaylightName);
       }
    }
  }
}
	
	

----------------------------------------		
	
<option value="GMT Standard Time">(GMT) Greenwich Mean Time : Dublin, Edinburgh, Lisbon, London</option>
<option value="Greenwich Standard Time">(GMT) Monrovia, Reykjavik</option>
<option value="China Standard Time">(GMT+08:00) Beijing, Chongqing, Hong Kong, Urumqi</option>
<option value="Cen. Australia Standard Time">(GMT+09:30) Adelaide</option>
<option value="AUS Central Standard Time">(GMT+09:30) Darwin</option>
<option value="E. Australia Standard Time">(GMT+10:00) Brisbane</option>
<option value="AUS Eastern Standard Time">(GMT+10:00) Canberra, Melbourne, Sydney</option>
<option value="West Pacific Standard Time">(GMT+10:00) Guam, Port Moresby</option>
<option value="Tasmania Standard Time">(GMT+10:00) Hobart</option>
<option value="Vladivostok Standard Time">(GMT+10:00) Vladivostok</option>
<option value="Central Pacific Standard Time">(GMT+11:00) Magadan, Solomon Is., New Caledonia</option>
<option value="New Zealand Standard Time">(GMT+12:00) Auckland, Wellington</option>
<option value="Fiji Standard Time">(GMT+12:00) Fiji, Kamchatka, Marshall Is.</option>
<option value="Tonga Standard Time">(GMT+13:00) Nuku'alofa</option>
```
{% endcode %}



{% code fullWidth="true" %}
```

----------------------------------------	 
C# set time  
----------------------------------------		
	
//"en-us"	
string cultureStr = "en-au";    
weighRecordVM.WeighDateTime = converLocalTimeToCultureTime(weighRecordVM.WeighDateTime, cultureStr);

weighRecordVM.WeighDateTime = weighRecordVM.WeighDateTime.ToUniversalTime();

//cunvert Datatime to US type
var convert_time = DateTime.Parse(weighRecordVM.WeighDateTime.ToString("G", CultureInfo.CreateSpecificCulture("en-US")));
weighRecordVM.WeighDateTime = convert_time;
	
	

private DateTime converLocalTimeToCultureTime(DateTime localTime, string cultureStr)
{
    DateTime convert_time;

    if (cultureStr == "en-us")
    {
        convert_time = DateTime.Parse(localTime.ToString("G", CultureInfo.CreateSpecificCulture("en-US")));
        _logger.LogInformation("Before convert Datetime:" + convert_time.ToString());
        return convert_time;
    }
    else if (cultureStr == "en-au")
    {

        var cultureInfo = new CultureInfo("en-AU");
        if (DateTime.TryParse(localTime.ToString("G", cultureInfo), out convert_time))
        {
            _logger.LogInformation("Before convert Datetime:" + convert_time.ToString());
            return convert_time;
        }
        return (DateTime)(DateTime?)null;
    }
    else
        return (DateTime)(DateTime?)null;
}
 
```
{% endcode %}

{% code fullWidth="true" %}
```
// C# API Datatime format
		

public class CustomDateTimeConverter : IsoDateTimeConverter
{
	public CustomDateTimeConverter()
	{
		base.DateTimeFormat = "yyyy'-'MM'-'dd'T'HH':'mm':'ssZ";
	}
}

public class MyViewModel
{
	[JsonConverter(typeof(CustomDateTimeConverter))]
	public DateTime MyDate { get; set; }
}	
		
```
{% endcode %}

>
