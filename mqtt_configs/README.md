# MQTT Energy Monitoring Configurations

This directory contains complete MQTT sensor configurations for Home Assistant integration with Rolectro R12 energy monitoring devices.

## Quick Start

1. **Publish configurations to MQTT broker:**
   Each JSON file in the `sensor/`, `binary_sensor/`, and `switch/` directories should be published to its corresponding MQTT config topic.

2. **Configuration topic pattern:**
   ```
   homeassistant/{type}/{device_id}_{sensor_id}/config
   ```

3. **Example for Qt sensor:**
   - File: `sensor/31212256C0630_qt.json`
   - Topic: `homeassistant/sensor/31212256C0630_qt/config`
   - Payload: Contents of the JSON file

4. **Data publishing:**
   The device publishes all sensor data in a single JSON payload to:
   ```
   homeassistant/sensor/31212256C0630
   ```

## Directory Structure

```
mqtt_configs/
├── README.md                # This file
├── SENSORS_LIST.md         # Complete list of all sensors
├── sensor/                 # 44 sensor configurations
│   ├── 31212256C0630_*.json
├── binary_sensor/          # 1 binary sensor configuration
│   └── 31212256C0630_input.json
├── switch/                 # 1 switch configuration
│   └── 31212256C0630_relay.json
└── examples/               # Example MQTT payloads
    ├── 31212256C0630_data.json
    ├── MQTT_RT_DATA.json
    └── MQTT_TELEIND.json
```

## Measured Parameters

The Rolectro R12 device monitors comprehensive three-phase electrical parameters:

- **Voltage:** Phase voltages (Ua, Ub, Uc) and line voltages (Uab, Ubc, Uca)
- **Current:** Phase currents (Ia, Ib, Ic)
- **Active Power:** Per-phase and total (Pa, Pb, Pc, Pt, Pdm)
- **Reactive Power:** Per-phase and total (Qa, Qb, Qc, Qt, Qdm)
- **Apparent Power:** Per-phase and total (Sa, Sb, Sc, St, Sdm)
- **Energy:** Active and reactive, import and export
- **Power Quality:** Power factor, THD for voltage and current
- **Other:** Frequency, temperature, timestamps

## Configuration Features

All sensor configurations include:
- ✅ Proper device classes (VOLTAGE, CURRENT, POWER, ENERGY, etc.)
- ✅ State classes (MEASUREMENT, TOTAL_INCREASING)
- ✅ Units of measurement (V, A, kW, kvar, VA, kWh, Hz, °C, %)
- ✅ Material Design Icons (mdi:*)
- ✅ Device information (identifiers, name, model, manufacturer)
- ✅ Unique IDs for entity management

## Home Assistant Integration

Once published, the sensors will:
1. Auto-discover in Home Assistant via MQTT Discovery
2. Appear grouped under device "31212256C0630" (Rolectro R12)
3. Update automatically when the device publishes new data
4. Support energy dashboard integration for energy sensors
5. Provide historical data and statistics

## Example Data Payload

See `examples/31212256C0630_data.json` for a complete example of the JSON payload structure that the device publishes.

## Support

For detailed sensor list and descriptions, see [SENSORS_LIST.md](SENSORS_LIST.md)
