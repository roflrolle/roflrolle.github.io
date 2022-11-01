---
title: "Display the current weather with Powershell"
date: 2022-10-25T20:56:35+02:00
draft: false
toc: true
images:
tags:
  - Powershell
  - API
---

We will display the current weather with a small Powershell script.  
To retrieve the weather we need a free API key from <https://openweathermap.org/> .

## Openweather Api Key

Go to the OpenWeather website and create a free account => <https://openweathermap.org/> .  
Then in your settings you can create your free API key and use it in your script.  

## Powershell Script
In the first line insert your own API key.  
The second and third line is your ZIP and Country code you like to use.  

````powershell
$apikey="MYAPIKEY"
$zipcode="90765"
$country="de"

$urlcoordinates="http://api.openweathermap.org/geo/1.0/zip?zip=$($zipcode),$($country)&appid=$($apikey)"

$MyCoordinates=Invoke-RestMethod -Uri $urlcoordinates -Method Get

$urlWeather="https://api.openweathermap.org/data/2.5/weather?units=metric&lat=$($MyCoordinates.lat)&lon=$($MyCoordinates.lon)&appid=$($apikey)&lang=de"

$MyWeather=Invoke-RestMethod -Uri $urlWeather -Method Get

Write-Output "--- $($myweather.name) | $($MyWeather.main.temp)° | $($MyWeather.weather.description) ---"
````

### Example Output

![Example Output](/weather1.png)

### Response structure

The response structure can be found on the Openweather website and looks something like this:
<https://openweathermap.org/current#current_JSON>

````json
{
  "coord": {
    "lon": 10.99,
    "lat": 44.34
  },
  "weather": [
    {
      "id": 501,
      "main": "Rain",
      "description": "moderate rain",
      "icon": "10d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 298.48,
    "feels_like": 298.74,
    "temp_min": 297.56,
    "temp_max": 300.05,
    "pressure": 1015,
    "humidity": 64,
    "sea_level": 1015,
    "grnd_level": 933
  },
  "visibility": 10000,
  "wind": {
    "speed": 0.62,
    "deg": 349,
    "gust": 1.18
  },
  "rain": {
    "1h": 3.16
  },
  "clouds": {
    "all": 100
  },
  "dt": 1661870592,
  "sys": {
    "type": 2,
    "id": 2075663,
    "country": "IT",
    "sunrise": 1661834187,
    "sunset": 1661882248
  },
  "timezone": 7200,
  "id": 3163858,
  "name": "Zocca",
  "cod": 200
}                     
````