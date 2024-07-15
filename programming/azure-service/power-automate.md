# Power Automate

```
// Convert UCT to Local Adelaide time:

Power Bi:
         AdelaideTime = NOW()+9.5/24 
		 
Automate:
 (MSN weather)
    addMinutes(convertTimeZone(outputs('Get_current_weather')?['body/responses/weather/current/created'],'UTC','AUS Eastern Standard Time'),-30)
 (Sensor)
    addMinutes(convertTimeZone(outputs('Run_KQL_query')?['body/value/timestamp'],'UTC','AUS Eastern Standard Time'),-30)
```

```
// Wind speed: mPh to km/h
Power bi:
          WindSpeedKmh = AVERAGE(RealTimeData[Wind Speed])*1.609344
		  
Automate: 
          mul(outputs('Get_current_weather')?['body/responses/weather/current/windSpd'],1.609344)

```

```
// Temperature Fahrenheit to Celsius
Power Bi:
	TemperatureCelsius = (AVERAGE(RealTimeData[Temperature])-32) * 5/9
	
Automate:
 (MSN weather)
    div(mul(sub(outputs('Get_current_weather')?['body/responses/weather/current/temp'],32),5),9)
	
	
	
	div(mul(sub(body('Get_forecast_for_today')?['responses']?['daily']?['tempLo'],32),5),9)
	div(mul(sub(body('Get_forecast_for_today')?['responses']?['daily']?['tempHi'],32),5),9)

 (Sensor)
    div(mul(sub(outputs('Run_KQL_query')?['body/value/temperature'],32),5),9)
```



```
// The Temperature Conversion (Fahrenheit / Celsius) in Power Automate

MSN Weather

F to C: Example: (50°F - 32) * 5/9 = 10°C)

  div(mul(sub(outputs('Get_current_weather')?['body/responses/weather/current/temp'],32),5),9)


C to F: Example: (30°C * 1.8) + 32 = 86°F’)

  add(mul(outputs('Get_current_weather')?['body/responses/weather/current/temp'],1.8),32)
```
