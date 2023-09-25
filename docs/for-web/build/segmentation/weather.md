# Segment by Weather

You can segment by the user's weather in Webtrends Optimize, or use these values as part of your test code by accessing the values with JavaScript.

**Note:** We will need to enable this for you - we default to downloading as little data as possible and so don't add these values for customers unless required. Please email us at support@webtrends-optimize.com to get it enabled.

## Available Values

The following values are available - here are the names and descriptions.

| Attribute               | Data Type | Description |
| ----------------------- | --------- | ----------- |
| weather_maxtemp_c | decimal | Maximum temperature in celsius for the day.
| weather_maxtemp_f | decimal | Maximum temperature in fahrenheit for the day         
| weather_avgtemp_c | decimal | Average temperature in celsius for the day
| weather_avgtemp_f | decimal | Average temperature in fahrenheit for the day
| weather_maxwind_mph | decimal | Maximum wind speed in miles per hour
| weather_maxwind_kph | decimal | Maximum wind speed in kilometer per hour
| weather_totalprecip_mm | decimal | Total precipitation in milimeter
| weather_totalprecip_in | decimal | Total precipitation in inches
| weather_totalsnow_cm | decimal | Expected daily snow in cm.<br>E.g. 2.5
| weather_avgvis_km | decimal | Average visibility in kilometer<br>E.g. 10.0
| weather_avgvis_miles | decimal | Average visibility in miles<br>E.g. 6.0
| weather_avghumidity | integer | Average humidity as percentage<br>E.g. 74
| weather_daily_will_it_rain | integer | Will it will rain or not.<br>1 = Yes, 2 = No
| weather_daily_will_it_snow | integer | Will it will snow or not.<br>1 = Yes, 2 = No
| weather_daily_chance_of_rain | integer | Chance of rain as percentage
| weather_daily_chance_of_snow | integer | Chance of snow as percentage
| weather_condition_text | string | Weather condition text. See possible values below.
| weather_condition_text_simple | string | Simplified version of weather_condition_text
| weather_condition_code | integer | Weather condition code. See possible values below.
| weather_uv_index | decimal | The UV index.<br>E.g. 2.0
| weather_uv | string | Translated UV band.<br><br>Possible values:<br>- **low**: Under 3.0<br>- **moderate**: 3.0 to 5.9<br>- **high**: 6.0 to 7.9<br>- **very high**: 8.0 to 10.9<br>- **extreme**: 11.0+

## Weather Condition values

Here are a full list of weather condition codes and weather condition text values.

| weather_condition_code | weather_condition_text | weather_condition_text_simple
| ---------------------- | ---------------------- | -----------------------------
| 1000 | Sunny | sunny
| 1003 | Partly cloudy | cloudy
| 1006 | Cloudy | cloudy
| 1009 | Overcast | cloudy
| 1030 | Mist | rain
| 1063 | Patchy rain possible | rain
| 1066 | Patchy snow possible | snow
| 1069 | Patchy sleet possible | snow
| 1072 | Patchy freezing drizzle possible | rain
| 1087 | Thundery outbreaks possible | rain
| 1114 | Blowing snow | snow
| 1117 | Blizzard | snow
| 1135 | Fog | fog
| 1147 | Freezing fog | fog
| 1150 | Patchy light drizzle | rain
| 1153 | Light drizzle | rain
| 1168 | Freezing drizzle | rain
| 1171 | Heavy freezing drizzle | rain
| 1180 | Patchy light rain | rain
| 1183 | Light rain | rain
| 1186 | Moderate rain at times | rain
| 1189 | Moderate rain | rain
| 1192 | Heavy rain at times | rain
| 1195 | Heavy rain | rain
| 1198 | Light freezing rain | rain
| 1201 | Moderate or heavy freezing rain | rain
| 1204 | Light sleet | snow
| 1207 | Moderate or heavy sleet | snow
| 1210 | Patchy light snow | snow
| 1213 | Light snow | snow
| 1216 | Patchy moderate snow | snow
| 1219 | Moderate snow | snow
| 1222 | Patchy heavy snow | snow
| 1225 | Heavy snow | snow
| 1237 | Ice pellets | snow
| 1240 | Light rain shower | rain
| 1243 | Moderate or heavy rain shower | rain
| 1246 | Torrential rain shower | rain
| 1249 | Light sleet showers | snow
| 1252 | Moderate or heavy sleet showers | snow
| 1255 | Light snow showers | snow
| 1258 | Moderate or heavy snow showers | snow
| 1261 | Light showers of ice pellets | snow
| 1264 | Moderate or heavy showers of ice pellets | snow
| 1273 | Patchy light rain with thunder | rain
| 1276 | Moderate or heavy rain with thunder | rain
| 1279 | Patchy light snow with thunder | snow
| 1282 | Moderate or heavy snow with thunder | snow