---
title: "Building Your First MCP Server"
date: 2025-05-20T11:00:00-07:00
draft: false
tags: ["MCP", "Tutorial", "Development"]
categories: ["Tutorials"]
---

# Building Your First MCP Server

In this tutorial, we'll walk through creating a simple MCP server that exposes a weather forecasting tool to Claude or any other MCP-compatible AI assistant. By the end, you'll have a functional MCP server that demonstrates the power of the Model Context Protocol.

## Prerequisites

Before we begin, make sure you have:

- Python 3.10 or higher installed
- Basic knowledge of Python programming
- A Claude Desktop or other MCP client to test your server

## Setting Up Your Environment

First, let's create a new project directory and set up a virtual environment:

```bash
# Create a new directory
mkdir weather-mcp-server
cd weather-mcp-server

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows, use: venv\Scripts\activate
```

Next, install the MCP Python SDK:

```bash
pip install mcp[cli]
```

## Creating Our Weather Tool

Let's create a simple weather tool that retrieves weather information for a specified location. We'll use a free weather API for this purpose.

Create a new file called `weather_server.py` and add the following code:

```python
import json
import requests
from typing import Annotated, Optional
from pydantic import BaseModel, Field

from mcp import JSONSchema, Response, Server, Tool, router

# Weather API request model
class WeatherRequest(BaseModel):
    location: Annotated[
        str,
        Field(
            description="The city and country for the weather forecast, e.g., 'London,UK'"
        ),
    ]
    units: Annotated[
        Optional[str],
        Field(
            description="Temperature units: 'metric' (Celsius) or 'imperial' (Fahrenheit)",
            default="metric",
        ),
    ] = "metric"

# Weather tool implementation
@router.tool(
    "weather",
    "Get the current weather for a location",
    request_schema=JSONSchema.from_type(WeatherRequest),
)
async def weather(request: WeatherRequest) -> Response:
    # Replace with your actual API key if needed
    API_KEY = "demo_key"  # Use a real API key in production
    BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
    
    params = {
        "q": request.location,
        "units": request.units,
        "appid": API_KEY,
    }
    
    try:
        response = requests.get(BASE_URL, params=params)
        response.raise_for_status()
        data = response.json()
        
        if request.units == "metric":
            temp_unit = "?C"
        else:
            temp_unit = "?F"
        
        weather_info = {
            "location": f"{data['name']}, {data['sys']['country']}",
            "temperature": f"{data['main']['temp']}{temp_unit}",
            "condition": data['weather'][0]['description'],
            "humidity": f"{data['main']['humidity']}%",
            "wind": f"{data['wind']['speed']} m/s",
        }
        
        return Response(weather_info)
    except Exception as e:
        return Response({"error": str(e)}, status_code=500)

# Create and run the server
if __name__ == "__main__":
    server = Server(tools=[weather])
    server.run(host="127.0.0.1", port=8000)
```

## Running Your MCP Server

To run your MCP server, execute the following command:

```bash
python weather_server.py
```

This will start your MCP server on localhost port 8000.

## Connecting to Claude Desktop

Now that your server is running, you can connect it to Claude Desktop:

1. Open Claude Desktop
2. Go to Settings > MCP Servers
3. Click "Add Server"
4. Enter `http://localhost:8000` as the server URL
5. Click "Connect"

## Testing Your Weather Tool

Once connected, you can ask Claude to use your weather tool:

> "Can you tell me the current weather in San Francisco?"

Claude will use your MCP server to fetch real-time weather information and respond accordingly.

## Next Steps

This is just the beginning of what you can do with MCP. You could extend this server to:

- Add more weather-related tools (forecasts, historical data, etc.)
- Implement caching for improved performance
- Add authentication for secure access
- Deploy the server publicly for broader use

In our next tutorial, we'll explore more advanced MCP features, including resources and prompts!
