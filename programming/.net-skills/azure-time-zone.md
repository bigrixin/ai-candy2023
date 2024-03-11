# Azure Time zone



{% code fullWidth="true" %}
```
// Time Zone Service

  public class TimeZoneService : ITimeZoneService
  {

      private readonly ILogger<TimeZoneService> _logger;
      public TimeZoneService(ILogger<TimeZoneService> logger)
      {
          this._logger = logger;
      }

      [Obsolete]
      public string CurrentTimeZoneName()
      {
          return TimeZone.CurrentTimeZone.StandardName;
      }

      public List<string> GetSystemTimeZoneDisplayNameList()
      {
          var systemTimeZoneList = TimeZoneInfo.GetSystemTimeZones();
          var timezoneList = new List<string>();
          foreach (var systemTimeZone in systemTimeZoneList)
              timezoneList.Add(systemTimeZone.DisplayName + " - " + systemTimeZone.Id);

          return timezoneList;
      }


      [Obsolete]
      public List<string> GetCurrentTimeZoneInfoList(string settingTimeZone, string localTimeStr, string localTimezoneName)
      {
          DateTime timeUtc = DateTime.UtcNow;
          var list = new List<string>();

          // get current timezone name   -- Azure timezone: 'Coordinated Universal Time'
          string timeZoneName = CurrentTimeZoneName();
          string timezoneInfo = "(Server) Timezone: " + timeZoneName;
          list.Add(timezoneInfo);
          _logger.LogInformation(timezoneInfo);

          // get current (Server) time
          DateTime currenttime = DateTime.Now;
          string currentTimeInfo = "Current (Server) Time: " + currenttime.ToString("dd/MM/yyyy HH:mm:ss");
          list.Add(currentTimeInfo);
          _logger.LogInformation(currentTimeInfo);


          // get utc time 
          string utcTimeInfo = "UTC Time: " + timeUtc.ToString("dd/MM/yyyy HH:mm:ss");
          list.Add(utcTimeInfo);
          _logger.LogInformation(utcTimeInfo);


          // get local (UI) time
          DateTime localtime = DateTime.Now;
          if (localTimeStr != null)
              localtime = DateTime.Parse(localTimeStr);

          string localTimeInfo = "Local (UI) Time: " + localtime.ToString("dd/MM/yyyy HH:mm:ss");
          list.Add(localTimeInfo);
          _logger.LogInformation(localTimeInfo);

          // get timezone id by local timezone name
          string localTimezoneId = GetAustraliaCityTimeZoneIdByName(localTimezoneName);
          if (localTimezoneId != null)
          {
              TimeZoneInfo ltz = TimeZoneInfo.FindSystemTimeZoneById(localTimezoneId);

              // get local timezone offset hours
              TimeSpan localOffset = ltz.GetUtcOffset(localtime);
              string localOffsetHoursInfo = "Local Timezone Offset hours: " + localOffset.Hours.ToString();
              list.Add(localOffsetHoursInfo);
              _logger.LogInformation(localOffsetHoursInfo);
          }

          // local timezone city
          string city = GetAustraliaCityNameByTimezoneName(localTimezoneName);
          if (city != null)
          {
              string localCityInfo = "Local Timezone City: " + city;
              list.Add(localCityInfo);
              _logger.LogInformation(localCityInfo);
          }

          // get setting timezone and offset hours ----  "TimeZone": "AUS Eastern Standard Time",
          TimeZoneInfo tzi = TimeZoneInfo.FindSystemTimeZoneById(settingTimeZone);
          DateTime cstTime = TimeZoneInfo.ConvertTimeFromUtc(timeUtc, tzi);

          // setting timezone name
          string settingTimezone = "Setting Timezone: " + settingTimeZone;
          list.Add(settingTimezone);
          _logger.LogInformation(settingTimezone);


          // get setting timezone offset hours
          TimeSpan offset = tzi.GetUtcOffset(timeUtc);
          string settingOffsetHoursInfo = "Setting Timezone Offset hours: " + offset.Hours.ToString();
          list.Add(settingOffsetHoursInfo);
          _logger.LogInformation(settingOffsetHoursInfo);



          // get Sydney time
          string sydneytimeInfo = "Sydney Time: " + cstTime.ToString("dd/MM/yyyy HH:mm:ss");
          list.Add(sydneytimeInfo);
          _logger.LogInformation(sydneytimeInfo);


          return list;
      }

      public List<string> SystemTimeZoneNameList()
      {
          List<string> timeZoneNameList = new List<string>();
          var tzList = TimeZoneInfo.GetSystemTimeZones().ToList();

          foreach (var tz in tzList)
          {
              timeZoneNameList.Add(tz.StandardName);
          }
          return timeZoneNameList;
      }

      public List<string> SystemTimeZoneIDList()
      {
          List<string> timeZoneId = new List<string>();
          var tzList = TimeZoneInfo.GetSystemTimeZones().ToList();

          foreach (var tz in tzList)
          {
              timeZoneId.Add(tz.Id);
          }

          return timeZoneId;
      }


      [Obsolete]
      public int GetTimeZoneOffsetHours(string timeZoneId)
      {
          // timeZoneId == "AUS Eastern Standard Time";
          TimeZone localZone = TimeZone.CurrentTimeZone;
          DateTime currentDate = DateTime.Now;

          var localTime = localZone.ToLocalTime(currentDate);
          var utcTime = localZone.ToUniversalTime(currentDate);

          _logger.LogInformation($"location time: " + localTime.ToString("MM/dd/yyyy HH:mm:ss"));
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

      public int GetTimeZoneOffsetHoursByLocalTimezone(string localTimezoneName, string localTimeStr)
      {
          // get local (UI) time
          DateTime localtime = DateTime.Now;
          if (localTimeStr != null)
              localtime = DateTime.Parse(localTimeStr);

          // get timezone id by local timezone name
          string localTimezoneId = GetAustraliaCityTimeZoneIdByName(localTimezoneName);
          if (localTimezoneId != null)
          {
              TimeZoneInfo ltz = TimeZoneInfo.FindSystemTimeZoneById(localTimezoneId);

              // get local timezone offset hours
              TimeSpan localOffset = ltz.GetUtcOffset(localtime);
              _logger.LogInformation("Local Timezone Offset hours: " + localOffset.Hours.ToString());
              return localOffset.Hours;
          }
          return 0;
      }


      public string GetAustraliaCityTimeZoneIdByName(string timeZoneName)
      {
          var list = AustraliaTimeZoneList();

          //get .net timezone id by js timezone name
          var result = list.Where(x => x.Item1 == timeZoneName).FirstOrDefault();
          return result.Item3;
      }

      public string GetAustraliaCityNameByTimezoneName(string timeZoneName)
      {
          var list = AustraliaTimeZoneList();

          //get city name by js timezone name
          var result = list.Where(x => x.Item1 == timeZoneName).FirstOrDefault();
          return result.Item2;
      }

      public string GetTimeZoneById(string timeZoneId)
      {
          var list = timeZoneList();    // or SystemTimeZoneIDList();

          var result = list.Where(x => x.Item1 == timeZoneId).FirstOrDefault();
          return result.Item3;
      }

      #region cron


      public string GetHourTimeCronStr(int hour, int minute)
      {
          // Cron("0 1 * * *") >> run at 1:00 AM daily 

          var h = hour.ToString();
          if (h.Length == 1)
              h = "0" + h;

          var m = minute.ToString();
          if (m.Length == 1)
              m = "0" + m;

          return m + " " + h;
      }

      public string GetOffsetHourTimeCronStr(int hour, int minute, int offsetHours)
      {
          // 08:26 ==> '26 08'
          if (hour >= offsetHours)
              hour = hour - offsetHours;
          else
              hour = hour + 24 - offsetHours;

          var h = hour.ToString();
          if (h.Length == 1)
              h = "0" + h;

          var m = minute.ToString();
          if (m.Length == 1)
              m = "0" + m;

          string cronTimeStr = m + " " + h;

          return cronTimeStr;
      }

      #endregion


      #region appsetting

      public string GetAppSettingTimeZone(string sectionName)
      {
          var jsonStr = GetAppSettingJsonStr(sectionName);
          return (string)jsonStr["TimeZone"];
      }


      public JToken GetAppSettingJsonStr(string sectionName)
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



      public void UpdateAppSetting(string key, string value, string schedule)
      {
          switch (schedule)
          {
              case "monthly":
                  value = value + " 1 * *";
                  break;
              case "weekly":
                  value = value + " * * 1";
                  break;
              case "daily":
                  value = value + " * * *";
                  break;
              default:
                  break;
          }

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
              _logger.LogInformation($"Error writing app settings");
              Console.WriteLine("Error writing app settings");
          }
      }

      #endregion

      private List<Tuple<string, string, string>> AustraliaTimeZoneList()
      {
          // JS name, JS City, .NET id
          var list = new List<Tuple<string, string, string>>{
              new Tuple<string,string,string>("Australian Western Standard Time", "Perth", "W. Australia Standard Time"),                     // +8
              new Tuple<string,string,string>("Australian Central Western Standard Time", "Eucla", "Aus Central W. Standard Time"),           // +8:45
              new Tuple<string,string,string>("Australian Central Standard Time", "Darwin", "AUS Central Standard Time"),                     // +9:30/+10:30
              new Tuple<string,string,string>("Australian Eastern Standard Time", "Brisbane", "E. Australia Standard Time"),                  // +10
              new Tuple<string,string,string>("Australian Central Daylight Time", "Adelaide", "Cen. Australia Standard Time"),                // +10:30
              new Tuple<string,string,string>("Australian Eastern Daylight Time", "Canberra/Sydney/Melbourne", "AUS Eastern Standard Time"),  // +11/+10
              new Tuple<string,string,string>("Lord Howe Daylight Time", "Lord Howe Island", "Lord Howe Standard Time")};                     // +11/+10

          return list;
      }


      private List<Tuple<string, string, string>> timeZoneList()
      {
          // .NET id, display-name
          var list = new List<Tuple<string, string, string>>{
                  new Tuple<string,string,string>("Dateline Standard Time", "(UTC-12:00)", "W. Australia Standard Time"),
                  new Tuple<string,string,string>("UTC-11", "(UTC-11:00)", "Coordinated Universal Time-11"),
                  new Tuple<string,string,string>("Aleutian Standard Time", "(UTC-10:00)", "Aleutian Islands"),
                  new Tuple<string,string,string>("Hawaiian Standard Time", "(UTC-10:00)", "Hawaii"),
                  new Tuple<string,string,string>("Marquesas Standard Time", "(UTC-09:30)", "Marquesas Islands"),
                  new Tuple<string,string,string>("Alaskan Standard Time", "(UTC-09:00)", "Alaska"),
                  new Tuple<string,string,string>("UTC-09", "(UTC-09:00)", "Coordinated Universal Time-09"),
                  new Tuple<string,string,string>("Pacific Standard Time (Mexico)", "(UTC-08:00)", "Baja California"),
                  new Tuple<string,string,string>("UTC-08", "(UTC-08:00)", "Coordinated Universal Time-08"),
                  new Tuple<string,string,string>("Pacific Standard Time", "(UTC-08:00)", "Pacific Time (US & Canada)"),
                  new Tuple<string,string,string>("US Mountain Standard Time", "(UTC-07:00)", "Arizona"),
                  new Tuple<string,string,string>("Mountain Standard Time (Mexico)", "(UTC-07:00)", "Chihuahua, La Paz, Mazatlan"),
                  new Tuple<string,string,string>("Mountain Standard Time", "(UTC-07:00)", "Mountain Time (US & Canada)"),
                  new Tuple<string,string,string>("Central America Standard Time", "(UTC-06:00)", "Central America"),
                  new Tuple<string,string,string>("Central Standard Time", "(UTC-06:00)", "Central Time (US & Canada)"),
                  new Tuple<string,string,string>("Easter Island Standard Time", "(UTC-06:00)", "Easter Island"),
                  new Tuple<string,string,string>("Central Standard Time (Mexico)", "(UTC-06:00)", "Guadalajara, Mexico City, Monterrey"),
                  new Tuple<string,string,string>("Canada Central Standard Time", "(UTC-06:00)", "Saskatchewan"),
                  new Tuple<string,string,string>("SA Pacific Standard Time", "(UTC-05:00)", "Bogota, Lima, Quito, Rio Branco"),
                  new Tuple<string,string,string>("Eastern Standard Time (Mexico)", "(UTC-05:00)", "Chetumal"),
                  new Tuple<string,string,string>("Eastern Standard Time", "(UTC-05:00)", "Eastern Time (US & Canada)"),
                  new Tuple<string,string,string>("Haiti Standard Time", "(UTC-05:00)", "Haiti"),
                  new Tuple<string,string,string>("Cuba Standard Time", "(UTC-05:00)", "Havana"),
                  new Tuple<string,string,string>("US Eastern Standard Time", "(UTC-05:00)", "Indiana (East)"),
                  new Tuple<string,string,string>("Turks And Caicos Standard Time", "(UTC-05:00)", "Turks and Caicos"),
                  new Tuple<string,string,string>("Paraguay Standard Time", "(UTC-04:00)", "Asuncion"),
                  new Tuple<string,string,string>("Atlantic Standard Time", "(UTC-04:00)", "Atlantic Time (Canada)"),
                  new Tuple<string,string,string>("Venezuela Standard Time", "(UTC-04:00)", "Caracas"),
                  new Tuple<string,string,string>("Central Brazilian Standard Time", "(UTC-04:00)", "Cuiaba"),
                  new Tuple<string,string,string>("SA Western Standard Time", "(UTC-04:00)", "Georgetown, La Paz, Manaus, San Juan"),
                  new Tuple<string,string,string>("Pacific SA Standard Time", "(UTC-04:00)", "Santiago"),
                  new Tuple<string,string,string>("Newfoundland Standard Time", "(UTC-03:30)", "Newfoundland"),
                  new Tuple<string,string,string>("Tocantins Standard Time", "(UTC-03:00)", "Araguaina"),
                  new Tuple<string,string,string>("E. South America Standard Time", "(UTC-03:00)", "Brasilia"),
                  new Tuple<string,string,string>("SA Eastern Standard Time", "(UTC-03:00)", "Cayenne, Fortaleza"),
                  new Tuple<string,string,string>("Argentina Standard Time", "(UTC-03:00)", "City of Buenos Aires"),
                  new Tuple<string,string,string>("Greenland Standard Time", "(UTC-03:00)", "Greenland"),
                  new Tuple<string,string,string>("Montevideo Standard Time", "(UTC-03:00)", "Montevideo"),
                  new Tuple<string,string,string>("Magallanes Standard Time", "(UTC-03:00)", "Punta Arenas"),
                  new Tuple<string,string,string>("Saint Pierre Standard Time", "(UTC-03:00)", "Saint Pierre and Miquelon"),
                  new Tuple<string,string,string>("Bahia Standard Time", "(UTC-03:00)", "Salvador"),
                  new Tuple<string,string,string>("UTC-02", "(UTC-02:00)", "Coordinated Universal Time-02"),
                  new Tuple<string,string,string>("Mid-Atlantic Standard Time", "(UTC-02:00)", "Mid-Atlantic - Old"),
                  new Tuple<string,string,string>("Azores Standard Time", "(UTC-01:00)", "Azores"),
                  new Tuple<string,string,string>("Cape Verde Standard Time", "(UTC-01:00)", "Cabo Verde Is."),
                  new Tuple<string,string,string>("UTC", "(UTC)", "Coordinated Universal Time"),
                  new Tuple<string,string,string>("GMT Standard Time", "(UTC+00:00)", "Dublin, Edinburgh, Lisbon, London"),
                  new Tuple<string,string,string>("Greenwich Standard Time", "(UTC+00:00)", "Monrovia, Reykjavik"),
                  new Tuple<string,string,string>("W. Europe Standard Time", "(UTC+01:00)", "Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna"),
                  new Tuple<string,string,string>("Central Europe Standard Time", "(UTC+01:00)", "Belgrade, Bratislava, Budapest, Ljubljana, Prague"),
                  new Tuple<string,string,string>("Romance Standard Time", "(UTC+01:00)", "Brussels, Copenhagen, Madrid, Paris"),
                  new Tuple<string,string,string>("Morocco Standard Time", "(UTC+01:00)", "Casablanca"),
                  new Tuple<string,string,string>("Sao Tome Standard Time", "(UTC+01:00)", "Sao Tome"),
                  new Tuple<string,string,string>("Central European Standard Time", "(UTC+01:00)", "Sarajevo, Skopje, Warsaw, Zagreb"),
                  new Tuple<string,string,string>("W. Central Africa Standard Time", "(UTC+01:00)", "West Central Africa"),
                  new Tuple<string,string,string>("Jordan Standard Time", "(UTC+02:00)", "Amman"),
                  new Tuple<string,string,string>("GTB Standard Time", "(UTC+02:00)", "Athens, Bucharest"),
                  new Tuple<string,string,string>("Middle East Standard Time", "(UTC+02:00)", "Beirut"),
                  new Tuple<string,string,string>("Egypt Standard Time", "(UTC+02:00)", "Cairo"),
                  new Tuple<string,string,string>("E. Europe Standard Time", "(UTC+02:00)", "Chisinau"),
                  new Tuple<string,string,string>("Syria Standard Time", "(UTC+02:00)", "Damascus"),
                  new Tuple<string,string,string>("West Bank Standard Time", "(UTC+02:00)", "Gaza, Hebron"),
                  new Tuple<string,string,string>("South Africa Standard Time", "(UTC+02:00)", "Harare, Pretoria"),
                  new Tuple<string,string,string>("FLE Standard Time", "(UTC+02:00)", "Helsinki, Kyiv, Riga, Sofia, Tallinn, Vilnius"),
                  new Tuple<string,string,string>("Israel Standard Time", "(UTC+02:00)", "Jerusalem"),
                  new Tuple<string,string,string>("Kaliningrad Standard Time", "(UTC+02:00)", "Kaliningrad"),
                  new Tuple<string,string,string>("Sudan Standard Time", "(UTC+02:00)", "Khartoum"),
                  new Tuple<string,string,string>("Libya Standard Time", "(UTC+02:00)", "Tripoli"),
                  new Tuple<string,string,string>("Namibia Standard Time", "(UTC+02:00)", "Windhoek"),
                  new Tuple<string,string,string>("Arabic Standard Time", "(UTC+03:00)", "Baghdad"),
                  new Tuple<string,string,string>("Turkey Standard Time", "(UTC+03:00)", "Istanbul"),
                  new Tuple<string,string,string>("Arab Standard Time", "(UTC+03:00)", "Kuwait, Riyadh"),
                  new Tuple<string,string,string>("Belarus Standard Time", "(UTC+03:00)", "Minsk"),
                  new Tuple<string,string,string>("Russian Standard Time", "(UTC+03:00)", "Moscow, St. Petersburg"),
                  new Tuple<string,string,string>("E. Africa Standard Time", "(UTC+03:00)", "Nairobi"),
                  new Tuple<string,string,string>("Iran Standard Time", "(UTC+03:30)", "Tehran"),
                  new Tuple<string,string,string>("Arabian Standard Time", "(UTC+04:00)", "Abu Dhabi, Muscat"),
                  new Tuple<string,string,string>("Astrakhan Standard Time", "(UTC+04:00)", "Astrakhan, Ulyanovsk"),
                  new Tuple<string,string,string>("Azerbaijan Standard Time", "(UTC+04:00)", "Baku"),
                  new Tuple<string,string,string>("Russia Time Zone 3", "(UTC+04:00)", "Izhevsk, Samara"),
                  new Tuple<string,string,string>("Mauritius Standard Time", "(UTC+04:00)", "Port Louis"),
                  new Tuple<string,string,string>("Saratov Standard Time", "(UTC+04:00)", "Saratov"),
                  new Tuple<string,string,string>("Georgian Standard Time", "(UTC+04:00)", "Tbilisi"),
                  new Tuple<string,string,string>("Volgograd Standard Time", "(UTC+04:00)", "Volgograd"),
                  new Tuple<string,string,string>("Caucasus Standard Time", "(UTC+04:00)", "Yerevan"),
                  new Tuple<string,string,string>("Afghanistan Standard Time", "(UTC+04:30)", "Kabul"),
                  new Tuple<string,string,string>("West Asia Standard Time", "(UTC+05:00)", "Ashgabat, Tashkent"),
                  new Tuple<string,string,string>("Ekaterinburg Standard Time", "(UTC+05:00)", "Ekaterinburg"),
                  new Tuple<string,string,string>("Pakistan Standard Time", "(UTC+05:00)", "Islamabad, Karachi"),
                  new Tuple<string,string,string>("India Standard Time", "(UTC+05:30)", "Chennai, Kolkata, Mumbai, New Delhi"),
                  new Tuple<string,string,string>("Sri Lanka Standard Time", "(UTC+05:30)", "Sri Jayawardenepura"),
                  new Tuple<string,string,string>("Nepal Standard Time", "(UTC+05:45)", "Kathmandu"),
                  new Tuple<string,string,string>("Central Asia Standard Time", "(UTC+06:00)", "Nur-Sultan"),
                  new Tuple<string,string,string>("Bangladesh Standard Time", "(UTC+06:00)", "Dhaka"),
                  new Tuple<string,string,string>("Omsk Standard Time", "(UTC+06:00)", "Omsk"),
                  new Tuple<string,string,string>("Myanmar Standard Time", "(UTC+06:30)", "Yangon (Rangoon)"),
                  new Tuple<string,string,string>("SE Asia Standard Time", "(UTC+07:00)", "Bangkok, Hanoi, Jakarta"),
                  new Tuple<string,string,string>("Altai Standard Time", "(UTC+07:00)", "Barnaul, Gorno-Altaysk"),
                  new Tuple<string,string,string>("W. Mongolia Standard Time", "(UTC+07:00)", "Hovd"),
                  new Tuple<string,string,string>("North Asia Standard Time", "(UTC+07:00)", "Krasnoyarsk"),
                  new Tuple<string,string,string>("N. Central Asia Standard Time", "(UTC+07:00)", "Novosibirsk"),
                  new Tuple<string,string,string>("Tomsk Standard Time", "(UTC+07:00)", "Tomsk"),
                  new Tuple<string,string,string>("China Standard Time", "(UTC+08:00)", "Beijing, Chongqing, Hong Kong, Urumqi"),
                  new Tuple<string,string,string>("North Asia East Standard Time", "(UTC+08:00)", "Irkutsk"),
                  new Tuple<string,string,string>("Singapore Standard Time", "(UTC+08:00)", "Kuala Lumpur, Singapore"),
                  new Tuple<string,string,string>("W. Australia Standard Time", "(UTC+08:00)", "Perth"),
                  new Tuple<string,string,string>("Taipei Standard Time", "(UTC+08:00)", "Taipei"),
                  new Tuple<string,string,string>("Ulaanbaatar Standard Time", "(UTC+08:00)", "Ulaanbaatar"),
                  new Tuple<string,string,string>("Aus Central W. Standard Time", "(UTC+08:45)", "Eucla"),
                  new Tuple<string,string,string>("Transbaikal Standard Time", "(UTC+09:00)", "Chita"),
                  new Tuple<string,string,string>("Tokyo Standard Time", "(UTC+09:00)", "Osaka, Sapporo, Tokyo"),
                  new Tuple<string,string,string>("North Korea Standard Time", "(UTC+09:00)", "Pyongyang"),
                  new Tuple<string,string,string>("Korea Standard Time", "(UTC+09:00)", "Seoul"),
                  new Tuple<string,string,string>("Yakutsk Standard Time", "(UTC+09:00)", "Yakutsk"),
                  new Tuple<string,string,string>("Cen. Australia Standard Time", "(UTC+09:30)", "Adelaide"),
                  new Tuple<string,string,string>("AUS Central Standard Time", "(UTC+09:30)", "Darwin"),
                  new Tuple<string,string,string>("E. Australia Standard Time", "(UTC+10:00)", "Brisbane"),
                  new Tuple<string,string,string>("AUS Eastern Standard Time", "(UTC+10:00)", "Canberra, Melbourne, Sydney"),
                  new Tuple<string,string,string>("West Pacific Standard Time", "(UTC+10:00)", "Guam, Port Moresby"),
                  new Tuple<string,string,string>("Tasmania Standard Time", "(UTC+10:00)", "Hobart"),
                  new Tuple<string,string,string>("Vladivostok Standard Time", "(UTC+10:00)", "Vladivostok"),
                  new Tuple<string,string,string>("Lord Howe Standard Time", "(UTC+10:30)", "Lord Howe Island"),
                  new Tuple<string,string,string>("Bougainville Standard Time", "(UTC+11:00)", "Bougainville Island"),
                  new Tuple<string,string,string>("Russia Time Zone 10", "(UTC+11:00)", "Chokurdakh"),
                  new Tuple<string,string,string>("Magadan Standard Time", "(UTC+11:00)", "Magadan"),
                  new Tuple<string,string,string>("Norfolk Standard Time", "(UTC+11:00)", "Norfolk Island"),
                  new Tuple<string,string,string>("Sakhalin Standard Time", "(UTC+11:00)", "Sakhalin"),
                  new Tuple<string,string,string>("Central Pacific Standard Time", "(UTC+11:00)", "Solomon Is., New Caledonia"),
                  new Tuple<string,string,string>("Russia Time Zone 11", "(UTC+12:00)", "Anadyr, Petropavlovsk-Kamchatsky"),
                  new Tuple<string,string,string>("New Zealand Standard Time", "(UTC+12:00)", "Auckland, Wellington"),
                  new Tuple<string,string,string>("UTC+12", "(UTC+12:00)", "Coordinated Universal Time+12"),
                  new Tuple<string,string,string>("Fiji Standard Time", "(UTC+12:00)", "Fiji"),
                  new Tuple<string,string,string>("Kamchatka Standard Time", "(UTC+12:00)", "Petropavlovsk-Kamchatsky - Old"),
                  new Tuple<string,string,string>("Chatham Islands Standard Time", "(UTC+12:45)", "Chatham Islands"),
                  new Tuple<string,string,string>("UTC+13", "(UTC+13:00)", "Coordinated Universal Time+13"),
                  new Tuple<string,string,string>("Tonga Standard Time", "(UTC+13:00)", "Nuku'alofa"),
                  new Tuple<string,string,string>("Samoa Standard Time", "(UTC+13:00)", "Samoa"),
                  new Tuple<string,string,string>("Line Islands Standard Time", "(UTC+14:00)", "Kiritimati Island")
          };
          return list;
      }

```
{% endcode %}

