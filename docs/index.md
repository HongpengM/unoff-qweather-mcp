# QWeather MCP Documentation

Welcome to the documentation for the unofficial QWeather MCP package. This package provides tools to access QWeather's comprehensive weather data services through the Multiagent Conversation Protocol (MCP).

## Documentation Contents

1. [Getting Started](../README.md) - Installation and basic setup
2. [Usage Guide](usage_guide.md) - Detailed examples of how to use the package
3. [API Reference](api_reference.md) - Comprehensive reference of all available tools
4. [Publishing Guide](publishing_guide.md) - How to publish on GitHub and MCP.so

## What is QWeather?

[QWeather](https://www.qweather.com/) is a professional weather data service provider offering global weather data through APIs. Their service includes:

- Real-time weather conditions
- Weather forecasts (hourly and daily)
- Weather warnings and alerts
- Air quality data
- Weather indices for lifestyle planning
- And more

## What is MCP?

[MCP (Multiagent Conversation Protocol)](https://mcp.so) is a protocol for connecting AI models and agents to external tools, APIs, and data sources. It enables:

- Building modular tools for AI agents
- Sharing tools across different AI systems
- Creating a ecosystem of specialized tools

This package combines QWeather's data services with MCP's tool protocol to make weather data easily accessible to AI agents and applications.

## Quick Start

```bash
# Install the package
pip install unoff-qweather-mcp

# Set up environment variables
export QWEATHER_API_KEY="your_api_key_here"
export QWEATHER_API_HOST="api.qweather.com"

# Run the MCP server
python -m unoff_qweather.qweather
```

Then in your client code:

```python
from mcp.client import Client

client = Client("unoff-qweather-mcp")

# Get weather for a city
cities = await client.lookup_city(location="London")
location_id = cities["location"][0]["id"]
weather = await client.weather_now(location=location_id)

print(f"Current weather in London: {weather['now']['text']}, {weather['now']['temp']}°C")
```

## Requirements

- Python 3.12 or higher
- A QWeather API key (register at [QWeather Developer Platform](https://dev.qweather.com/))
- For MCP.so deployment: an MCP.so account

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.

## Disclaimer

This is an unofficial wrapper and is not affiliated with or endorsed by QWeather. Users must comply with QWeather's terms of service when using this package. 