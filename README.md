# Zabbix Integration: Home Assistant Monitoring

**Template:** `Home Assistant - Zabbix`  
**Zabbix Version:** 7.0+  

---

## 💡 Overview

Agentless monitoring of Home Assistant entities via the REST API:  
- Fetches all `entity` states from Home Assistant (`{$HA.URL}/api/states`)  
- Discovers entities (automations, sensors, switches, cameras, zones, etc.) via Low-Level Discovery (LLD)  
- Extracts state, values, and attributes (e.g., battery level, temperature, disk usage)  
- Applies JSON preprocessing and value mappings to normalize data  
- Generates dependent items and trigger prototypes for various entity types

---

## 🔍 Requirements

- Home Assistant instance with REST API enabled  
- Long-Lived Access Token stored in macro `{$API.TOKEN}`  
- Accessible URL `{$HA.URL}` (e.g., `http://192.168.10.121:8123`)  
- Template applied to a Zabbix host with network access to HA  

   | Macro                                | Example Value                        | Description                                    |
   |--------------------------------------|--------------------------------------|------------------------------------------------|
   | `{$API.TOKEN}`                       | _(SECRET)_                           | Home Assistant Long-Lived Access Token         |
   | `{$HA.URL}`                          | `http://192.168.1.100:8123`          | Base URL of the Home Assistant API             |
   | `{$BATTERY.MINIMUM}`                 | `30`                                 | Battery level threshold (%)                    |
   | `{$ZIGBEE.SIGNAL.MINIMUM}`           | `20`                                 | Minimum Zigbee link quality (LQI)              |
   | `{$DISK.PFREE.MIN.WARN.GIB}`         | `2`                                  | Minimum free disk space (GiB)                  |
   | `{$MEMORY.UTIL.MAX.PERCENT}`         | `95`                                 | Maximum memory utilization (%)                 |
   | `{$CPU.UTIL.MAX.PERCENT}`            | `90`                                 | Maximum CPU utilization (%)                    |
   | `{$STORAGE_PERCENT}`                 | `5`                                  | Free storage percentage threshold (%)          |
   | `{$SWAP.PFREE.MIN.WARN.PERCENT}`     | `50`                                 | Minimum free swap percentage (%)               |
   | `{$TRIGGER.TEMPERATURE}`             | `44`                                 | Temperature threshold (°C)                     |
   | `{$BACKUP.MAX.AGE.DAYS}`             | `7`                                  | Maximum age of backup (days)                   |
   | `{$POWER.CONSUMPTION.MAX.WATT}`      | `1000`                               | Maximum power consumption (Watt)               |
   | `{$LOAD.AVG.MAX.1M}`                 | `2`                                  | Load average (1m) threshold                    |
   | `{$LOAD.AVG.MAX.5M}`                 | `1.5`                                | Load average (5m) threshold                    |
   | `{$LOAD.AVG.MAX.15M}`                | `1`                                  | Load average (15m) threshold                   |
   | `{$SWITCH.TRIGGER}`        | `1` or `0`                           | Enable/disable trigger for each feature        |

---

## 📑 Items Included in the Template

| Item                                           | Description                                                                                                          |
|------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Automations**                                | Monitors active automations in Home Assistant, detecting faulty or blocked automations.                              |
| **Battery Status**                             | Checks battery level of devices (sensors, remotes), alerts on low battery.                                           |
| **Cameras**                                    | Monitors camera availability and status, including reachability and stream health.                                   |
| **Contact Sensors**                            | Tracks state of doors/windows sensors (open/closed).                                                                 |
| **Firmware Updates**                           | Detects available firmware updates for integrated devices.                                                          |
| **Humidity Sensors**                           | Monitors humidity levels in environments.                                                                            |
| **Additional Humidity Sensors**                | Includes extra humidity sensor types for broader coverage.                                                           |
| **Temperature Sensors**                        | Monitors temperature readings from Home Assistant.                                                                   |
| **Air Pressure**                               | Tracks atmospheric pressure via HA sensors for weather trends.                                                       |
| **Person Zone Assignment**                     | Follows which zone (Home, Work, etc.) a person entity is currently in.                                              |
| **Zone Radius**                                | Monitors configured zone radius to ensure accurate geofencing.                                                       |
| **Longitude**                                  | Displays longitude attribute of zone entities.                                                                      |
| **Latitude**                                   | Displays latitude attribute of zone entities.                                                                       |
| **Home Assistant Updates**                     | Monitors HA core version and alerts when a new release is available.                                                |
| **Power Consumption**                          | Tracks instantaneous power usage (Watt) of devices like smart plugs.                                                |
| **Energy Monitoring**                          | Tracks cumulative energy usage (kWh) for devices.                                                                   |
| **Weather Forecast**                           | Integrates HA weather forecast data (temperature, precipitation probability, wind speed).                          |
| **Zigbee Link Quality**                        | Monitors signal quality (LQI) of Zigbee devices and triggers on low values.                                         |
| **Occupancy**                                  | Detects presence/motion sensor state to indicate room occupancy.                                                    |
| **Illuminance**                                | Monitors light level (lux) for use in automations like adaptive lighting.                                          |
| **PM2.5**                                      | Tracks fine particulate matter concentration for air quality monitoring.                                            |
| **VOC**                                        | Monitors volatile organic compound levels for indoor air quality.                                                  |
| **Light Status**                               | Tracks on/off state of Home Assistant light entities.                                                              |
| **Step Counter (Companion App)**               | Reads step count reported by the HA companion app.                                                                 |
| **Distance Traveled (Companion App)**          | Measures distance covered based on companion app data.                                                             |
| **Floors Descended (Companion App)**           | Tracks floors descended from device sensor data.                                                                    |
| **Floors Ascended (Companion App)**            | Tracks floors ascended from device sensor data.                                                                     |
| **Storage Available (Companion App) (%)**      | Monitors free storage on HA companion devices, triggers below threshold.                                            |
| **Restart Required**                           | Alerts when a restart is required for HA or integrated devices.                                                     |
| **Switch Status**                              | Monitors state (on/off) of switch entities.                                                                         |
| **Device Uptime**                              | Monitors uptime of devices connected to Home Assistant.                                                             |
| **Backup**                                     | Monitors timestamp of last HA backup, alerts if no backup within configured threshold.                              |

---
