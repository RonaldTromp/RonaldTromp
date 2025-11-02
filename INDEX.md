# Rolectro R12 MQTT Sensor Integration - Documentation Index

Welcome! This repository provides a complete Home Assistant MQTT integration for the Rolectro R12 three-phase energy monitor.

## 📚 Documentation Guide

### Getting Started

1. **[README.md](README.md)** - Start here!
   - Project overview
   - Feature summary
   - Quick links to other docs

2. **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Fast lookup
   - Sensor entity IDs
   - MQTT topics
   - Common tasks
   - Troubleshooting checklist

### Configuration Files

3. **[mqtt_sensors.yaml](mqtt_sensors.yaml)** - Main configuration
   - 46 sensor definitions
   - 1 binary sensor
   - 1 switch
   - Ready to use with Home Assistant

4. **[mqtt_discovery_config.json](mqtt_discovery_config.json)** - MQTT Discovery
   - JSON payloads for MQTT discovery protocol
   - Configuration topic definitions
   - Device information

5. **[configuration_example.yaml](configuration_example.yaml)** - Complete example
   - Full Home Assistant configuration
   - MQTT broker setup
   - Template sensors
   - Automations
   - Energy Dashboard integration

### Detailed Documentation

6. **[MQTT_SENSORS_README.md](MQTT_SENSORS_README.md)** - Comprehensive guide
   - Installation instructions
   - Sensor descriptions
   - MQTT topic structure
   - Home Assistant Energy Dashboard setup
   - Troubleshooting

7. **[USAGE_EXAMPLES.md](USAGE_EXAMPLES.md)** - Practical examples
   - Publishing test data with mosquitto_pub
   - Python scripts
   - Node-RED flows
   - Home Assistant automations
   - Lovelace dashboard cards

8. **[ARCHITECTURE.md](ARCHITECTURE.md)** - System design
   - Architecture diagrams
   - Data flow
   - Integration points
   - Security considerations
   - Deployment workflow

## 🎯 Quick Start Path

**Beginner Path:**
```
README.md → MQTT_SENSORS_README.md → configuration_example.yaml → USAGE_EXAMPLES.md
```

**Experienced User Path:**
```
README.md → QUICK_REFERENCE.md → mqtt_sensors.yaml → (Start using)
```

**Developer Path:**
```
README.md → ARCHITECTURE.md → mqtt_discovery_config.json → mqtt_sensors.yaml
```

## 📊 Sensor Categories

### Electrical Measurements (24 sensors)
- **Voltage**: Phase-to-neutral (3) + Phase-to-phase (3)
- **Current**: Per phase (3)
- **Active Power**: Per phase + total + demand (5)
- **Reactive Power**: Per phase + total + demand (5)
- **Apparent Power**: Per phase + total + demand (5)

### Energy Tracking (7 sensors)
- **Active Energy**: Import + Export
- **Reactive Energy**: Import + Export
- **Per-Phase Energy**: A, B, C

### Quality Metrics (10 sensors)
- **Power Factor**: Per phase + total (4)
- **THD Voltage**: Per phase (3)
- **THD Current**: Per phase (3)

### System Sensors (5 items)
- **Frequency**: 1 sensor
- **Temperature**: 1 sensor
- **Last Update**: 1 sensor
- **Input State**: 1 binary sensor
- **Relay Control**: 1 switch

**Total: 46 sensors + 1 binary sensor + 1 switch**

## 🔧 Common Tasks

