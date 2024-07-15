---
description: https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/
---

# Kusto Query Language

````
```kusto function
//*** Float-point Dec to dec
//*** Formula = sign * (1 + fraction) * 2^(exponent-bias)
let FloatPointDEC_to_DEC = (floatPointDec:int, sign:int){
   // sign = 1: positive, -1: negtive
   let bias = 127;
   let exponent = binary_shift_right(floatPointDec, 23);  // ==>131
   let fraction = binary_shift_right((binary_shift_left(floatPointDec,9)-binary_shift_left(binary_shift_right(floatPointDec,32-9),32)),9);  // =>2322689
   //bit_feed = 1 to 23, from left to right in fraction
   //sample fraction: 2322689(dec) eque to 01000110111000100000001(bin)
   //get fraction bit value
   let getFractionBitValue=(fraction:int, bit_feed:int){
      let m_bit = 23 + 1 - bit_feed;
      let bit_value = binary_shift_right(fraction, m_bit-1) -binary_shift_left(binary_shift_right(fraction,m_bit),1);
      todecimal(bit_value * exp2(-bit_feed))
   };
   let fraction_bit_value = getFractionBitValue(fraction, 1) + getFractionBitValue(fraction, 2)
                              + getFractionBitValue(fraction, 3) + getFractionBitValue(fraction, 4)
                              + getFractionBitValue(fraction, 5) + getFractionBitValue(fraction, 6)
                              + getFractionBitValue(fraction, 7) + getFractionBitValue(fraction, 8)
                              + getFractionBitValue(fraction, 9) + getFractionBitValue(fraction, 10)
                              + getFractionBitValue(fraction, 11) + getFractionBitValue(fraction, 12)
                              + getFractionBitValue(fraction, 13) + getFractionBitValue(fraction, 14)
                              + getFractionBitValue(fraction, 15) + getFractionBitValue(fraction, 16)
                              + getFractionBitValue(fraction, 17) + getFractionBitValue(fraction, 18)
                              + getFractionBitValue(fraction, 19) + getFractionBitValue(fraction, 20)
                              + getFractionBitValue(fraction, 21) + getFractionBitValue(fraction, 22)
                              + getFractionBitValue(fraction, 23) + 1;
   sign * fraction_bit_value * exp2(exponent - bias);
};
//print(FloatPointDEC_to_DEC(1101230337,1));
````

````
```kusto function
// combine high and low float-point value
let Combine_HighLow = (floatPointHigh: string, floatPointLow:string){
	toreal(floatPointHigh)*65536 + toreal(floatPointLow);
};

````

````
```kusto function
// convert wind direction from degree (0~365) to text
let WindDirectionWord = (windDirection: string){
	let windDirectionValue = toreal(windDirection);
   iff(isnull(windDirectionValue) or isempty(windDirectionValue) or isnan(windDirectionValue),'',
	iff(windDirectionValue>=348.75,'N',
	iff(windDirectionValue> 326.25, 'NNW',
		iff(windDirectionValue> 303.75, 'NW',  
			iff(windDirectionValue> 281.25, 'WNW',  
				iff(windDirectionValue> 258.75, 'W',  
					iff(windDirectionValue> 236.25, 'WSW',  
						iff(windDirectionValue> 213.75, 'SW',  
							iff(windDirectionValue> 191.25, 'SSW',  
								iff(windDirectionValue> 168.75, 'S',  
									iff(windDirectionValue> 146.25, 'SSE',  
										iff(windDirectionValue> 123.75, 'SE',  
											iff(windDirectionValue> 101.25, 'ESE',  
												iff(windDirectionValue> 78.75, 'E',  
													iff(windDirectionValue> 56.25, 'ENE',  
														iff(windDirectionValue> 33.75, 'NE',  
															iff(windDirectionValue> 11.25, 'NNE', 'N')))))))))))))))))
};

````

````
// ```kusto Query
let detailed_data = materialize(WeatherStation
| where isnotempty(HumidityHigh) and isnotempty(HumidityLow) and isnotempty(TemperatureHigh)
| sort by timestamp
| take 200
| project timestamp, Humidity=Combine_HighLow(HumidityHigh, HumidityLow),
	Pressure = Combine_HighLow(PressureHigh, PressureLow),
	Temperature = Combine_HighLow(TemperatureHigh, TemperatureLow),
	WindSpeed = Combine_HighLow(WindSpeedHigh, WindSpeedLow), toreal(WindDirection)
| summarize HumidityDEC = avg(Humidity), TemperatureDEC = avg(Temperature), PressureDEC = avg(Pressure), 
	WindSpeedDEC = avg(WindSpeed), WindDirectionDEC = avg(WindDirection), Timestamp = max(timestamp), Count=count());
