# Rolectro R12 MQTT Integration - Quick Reference

## Quick Start

1. **Install Files**
   ```bash
   # Copy configuration files to your Home Assistant config directory
   cp mqtt_sensors.yaml /config/
   ```

2. **Update configuration.yaml**
   ```yaml
   mqtt:
     broker: YOUR_MQTT_BROKER
     sensor: !include mqtt_sensors.yaml
   ```

3. **Restart Home Assistant**

4. **Verify Sensors**
   - Go to Developer Tools → States
   - Search for "31212256C0629"

## Sensor Quick Reference

### Voltage Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.ua` | Phase A Voltage | V | voltage |
| `sensor.ub` | Phase B Voltage | V | voltage |
| `sensor.uc` | Phase C Voltage | V | voltage |
| `sensor.uab` | Phase A-B Voltage | V | voltage |
| `sensor.ubc` | Phase B-C Voltage | V | voltage |
| `sensor.uca` | Phase C-A Voltage | V | voltage |

### Current Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.ia` | Phase A Current | A | current |
| `sensor.ib` | Phase B Current | A | current |
| `sensor.ic` | Phase C Current | A | current |

### Power Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.pa` | Phase A Active Power | kW | power |
| `sensor.pb` | Phase B Active Power | kW | power |
| `sensor.pc` | Phase C Active Power | kW | power |
| `sensor.pt` | Total Active Power | kW | power |
| `sensor.pdm` | Demand Active Power | kW | power |

### Reactive Power Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.qa` | Phase A Reactive Power | kvar | reactive_power |
| `sensor.qb` | Phase B Reactive Power | kvar | reactive_power |
| `sensor.qc` | Phase C Reactive Power | kvar | reactive_power |
| `sensor.qt` | Total Reactive Power | kvar | reactive_power |
| `sensor.qdm` | Demand Reactive Power | kvar | reactive_power |

### Apparent Power Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.sa` | Phase A Apparent Power | VA | apparent_power |
| `sensor.sb` | Phase B Apparent Power | VA | apparent_power |
| `sensor.sc` | Phase C Apparent Power | VA | apparent_power |
| `sensor.st` | Total Apparent Power | VA | apparent_power |
| `sensor.sdm` | Demand Apparent Power | VA | apparent_power |

### Energy Sensors (for Energy Dashboard)
| Entity ID | Description | Unit | Device Class | State Class |
|-----------|-------------|------|--------------|-------------|
| `sensor.iactive` | Import Active Energy | kWh | energy | total_increasing |
| `sensor.eactive` | Export Active Energy | kWh | energy | total_increasing |
| `sensor.ireact` | Import Reactive Energy | kvarh | - | total_increasing |
| `sensor.ereact` | Export Reactive Energy | kvarh | - | total_increasing |
| `sensor.pos_en_a` | Phase A Positive Energy | kWh | energy | total_increasing |
| `sensor.pos_en_b` | Phase B Positive Energy | kWh | energy | total_increasing |
| `sensor.pos_en_c` | Phase C Positive Energy | kWh | energy | total_increasing |

### Power Factor Sensors
| Entity ID | Description | Device Class |
|-----------|-------------|--------------|
| `sensor.pfa` | Phase A Power Factor | power_factor |
| `sensor.pfb` | Phase B Power Factor | power_factor |
| `sensor.pfc` | Phase C Power Factor | power_factor |
| `sensor.pft` | Total Power Factor | power_factor |

### Quality Sensors
| Entity ID | Description | Unit | Device Class |
|-----------|-------------|------|--------------|
| `sensor.freq` | Frequency | Hz | frequency |
| `sensor.temp` | Temperature | °C | temperature |
| `sensor.uathd` | Phase A Voltage THD | % | - |
| `sensor.ubthd` | Phase B Voltage THD | % | - |
| `sensor.ucthd` | Phase C Voltage THD | % | - |
| `sensor.iathd` | Phase A Current THD | % | - |
| `sensor.ibthd` | Phase B Current THD | % | - |
| `sensor.icthd` | Phase C Current THD | % | - |