```
// appsetting.json

  "ScheduledReportTime": {
    "TimeZone": "AUS Eastern Standard Time",
    "daily-report-time": "01 08 * * *",
    "contamination-report-time": "05 08 * * *",
    "weekly-report-time": "15 08 * * 1",
    "monthly-report-time": "20 08 1 * *",
    "monthly-two": "00 11 1 * *",
    "weekly-report-two": "22 13 * * 1"
  },
  
  
//startup.cs
// timezone service
   services.AddScoped<ITimeZoneService, TimeZoneService>();
```

{% code fullWidth="true" %}
```
// send_locaTime_change_appsetting 


   // send localTimeZoneName & localTimeStr
    let localTimeZoneName = new Date().toLocaleDateString(undefined, { day: '2-digit', timeZoneName: 'long' }).substring(4);
    let localTimeStr = formatDate(new Date(), 'YYYY-MM-ddTHH:mm:ss', 'en-AU');
    formData.append("LocalTimezoneName", localTimeZoneName);
    formData.append("LocalTimeStr", localTimeStr);


	_repository.UpdateEmailSetting(newRecord);

	if (await _unitOfWork.SaveAsyc() > 0)
	{
	   // change appsetting.json value with offset hours
	   updateSetingValue(emailSetting.EmailName, emailSetting.SendTime.Hour, emailSetting.SendTime.Minute, emailSettingVM.LocalTimezoneName, emailSettingVM.LocalTimeStr);
       ...
     }



	private void updateSetingValue(string emailReportName, int hour, int minute, string localTimezoneName, string localTimeStr)
	{

		// get local timezone offset hours by local timezone name & time
		int offsetHours = this._timeZoneService.GetTimeZoneOffsetHoursByLocalTimezone(localTimezoneName, localTimeStr);

		_logger.LogInformation($"Get offset hours: " + offsetHours.ToString());
		string cronTime = this._timeZoneService.GetOffsetHourTimeCronStr(hour, minute, offsetHours);// report-time
		_logger.LogInformation($"Get cron time: " + cronTime);

		string sectionKey = sectionName + ':' + emailReportName.Replace('_', '-');
		_logger.LogInformation($"Update SectionKey: " + sectionKey);

		if (emailReportName.Contains("monthly"))
			this._timeZoneService.UpdateAppSetting(sectionKey, cronTime, "monthly");   // monthly = " 1 * *"
		else if (emailReportName.Contains("weekly"))
			this._timeZoneService.UpdateAppSetting(sectionKey, cronTime, "weekly");    // weekly =" * * 1"
		else if (emailReportName.Contains("daily") || emailReportName.Contains("contamination"))
			this._timeZoneService.UpdateAppSetting(sectionKey, cronTime, "daily");     // daily =  " * * *"
	}
```
{% endcode %}

```
// Some code
```
