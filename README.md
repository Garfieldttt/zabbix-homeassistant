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

---

## ⚙️ Macros

| Macro                                | Default Value                                | Description                                                            |
|--------------------------------------|----------------------------------------------|------------------------------------------------------------------------|
| `{$API.TOKEN}`                       | _(secret)_                                   | Home Assistant Long-Lived Access Token                                 |
| `{$HA.URL}`                          | `http://192.168.10.121:8123`                 | Base URL of the Home Assistant API                                     |
| `{$BATTERY.MINIMUM}`                 | `30`                                         | Battery low threshold (%)                                              |
| `{$ZIGBEE.SIGNAL.MINIMUM}`           | `20`                                         | Minimum Zigbee link quality (LQI)                                      |
| `{$DISK.PFREE.MIN.WARN.GIB}`         | `2`                                          | Minimum free disk space (GiB)                                          |
| `{$MEMORY.UTIL.MAX.PERCENT}`         | `95`                                         | Maximum memory utilization (%)                                         |
| `{$CPU.UTIL.MAX.PERCENT}`            | `90`                                         | Maximum CPU utilization (%)                                            |
| `{$STORAGE_PERCENT}`                 | `5`                                          | Storage free percentage threshold                                      |
| `{$SWAP.PFREE.MIN.WARN.PERCENT}`     | `50`                                         | Minimum free swap percentage                                            |
| `{$TRIGGER.TEMPERATURE}`             | `44`                                         | Temperature threshold (°C)                                             |
| `{$SWITCH.TRIGGER.BATTERY}`          | `1`                                          | Enable battery triggers (1 = on, 0 = off)                              |
| `{$SWITCH.TRIGGER.FIRMWARE}`         | `1`                                          | Enable firmware update triggers                                        |
| `{$SWITCH.TRIGGER.HA_UPDATE}`        | `1`                                          | Enable Home Assistant update triggers                                  |
| `{$SWITCH.TRIGGER.RESTART.REQUIRED}` | `1`                                          | Enable restart required triggers                                       |
| `{$SWITCH.TRIGGER.STORAGE}`          | `1`                                          | Enable storage available triggers                                      |
| `{$SWITCH.TRIGGER.TEMPERATURE}`      | `0`                                          | Enable temperature triggers                                            |
| `{$SWITCH.TRIGGER.ZIGBEE}`           | `1`                                          | Enable Zigbee link quality triggers                                    |

---

## 📦 Value Maps

### automation

| Value | Meaning      |
|-------|--------------|
| `1`   | enabled      |
| `0`   | disabled     |
| `2`   | unavailable  |

### battery

| Value  | Meaning      |
|--------|--------------|
| `-1`   | unavailable  |

### camera

| Value  | Meaning      |
|--------|--------------|
| `0`    | unavailable  |
| `1`    | available    |
| `2`    | idle         |

### contact-sensot

| Value | Meaning  |
|-------|----------|
| `0`   | closed   |
| `1`   | open     |

### firmware-update

| Value | Meaning                     |
|-------|-----------------------------|
| `0`   | no update available         |
| `1`   | an update is available      |
| `2`   | firmware version unavailable|

### kWh

| Value | Meaning      |
|-------|--------------|
| `0`   | unavailable  |

### light

| Value | Meaning      |
|-------|--------------|
| `1`   | on           |
| `0`   | off          |
| `2`   | unavailable  |

### network

| Value | Meaning  |
|-------|----------|
| `0`   | unknown  |

### occupancy

| Value | Meaning  |
|-------|----------|
| `1`   | on       |
| `0`   | off      |

### restart required

| Value | Meaning                   |
|-------|---------------------------|
| `1`   | restart required          |
| `0`   | restart not required      |
| `2`   | restart status unknown    |
| `3`   | not reachable             |

### switch

| Value | Meaning      |
|-------|--------------|
| `1`   | on           |
| `0`   | off          |
| `2`   | unavailable  |

### update

| Value | Meaning                  |
|-------|--------------------------|
| `0`   | no update available      |
| `1`   | an update is available   |

### weather

| Value | Meaning        |
|-------|----------------|
| `1`   | clear-night    |
| `2`   | cloudy         |
| `3`   | fog            |
| `4`   | hail           |
| `5`   | lightning      |
| `6`   | lightning-rainy|
| `7`   | partly2        |
| `8`   | pouring        |
| `9`   | rainy          |
| `10`  | snowy          |
| `11`  | snowy-rainy    |
| `12`  | sunny          |
| `13`  | windy          |
| `14`  | windy-variant  |
| `15`  | exception      |

---

## 📋 Items

### Active Items