### System Sensors
| Entity ID | Description | Device Class |
|-----------|-------------|--------------|
| `sensor.last_update` | Last Update Timestamp | timestamp |

### Binary Sensor
| Entity ID | Description | Device Class |
|-----------|-------------|--------------|
| `binary_sensor.input` | Input State | power |

### Switch
| Entity ID | Description |
|-----------|-------------|
| `switch.relay` | Relay Control |

## MQTT Topics

### Sensor Data (All sensors read from this topic)
- **Topic**: `homeassistant/sensor/31212256C0629`
- **Format**: JSON
- **Example**:
  ```json
  {
    "id": "31212256C0629",
    "ua": 230.135,
    "ia": 4.083,
    "pt": 1.6298,
    "temp": 18.573,
    "iactive": 884.758,
    "time": "2025-11-02T13:06:00+01:00"
  }
  ```

### Binary Sensor
- **State Topic**: `homeassistant/binary_sensor/31212256C0629_input/state`
- **Payloads**: `ON` / `OFF`

### Switch
- **Command Topic**: `homeassistant/switch/31212256C0629_relay/set`
- **State Topic**: `homeassistant/switch/31212256C0629_relay/state`
- **Payloads**: `ON` / `OFF`

## Common Tasks

### View Sensor in Dashboard
```yaml
type: entities
entities:
  - sensor.pt
  - sensor.temp
  - sensor.freq
```

### Add to Energy Dashboard
1. Go to Configuration → Energy
2. Click "Add Consumption"
3. Select `sensor.iactive`
4. (Optional) Add `sensor.eactive` for return/production

### Create Alert
```yaml
automation:
  - alias: "Power Alert"
    trigger:
      platform: numeric_state
      entity_id: sensor.pt
      above: 10
    action:
      service: notify.mobile_app
      data:
        message: "Power is {{ states('sensor.pt') }} kW"
```

### Control Relay
```yaml
# In your Lovelace dashboard
type: button
entity: switch.relay
name: Relay Control
tap_action:
  action: toggle
```

## Troubleshooting

### Sensors show "Unavailable"
1. Check MQTT broker is running
2. Verify device is publishing to correct topic
3. Check MQTT credentials if authentication is enabled
4. Enable MQTT debug logging:
   ```yaml
   logger:
     logs:
       homeassistant.components.mqtt: debug
   ```

### Wrong Values
1. Check JSON payload structure
2. Verify `value_template` matches JSON keys
3. Test with `mosquitto_sub`:
   ```bash
   mosquitto_sub -h localhost -t "homeassistant/sensor/31212256C0629" -v
   ```

### Device Not Showing
1. Ensure all sensors share same device identifiers
2. Restart Home Assistant
3. Check for YAML syntax errors:
   ```bash
   hass --script check_config
   ```

## Support Resources

- **Documentation**: See [MQTT_SENSORS_README.md](MQTT_SENSORS_README.md)
- **Usage Examples**: See [USAGE_EXAMPLES.md](USAGE_EXAMPLES.md)
- **Configuration Example**: See [configuration_example.yaml](configuration_example.yaml)
- **Home Assistant MQTT Docs**: https://www.home-assistant.io/integrations/sensor.mqtt/
- **MQTT Explorer**: Great tool for debugging MQTT topics

## File Structure

```
RonaldTromp/
├── mqtt_sensors.yaml              # Main sensor configuration
├── mqtt_discovery_config.json     # Discovery payloads
├── MQTT_SENSORS_README.md         # Detailed documentation
├── USAGE_EXAMPLES.md              # Usage examples and scripts
├── configuration_example.yaml     # Home Assistant config example
├── QUICK_REFERENCE.md             # This file
└── README.md                      # Repository overview
```

## Device Information

- **Manufacturer**: Rolectro
- **Model**: R12
- **Type**: Three-Phase Energy Monitor
- **Device ID**: 31212256C0629
- **Communication**: MQTT over TCP/IP

## License

Feel free to use and modify these configurations for your own Home Assistant setup.
