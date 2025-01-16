# zabbix-homeassistant
![Zabbix](https://img.shields.io/badge/Zabbix-7.0%2B-blue) ![License](https://img.shields.io/badge/License-GPLv3-blue.svg)

## Requirements
- **Zabbix Server** version 7.0 or higher
- **Home Assistant** with API enabled

## Setup

### 1. Import the Template
- Upload the `.yaml` file to Zabbix by navigating to **Configuration** -> **Templates** and clicking **Import**.

### 2. Adjust Macros
Modify the following macros in the template:
- `{$API.TOKEN}`: API token used for authentication with the Home Assistant API.  
  This token can be generated in the Home Assistant settings under **Profile > Token**.
- `{$HA.URL}`: The URL of the Home Assistant instance, including the protocol (e.g., `http://`) and optional port.  
  Example: `http://192.168.1.100:8123`

### 3. Link the Host
- Link the template to the host that represents the Home Assistant instance.

## Items Included in the Template

| Item                     | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Automations**          | Monitors active automations in Home Assistant. Helps detect faulty or blocked automations. |
| **Battery Status**       | Checks the battery level of battery-powered devices, such as sensors or remotes. Alerts on low battery. |
| **Cameras**              | Monitors the status of cameras integrated with Home Assistant, including optional reachability and video streams. |
| **Contact Sensors**      | Tracks the state of contact sensors (e.g., doors and windows).  |
| **Firmware Updates**     | Checks for available firmware updates for connected devices. |
| **Humidity Sensors**     | Monitors current humidity levels in supervised rooms. |
| **Temperature Sensors**  | Tracks temperature values from Home Assistant.  |
| **Air Pressure** | Monitors the current air pressure readings from Home Assistant sensors. Useful for weather monitoring and trend analysis. |
| **Person Zone Assignment** | Tracks which zone (e.g., Home, Work) a person is currently assigned to, based on GPS location. |
| **Zone Radius**          | Monitors the radius size of configured zones in Home Assistant. Helps ensure accurate geofencing. |
| **Longitude** | Displays the longitude of a configured zone. Useful for precise geolocation settings. |
| **Latitude** | Displays the latitude of a configured zone. Useful for precise geolocation settings. |
| **Home Assistant Updates** | Monitors the Home Assistant version. Sends notifications when updates are available. |
| **Power Consumption**    | Tracks the energy usage of connected devices (e.g., smart plugs). Useful for analyzing power consumption. |
| **Weather Forecast**     | Integrates weather forecast data from Home Assistant, including temperature, rain probability, and wind speed. |
| **Zigbee Link Quality**  | Monitors the signal quality of Zigbee devices. Helps identify connection issues or interference. |
| **Occupancy** | Tracks whether a room or area is currently occupied, using motion or presence sensors. |
| **Illuminance**    | Monitors the light level (lux) in a room or area, useful for automations like adjusting lights. |
| **PM2.5**     | Tracks fine particulate matter (PM2.5) concentration in the air, important for air quality monitoring. |
| **VOC**  | Monitors volatile organic compounds (VOC) levels, helping to ensure good indoor air quality. |


## Notes
- The template is optimized for Zabbix 7.0; older versions may not support all features.



