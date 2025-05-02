# Usage Guide for QWeather MCP

This guide provides detailed examples for using the QWeather MCP tools with various scenarios.

## Setting Up

First, ensure you have properly set up your environment variables:

```bash
export QWEATHER_API_KEY="your_api_key_here"
export QWEATHER_API_HOST="api.qweather.com"  # or your appropriate host
```

## Basic Usage Examples

### 1. Location Search

#### Searching by City Name

```python
from mcp.client import Client

client = Client("unoff-qweather-mcp")

# Search for a city by name
result = await client.lookup_city(location="New York")
print(f"Found {len(result['location'])} locations")

# The first result is usually the most relevant
if result["code"] == "200" and result["location"]:
    location = result["location"][0]
    print(f"Name: {location['name']}")
    print(f"ID: {location['id']}")
    print(f"Country: {location['country']}")
    print(f"Coordinates: {location['lon']}, {location['lat']}")
```

#### Searching by Coordinates

```python
# Search by coordinates (longitude,latitude)
result = await client.lookup_city(location="116.41,39.92")
```

### 2. Current Weather

```python
# Get current weather using the location ID
location_id = "101010100"  # Beijing
weather = await client.weather_now(location=location_id)

if weather["code"] == "200":
    current = weather["now"]
    print(f"Temperature: {current['temp']}°C")
    print(f"Feels Like: {current['feelsLike']}°C")
    print(f"Weather: {current['text']}")
    print(f"Wind: {current['windDir']} {current['windScale']} scale")
    print(f"Humidity: {current['humidity']}%")
    print(f"Precipitation: {current['precip']} mm")
```

### 3. Weather Forecasts

#### Daily Forecast

```python
# Get a 7-day forecast
forecast = await client.weather_forecast_days(location=location_id, days=7)

if forecast["code"] == "200":
    print(f"Forecast for {forecast['fxLink']}")
    for day in forecast["daily"]:
        print(f"Date: {day['fxDate']}")
        print(f"  Temperature: {day['tempMin']}°C to {day['tempMax']}°C")
        print(f"  Day: {day['textDay']}, Night: {day['textNight']}")
        print(f"  Sunrise: {day['sunrise']}, Sunset: {day['sunset']}")
        print(f"  Humidity: {day['humidity']}%")
        print("")
```

#### Hourly Forecast

```python
# Get a 24-hour forecast
hourly = await client.weather_forecast_hours(location=location_id, hours=24)

if hourly["code"] == "200":
    for hour in hourly["hourly"]:
        print(f"Time: {hour['fxTime']}")
        print(f"  Temperature: {hour['temp']}°C")
        print(f"  Weather: {hour['text']}")
        print(f"  Wind: {hour['windDir']} {hour['windScale']} scale")
        print(f"  Precipitation Probability: {hour['pop']}%")
        print("")
```

### 4. Weather Warnings

```python
# Get cities with active warnings in China
warnings = await client.warning_city_list()

if warnings["code"] == "200":
    print(f"There are {len(warnings['locationId'])} cities with active warnings")
    
    # Get detailed warnings for a specific city
    if warnings["locationId"]:
        city_id = warnings["locationId"][0]
        city_warnings = await client.weather_warning(location=city_id)
        
        if city_warnings["code"] == "200" and city_warnings["warning"]:
            for warning in city_warnings["warning"]:
                print(f"Warning Type: {warning['typeName']}")
                print(f"Severity: {warning['severity']}")
                print(f"Title: {warning['title']}")
                print(f"Text: {warning['text']}")
```

### 5. Air Quality

```python
# Get current air quality for a location
latitude = 39.92
longitude = 116.41
air_quality = await client.air_quality_now(latitude=latitude, longitude=longitude)

if air_quality["code"] == "200":
    aqi = air_quality["now"]
    print(f"AQI: {aqi['aqi']}")
    print(f"Category: {aqi['category']}")
    print(f"Primary Pollutant: {aqi['primary']}")
    print(f"PM2.5: {aqi['pm2p5']}")
    print(f"PM10: {aqi['pm10']}")
    print(f"NO2: {aqi['no2']}")
    print(f"SO2: {aqi['so2']}")
    print(f"CO: {aqi['co']}")
    print(f"O3: {aqi['o3']}")
```

### 6. Weather Indices

```python
# Get weather indices for a location
location_id = "101010100"  # Beijing
indices = await client.weather_indices(location=location_id, type="1,2,3,5,6")

if indices["code"] == "200":
    for index in indices["daily"]:
        print(f"Index: {index['name']}")
        print(f"Category: {index['category']}")
        print(f"Level: {index['level']}")
        print(f"Description: {index['text']}")
        print("")
```

## Language and Unit Settings

Most methods support language and unit settings:

```python
# Get weather in English units (imperial)
weather = await client.weather_now(
    location=location_id,
    lang="en",
    unit="i"  # 'i' for imperial, 'm' for metric (default)
)
```

Supported languages include:
- `zh` - Chinese (Simplified)
- `zh-hk` - Chinese (Traditional)
- `en` - English
- `ja` - Japanese
- `ko` - Korean
- `es` - Spanish
- `fr` - French
- And more (refer to QWeather API documentation for the complete list)

## Error Handling

Always check the response code to handle errors:

```python
result = await client.lookup_city(location="invalid-location")

if result["code"] != "200":
    print(f"Error: {result['code']}")
    # Common error codes:
    # 204: No content returned
    # 400: Bad request
    # 401: Unauthorized (API key issue)
    # 402: Too many requests
    # 403: Permission denied
    # 404: Not found
    # 429: Too many requests
    # 500: Server error
```

## Tips for Optimal Usage

1. **Cache LocationIDs**: Store city LocationIDs for frequently accessed locations to reduce API calls
2. **Use Batch Requests**: When possible, request multiple indices at once by using comma-separated values
3. **Handle Timeouts**: Implement proper error handling for network issues
4. **Rate Limits**: Be aware of your QWeather subscription rate limits
5. **Data Freshness**: Check the `obsTime` or `updateTime` fields to verify data recency 