detailed_data
| where Count > 0
| extend LocalAdelaideTime= datetime_utc_to_local(Timestamp,'Australia/Adelaide')
| project HumidityDEC, TemperatureDEC, PressureDEC, WindSpeedDEC, Humidity = FloatPointDEC_to_DEC(HumidityDEC, 1), Temperature = FloatPointDEC_to_DEC(TemperatureDEC, 1),
	Pressure = FloatPointDEC_to_DEC(PressureDEC, 1), WindSpeed = FloatPointDEC_to_DEC(WindSpeedDEC, 1), 
	WindDirection = WindDirectionWord(WindDirectionDEC), Timestamp, LocalAdelaideTime
```
````



````
// convert utc to localtime
```kusto
print(datetime_list_timezones())  // get timezones words
```
// -30 minute
Location
| top 100 by ip
| sort by timestamp
| limit 5
| extend AdelaideTime=datetime_add('minute',-30, make_datetime(timestamp))

// get local time
Weight | take 100 | sort by timestamp
|project weight1=round(todecimal(Weight1),2), weight2=round(todecimal(Weight2),2), weight3=round(todecimal(Weight3),2), timestamp
| extend AdelaideTime = datetime_utc_to_local(timestamp,'Australia/Adelaide')
````



```
// join two table
let weight = materialize(Weight 
| take 10
| sort by timestamp
|project weight1=round(todecimal(Weight1),2), weight2=round(todecimal(Weight2),2), weight3=round(todecimal(Weight3),2), timestamp
| extend AdelaideTime = datetime_utc_to_local(timestamp,'Australia/Adelaide'), weightTotal=weight1+weight2+weight3);
let location = materialize(Location
| take 10
| sort by timestamp
| project Latitude, Longitude, timestamp
| extend AdelaideTime=datetime_add('minute',-30, make_datetime(timestamp))
);
location
| join kind=fullouter weight on AdelaideTime   // kind=inner ....
```

```
//average weight from last 100 records and join last record from location
let Weight = materialize(Weight 
| project Weight1, Weight2, Weight3, timestamp
| extend UTC_Now= datetime(now)
| extend AdelaideTime = datetime_utc_to_local(UTC_Now,'Australia/Adelaide')
| extend AdelaideTimeFromTimestamp = datetime_utc_to_local(timestamp,'Australia/Adelaide')
| order by timestamp desc  
| take 15
| project weight1 = todecimal(Weight1), weight2 = todecimal(Weight2), weight3 = todecimal(Weight3) 
| summarize Weight1=round(avg(weight1),2), Weight2=round(avg(weight2),2), Weight3=round(avg(weight3),2), WeightTotal=round(avg(weight1+weight2+weight3),2)
| extend UTC_Now= datetime(now)
);
let Location = materialize(Location
| order by timestamp desc 
| project Latitude, Longitude, timestamp
| extend UTC_Now= datetime(now)
| extend AdelaideTime = datetime_utc_to_local(UTC_Now,'Australia/Adelaide')
| take 1
);
Location
| project Latitude, Longitude, UTC_Now1 = UTC_Now
| join (Weight
      | project Weight1, Weight2, Weight3, WeightTotal, UTC_Now)
   on $left.UTC_Now1 ==  $right.UTC_Now
| extend AdelaideTime = datetime_utc_to_local(UTC_Now,'Australia/Adelaide')
| project AdelaideTime, Latitude, Longitude, Weight1, Weight2, Weight3, WeightTotal, Min=0, Max=60

```

