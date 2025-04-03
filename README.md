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
- Create a new host and link the template to it, or link the template to an existing host that represents the Home Assistant instance.  

## Items Included in the Template

| Item                     | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Automations**          | Monitors active automations in Home Assistant. Helps detect faulty or blocked automations. |
| **Battery Status**       | Checks the battery level of battery-powered devices, such as sensors or remotes. Alerts on low battery. |
| **Cameras**              | Monitors the status of cameras integrated with Home Assistant, including optional reachability and video streams. |
| **Contact Sensors**      | Tracks the state of contact sensors (e.g., doors and windows).  |
| **Firmware Updates**     | Checks for available firmware updates for connected devices. |
| **Humidity Sensors**     | Monitors current humidity levels in supervised rooms. |
| **Additional Humidity Sensors** | Detects and includes more types of humidity sensors for improved coverage. |
| **Temperature Sensors**  | Tracks temperature values from Home Assistant.  |
| **Air Pressure**         | Monitors the current air pressure readings from Home Assistant sensors. Useful for weather monitoring and trend analysis. |
| **Person Zone Assignment** | Tracks which zone (e.g., Home, Work) a person is currently assigned to, based on GPS location. |
| **Zone Radius**          | Monitors the radius size of configured zones in Home Assistant. Helps ensure accurate geofencing. |
| **Longitude**            | Displays the longitude of a configured zone. Useful for precise geolocation settings. |
| **Latitude**             | Displays the latitude of a configured zone. Useful for precise geolocation settings. |
| **Home Assistant Updates** | Monitors the Home Assistant version. Sends notifications when updates are available. |
| **Power Consumption**    | Tracks the energy usage (Watt) of connected devices (e.g., smart plugs). Useful for analyzing power consumption. |
| **Energy Monitoring**    | Tracks energy consumption (kWh) of connected devices (e.g., PC, smart plugs). |
| **Weather Forecast**     | Integrates weather forecast data from Home Assistant, including temperature, rain probability, and wind speed. |
| **Zigbee Link Quality**  | Monitors the signal quality of Zigbee devices. Helps identify connection issues or interference. |
| **Occupancy**            | Tracks whether a room or area is currently occupied, using motion or presence sensors. |
| **Illuminance**          | Monitors the light level (lux) in a room or area, useful for automations like adjusting lights. |
| **PM2.5**                | Tracks fine particulate matter (PM2.5) concentration in the air, important for air quality monitoring. |
| **VOC**                  | Monitors volatile organic compounds (VOC) levels, helping to ensure good indoor air quality. |
| **Light Status**         | Monitors the current status of lights (on/off). Useful for automation and monitoring energy usage. |
| **Step Counter (Companion App)** | Tracks the step count from the Home Assistant Companion app. |
| **Distance Traveled (Companion App)** | Measures the total distance traveled based on the Home Assistant Companion app data. |
| **Floors Descended (Companion App)** | Tracks the number of floors descended, based on device motion data. |
| **Floors Ascended (Companion App)** | Tracks the number of floors ascended, based on device motion data. |
| **Storage Available (Companion App) (%) & Trigger (≤5%)** | Monitors the available storage on the Home Assistant device and triggers an alert or automation when available storage drops below 5%. |
| **Restart Required**     | Monitors if a restart is required; triggers an alert if a restart is required. |
| **Switch Status**        | Monitors the state (on/off) of Home Assistant switches. |
| **Device Uptime**        | Monitors the uptime of devices integrated with Home Assistant. |




## Notes
- The template is optimized for Zabbix 7.0; older versions may not support all features.