- **HA-raw**  
  - UUID: `2c4ed5c0c5be4840ac89801834efae58`  
  - Key: `ha.raw`  
  - Type: HTTP_AGENT  
  - URL: `{$HA.URL}/api/states`  
  - Header: `Authorization: Bearer {$API.TOKEN}`  
  - Output Format: JSON  
  - Purpose: Master item fetching all HA states

### Dependent Items (via LLD)

Each discovery rule extracts entities and creates dependent items:

- **HA-automation** (UUID: d6a30debaed74aa09515805c866d8b87)  
  - Filter: `^{automation\.}.*$`  
  - Item Prototype: `ha.autot[{#ENTITY_ID}]` (stats, trends 730d, valuemap=automation)

- **HA-batterie** (UUID: 041bbefc72cb4ffea38a2012e0b79cf2)  
  - Filters: `.*_battery$`, `.*_battery_level$`  
  - Prototype: `ent.bat[{#ENTITY_ID}]` (FLOAT, units=%, valuemap=battery)  
  - Triggers: battery low, status unavailable

- **HA-camera** (UUID: b21d364cbc23433ca3876572fb74b8bb)  
  - Filter: `^camera\..*`  
  - Prototype: `ha.cam.stat[{#ENTITY_ID}]` (trends 730d, valuemap=camera)

- **HA-contact-sensor** (UUID: 1311148760784d948dc433324f030bc9)  
  - Filters: `.*_ajto$`, `.*_contact$`, `.*_door$`  
  - Prototype: `ent.conbtact[{#ENTITY_ID}]` (valuemap=contact-sensot)

- **HA-disk.free** & **HA-disk.used**  
  - Filters: `sensor.systemmonitor_disk_free`, `sensor.systemmonitor_disk_use` variants  
  - Prototypes: `disk.free.mb[{#ENTITY_ID}]` & `disk.used.mb[{#ENTITY_ID}]` (FLOAT, units=GiB)  
  - Trigger Prototype: low disk free

- (…and many more: HA-distance, HA-fine-dust, HA-floors-ascended/descended, HA-weather-forecast, HA-Illuminance, HA-kWh, HA-light, HA-zigbee-linkquality, HA-humidity, HA-memory-free/use, HA-memory-used-mb, HA-occupancy, HA-atmospheric-pressure, HA-processor-use, HA-restart, HA-steps, HA-storage-available, HA-swap-free/use, HA-switch, HA-temperature, HA-update, HA-firmware-update, HA-uptime, HA-VOC, HA-watt, HA-zone…)

---

## 🔔 Triggers

Representative trigger prototypes include:

- **Battery Low**:  
  `last(/Home Assistant - Zabbix/ent.bat[{#ENTITY_ID}]) < {$BATTERY.MINIMUM}`  

- **Temperature Exceeded**:  
  `last(/Home Assistant - Zabbix/ent.i.d[{#ENTITY_ID}]) > {$TRIGGER.TEMPERATURE}`  

- **Firmware Update Available**:  
  `last(/Home Assistant - Zabbix/ent.firmware[{#ENTITY_ID}]) = 1`  

- **HA Update Available**:  
  `last(/Home Assistant - Zabbix/ha.update[{#ENTITY_ID}]) = 1`  

- **Disk Space Low**:  
  `min(/Home Assistant - Zabbix/disk.free.mb[{#ENTITY_ID}],5m) < {$DISK.PFREE.MIN.WARN.GIB}`  

- **Memory Utilization High**:  
  `min(/Home Assistant - Zabbix/memory.used.p[{#ENTITY_ID}],5m) > {$MEMORY.UTIL.MAX.PERCENT}`  

- **CPU Utilization High**:  
  `min(/Home Assistant - Zabbix/cpu.utilization[{#ENTITY_ID}],5m) > {$CPU.UTIL.MAX.PERCENT}`  

- **Zigbee Signal Low**:  
  `min(/Home Assistant - Zabbix/ent.linkquality[{#ENTITY_ID}],5m) < {$ZIGBEE.SIGNAL.MINIMUM}`  

(…and others within each LLD rule)

---

## 🔧 Customization & Operation

- Adjust macros (`{$HA.URL}`, `{$API.TOKEN}`, thresholds) to your environment  
- Ensure Home Assistant API returns JSON states at `/api/states`  
- Fine‑tune LLD filters for custom entity patterns  
- Extend item and trigger prototypes as needed  
- Monitor performance: HTTP agent interval, preprocessing overhead  

---

## ✅ Conclusion

A comprehensive Zabbix template for Home Assistant, leveraging HTTP Agent, JSON preprocessing, LLD, dependent items, value maps, and trigger prototypes.  
Ideal for unified state tracking of your smart home in Zabbix.