| Task | Documentation | File |
|------|---------------|------|
| Install sensors | [MQTT_SENSORS_README.md](MQTT_SENSORS_README.md#installation) | mqtt_sensors.yaml |
| Find sensor entity ID | [QUICK_REFERENCE.md](QUICK_REFERENCE.md#sensor-quick-reference) | - |
| Set up Energy Dashboard | [MQTT_SENSORS_README.md](MQTT_SENSORS_README.md#usage-in-home-assistant) | configuration_example.yaml |
| Create automation | [USAGE_EXAMPLES.md](USAGE_EXAMPLES.md#home-assistant-automation-examples) | - |
| Test with MQTT commands | [USAGE_EXAMPLES.md](USAGE_EXAMPLES.md#publishing-test-data-with-mosquitto_pub) | - |
| Troubleshoot issues | [QUICK_REFERENCE.md](QUICK_REFERENCE.md#troubleshooting) | - |
| Understand architecture | [ARCHITECTURE.md](ARCHITECTURE.md) | - |

## 🌟 Features

✅ **Complete three-phase monitoring**
- Voltage, current, power measurements per phase
- Phase-to-phase voltage readings
- Total power calculation

✅ **Energy tracking**
- Import/export active energy
- Import/export reactive energy
- Per-phase energy accumulation
- Compatible with Home Assistant Energy Dashboard

✅ **Power quality monitoring**
- Power factor per phase
- Total Harmonic Distortion (THD) for voltage and current
- Frequency monitoring

✅ **Device monitoring**
- Temperature sensor
- Last update timestamp
- Input state detection

✅ **Control**
- Relay switch with command and state feedback

✅ **Documentation**
- 8 comprehensive documentation files
- Examples for Python, Node-RED, MQTT CLI
- Quick reference guide
- Architecture diagrams

## 📖 File Overview

| File | Type | Size | Purpose |
|------|------|------|---------|
| README.md | Doc | ~1.4 KB | Project overview |
| QUICK_REFERENCE.md | Doc | ~7.5 KB | Quick lookup guide |
| MQTT_SENSORS_README.md | Doc | ~6.3 KB | Full documentation |
| USAGE_EXAMPLES.md | Doc | ~6.7 KB | Practical examples |
| ARCHITECTURE.md | Doc | ~11 KB | System architecture |
| INDEX.md | Doc | This file | Documentation index |
| mqtt_sensors.yaml | Config | ~22 KB | Home Assistant config |
| mqtt_discovery_config.json | Config | ~34 KB | MQTT discovery |
| configuration_example.yaml | Config | ~6 KB | Complete HA example |

## 🎓 Learning Resources

### Home Assistant Documentation
- [MQTT Sensor Integration](https://www.home-assistant.io/integrations/sensor.mqtt/)
- [Energy Dashboard](https://www.home-assistant.io/docs/energy/)
- [MQTT Binary Sensor](https://www.home-assistant.io/integrations/binary_sensor.mqtt/)
- [MQTT Switch](https://www.home-assistant.io/integrations/switch.mqtt/)

### MQTT Resources
- [MQTT.org](https://mqtt.org/)
- [Mosquitto Documentation](https://mosquitto.org/documentation/)
- [MQTT Explorer](http://mqtt-explorer.com/) - GUI tool

### Energy Monitoring
- [Three-Phase Power Systems](https://en.wikipedia.org/wiki/Three-phase_electric_power)
- [Power Factor](https://en.wikipedia.org/wiki/Power_factor)
- [Total Harmonic Distortion](https://en.wikipedia.org/wiki/Total_harmonic_distortion)

## 🤝 Support

If you need help:
1. Check the [QUICK_REFERENCE.md](QUICK_REFERENCE.md#troubleshooting)
2. Review [MQTT_SENSORS_README.md](MQTT_SENSORS_README.md#troubleshooting)
3. Verify your configuration matches [configuration_example.yaml](configuration_example.yaml)
4. Try the examples in [USAGE_EXAMPLES.md](USAGE_EXAMPLES.md)

## 📝 Device Information

- **Manufacturer**: Rolectro
- **Model**: R12
- **Type**: Three-Phase Energy Monitor
- **Device ID**: 31212256C0629
- **Communication**: MQTT over TCP/IP
- **Data Format**: JSON

## 🔐 Security Notes

- Always use authentication for MQTT broker
- Consider TLS/SSL encryption for production
- Place IoT devices on separate VLAN
- Regular firmware updates for devices
- Regular backups of Home Assistant configuration

## 📅 Version History

- **v1.0** - Initial release with comprehensive configuration and documentation

---

**Happy monitoring! ⚡📊**
