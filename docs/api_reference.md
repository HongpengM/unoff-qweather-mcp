# API Reference

This document provides a comprehensive reference for all available tools in the QWeather MCP package.

## Geo API Tools

### `lookup_city`

Search for cities by location name, coordinates, LocationID, or Adcode.

**Parameters:**
- `location` (required): City name, coordinates (comma-separated longitude,latitude), LocationID or Adcode (China only). Supports fuzzy search with minimum 1 Chinese character or 2 letters.
- `adm` (optional): Administrative division to narrow search and filter results.
- `range` (optional): Limit search to specific country/region using ISO 3166 country code.
- `number` (optional): Number of results to return (1-20, default is 10).
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing city information including ID, name, country, administrative regions, latitude, longitude, and more.

**Example:**
```python
result = await client.lookup_city(location="London")
```

### `lookup_poi`

Lookup POI (Points of Interest) information by keyword and coordinates.

**Parameters:**
- `location` (required): Location to search, can be text, coordinates (comma-separated longitude,latitude), LocationID or Adcode (China only).
- `type` (required): POI type to search. Options include: 'scenic' (scenic spots), 'CSTA' (tide stations), 'TSTA' (tidal current stations)
- `city` (optional): Limit search to specific city. Can be city name or LocationID.
- `number` (optional): Number of results to return (1-20, default is 10).
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing POI information including ID, name, type, address, latitude, longitude, and more.

**Example:**
```python
result = await client.lookup_poi(location="London", type="scenic")
```

## Weather API Tools

### `weather_now`

Get real-time weather data for a specific location.

**Parameters:**
- `location` (required): LocationID or coordinates (comma-separated longitude,latitude). LocationID can be obtained via the GeoAPI.
- `lang` (optional): Language setting for the response.
- `unit` (optional): Unit of measurement, 'm' for metric (default) or 'i' for imperial.

**Returns:**
JSON object containing current weather data including temperature, feels-like temperature, wind direction and speed, humidity, pressure, precipitation, visibility, and more.

**Example:**
```python
weather = await client.weather_now(location="101010100")
```

### `weather_forecast_days`

Get daily weather forecast for 3 to 30 days.

**Parameters:**
- `location` (required): LocationID or coordinates (comma-separated longitude,latitude). LocationID can be obtained via the GeoAPI.
- `days` (optional): Number of forecast days. Valid values are 3, 7, 10, 15, 30. Default is 3.
- `lang` (optional): Language setting for the response.
- `unit` (optional): Unit of measurement, 'm' for metric (default) or 'i' for imperial.

**Returns:**
JSON object containing daily forecast data including sunrise/sunset times, moonrise/moonset times, temperature ranges, weather conditions, wind, humidity, pressure, and more.

**Example:**
```python
forecast = await client.weather_forecast_days(location="101010100", days=7)
```

### `weather_forecast_hours`

Get hourly weather forecast for an extended period.

**Parameters:**
- `location` (required): LocationID or coordinates (comma-separated longitude,latitude). LocationID can be obtained via the GeoAPI.
- `hours` (optional): Number of forecast hours. Valid values are 24, 72, 168. Default is 24.
- `lang` (optional): Language setting for the response.
- `unit` (optional): Unit of measurement, 'm' for metric (default) or 'i' for imperial.

**Returns:**
JSON object containing hourly forecast data including temperature, weather conditions, wind, humidity, pressure, precipitation probability, and more.

**Example:**
```python
hourly = await client.weather_forecast_hours(location="101010100", hours=24)
```

## Weather Warning Tools

### `warning_city_list`

Get a list of cities with active weather disaster warnings for a specified country or region.

**Parameters:**
- `range` (optional): Country or region in ISO 3166 format. Default is 'cn' (China).

**Returns:**
JSON object containing a list of LocationIDs for cities that have active weather warnings.

**Example:**
```python
warnings = await client.warning_city_list(range="cn")
```

### `weather_warning`

Get real-time weather disaster warnings for a specific location.

**Parameters:**
- `location` (required): LocationID or coordinates (comma-separated longitude,latitude). LocationID can be obtained via the GeoAPI.
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing weather warning data including warning ID, issuing organization, publication time, warning title and text, time range, severity level, and warning type.

**Example:**
```python
warnings = await client.weather_warning(location="101010100")
```

## Weather Indices Tools

### `weather_indices`

Get weather lifestyle indices forecast data for cities.

**Parameters:**
- `location` (required): LocationID or coordinates (comma-separated longitude,latitude). LocationID can be obtained via the GeoAPI.
- `type` (required): Weather index type IDs, can be a single ID or multiple IDs separated by commas.
- `days` (optional): Number of days to forecast, valid values are 1 or 3. Default is 1.
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing weather indices data including index date, type, name, level, category, and detailed description.

**Example:**
```python
indices = await client.weather_indices(location="101010100", type="1,2,3", days=1)
```

#### Available Index Types:
1. Sport
2. Car Wash
3. Clothing
4. Cold/Flu
5. Tourism
6. UV Index
7. Air Pollution
8. AC/Heating
9. Allergy
10. Sunglasses
11. Makeup
12. Drying
13. Traffic
14. Fishing
15. Sunscreen
And more (refer to QWeather API documentation for the complete list)

## Air Quality Tools

### `air_quality_now`

Get real-time air quality data for a specific location with 1x1 km precision.

**Parameters:**
- `latitude` (required): Latitude of the location. Decimal, supports up to two decimal places.
- `longitude` (required): Longitude of the location. Decimal, supports up to two decimal places.
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing detailed air quality data including AQI, pollutant concentrations, health recommendations, and related station information.

**Example:**
```python
air_quality = await client.air_quality_now(latitude=39.92, longitude=116.41)
```

### `air_quality_forecast`

Get air quality forecast for the next 3 days.

**Parameters:**
- `latitude` (required): Latitude of the location. Decimal, supports up to two decimal places.
- `longitude` (required): Longitude of the location. Decimal, supports up to two decimal places.
- `lang` (optional): Language setting for the response.

**Returns:**
JSON object containing air quality forecast data for the next 3 days including AQI, pollutant concentrations, and health recommendations.

**Example:**
```python
forecast = await client.air_quality_forecast(latitude=39.92, longitude=116.41)
```

## Response Format

All API responses follow a standard format:

```json
{
  "code": "200",             // Response code, "200" means success
  "updateTime": "2023-05-15T14:35+08:00",  // Data update time
  "fxLink": "http://hfx.link/...",         // Link to QWeather website
  // Specific data for each API...
}
```

## Error Codes

Common error codes returned in the response:
- `200`: Success
- `204`: No content returned
- `400`: Bad request parameters
- `401`: Unauthorized (API key issue)
- `402`: Too many requests
- `403`: Permission denied
- `404`: Not found
- `429`: Too many requests (rate limit exceeded)
- `500`: Server error

## Language Support

Set the `lang` parameter to change the language of the response:
- `zh` - Chinese (Simplified)
- `zh-hk` - Chinese (Traditional)
- `en` - English
- `ja` - Japanese
- `ko` - Korean
- `es` - Spanish
- `fr` - French
- And more (refer to QWeather API documentation) 