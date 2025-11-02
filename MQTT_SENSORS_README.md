# MQTT Sensor Configuration for Rolectro R12 Energy Monitor

This configuration provides comprehensive Home Assistant integration for the Rolectro R12 energy monitoring device via MQTT.

## Device Information

- **Device ID**: 31212256C0629
- **Model**: R12
- **Manufacturer**: Rolectro
- **Communication**: MQTT

## Overview

The Rolectro R12 is a three-phase energy monitoring device that measures various electrical parameters including:
- Voltage (phase-to-neutral and phase-to-phase)
- Current
- Active, Reactive, and Apparent Power
- Energy (import and export)
- Power Factor
- Frequency
- Total Harmonic Distortion (THD)
- Temperature

## Installation

1. Copy the contents of `mqtt_sensors.yaml` to your Home Assistant configuration
2. Add to your `configuration.yaml`:
   ```yaml
   # Include MQTT sensor configuration
   <<: !include mqtt_sensors.yaml
   ```
   OR merge the mqtt section with your existing MQTT configuration

3. Restart Home Assistant

## MQTT Topics

### Sensor Data Topic
- **Topic**: `homeassistant/sensor/31212256C0629`
- **Format**: JSON
- **Update Frequency**: Varies per sensor data

### Binary Sensor Topic
- **State Topic**: `homeassistant/binary_sensor/31212256C0629_input/state`
- **Payloads**: "ON" / "OFF"

### Switch Topics
- **Command Topic**: `homeassistant/switch/31212256C0629_relay/set`
- **State Topic**: `homeassistant/switch/31212256C0629_relay/state`
- **Payloads**: "ON" / "OFF"

## Sensor List

### Voltage Sensors (Phase-to-Neutral)
- `sensor.ua` - Phase A voltage (V)
- `sensor.ub` - Phase B voltage (V)
- `sensor.uc` - Phase C voltage (V)

### Voltage Sensors (Phase-to-Phase)
- `sensor.uab` - Phase A-B voltage (V)
- `sensor.ubc` - Phase B-C voltage (V)
- `sensor.uca` - Phase C-A voltage (V)

### Current Sensors
- `sensor.ia` - Phase A current (A)
- `sensor.ib` - Phase B current (A)
- `sensor.ic` - Phase C current (A)

### Active Power Sensors
- `sensor.pa` - Phase A active power (kW)
- `sensor.pb` - Phase B active power (kW)
- `sensor.pc` - Phase C active power (kW)
- `sensor.pt` - Total active power (kW)
- `sensor.pdm` - Demand active power (kW)

### Reactive Power Sensors
- `sensor.qa` - Phase A reactive power (kvar)
- `sensor.qb` - Phase B reactive power (kvar)
- `sensor.qc` - Phase C reactive power (kvar)
- `sensor.qt` - Total reactive power (kvar)
- `sensor.qdm` - Demand reactive power (kvar)

### Apparent Power Sensors
- `sensor.sa` - Phase A apparent power (VA)
- `sensor.sb` - Phase B apparent power (VA)
- `sensor.sc` - Phase C apparent power (VA)
- `sensor.st` - Total apparent power (VA)
- `sensor.sdm` - Demand apparent power (VA)

### Energy Sensors
- `sensor.iactive` - Import active energy (kWh)
- `sensor.eactive` - Export active energy (kWh)
- `sensor.ireact` - Import reactive energy (kvarh)
- `sensor.ereact` - Export reactive energy (kvarh)
- `sensor.pos_en_a` - Phase A positive energy (kWh)
- `sensor.pos_en_b` - Phase B positive energy (kWh)
- `sensor.pos_en_c` - Phase C positive energy (kWh)

### Power Factor Sensors
- `sensor.pfa` - Phase A power factor
- `sensor.pfb` - Phase B power factor
- `sensor.pfc` - Phase C power factor
- `sensor.pft` - Total power factor

### Quality Sensors
- `sensor.freq` - Frequency (Hz)
- `sensor.temp` - Temperature (°C)
- `sensor.uathd` - Phase A voltage THD (%)
- `sensor.ubthd` - Phase B voltage THD (%)
- `sensor.ucthd` - Phase C voltage THD (%)
- `sensor.iathd` - Phase A current THD (%)
- `sensor.ibthd` - Phase B current THD (%)
- `sensor.icthd` - Phase C current THD (%)

### System Sensors
- `sensor.last_update` - Last update timestamp

### Binary Sensor
- `binary_sensor.input` - Input state (power detection)

### Switch
- `switch.relay` - Control output relay

## Example JSON Payload

```json
{
  "id": "31212256C0629",
  "pfa": 0.571,
  "pfb": 0.575,
  "pfc": 0.575,
  "pft": 0.574,
  "pdm": 1.5061,
  "qdm": -2.3902,
  "sdm": 2830.6958,
  "uathd": 2.200,
  "ubthd": 2.200,
  "ucthd": 2.200,
  "iathd": 27.899,
  "ibthd": 28.000,
  "icthd": 27.799,
  "temp": 18.573,
  "iactive": 884.758,
  "eactive": 688.413,
  "ireact": 0.031,
  "ereact": 1542.202,
  "pos_en_a": 290.927,
  "pos_en_b": 295.857,
  "pos_en_c": 298.110,
  "time": "2025-11-02T13:06:00+01:00",
  "isend": "1"
}
```

## Usage in Home Assistant

### Energy Dashboard
The energy sensors with `state_class: total_increasing` can be added to the Home Assistant Energy Dashboard:
- Import Energy: `sensor.iactive`
- Export Energy: `sensor.eactive`
- Individual phases: `sensor.pos_en_a`, `sensor.pos_en_b`, `sensor.pos_en_c`

### Power Monitoring
Real-time power monitoring:
- Total Power: `sensor.pt`
- Individual phases: `sensor.pa`, `sensor.pb`, `sensor.pc`

### Automations
Example automation using the relay switch:
```yaml
automation:
  - alias: "Turn off relay when power exceeds threshold"
    trigger:
      - platform: numeric_state
        entity_id: sensor.pt
        above: 10
    action:
      - service: switch.turn_off
        target:
          entity_id: switch.relay
```

## Customization

To customize sensor names or add additional sensors:
1. Edit `mqtt_sensors.yaml`
2. Update the `name` field for desired sensors
3. Add new sensor definitions following the same pattern
4. Restart Home Assistant

## Troubleshooting

### Sensors show "Unavailable"
- Check MQTT broker connection
- Verify device is publishing to correct topics
- Check MQTT username/password if authentication is enabled

### Incorrect Values
- Verify JSON payload structure matches configuration
- Check `value_template` matches actual JSON keys
- Enable MQTT debugging in Home Assistant configuration:
  ```yaml
  logger:
    default: warning
    logs:
      homeassistant.components.mqtt: debug
  ```

### Device not showing in Home Assistant
- Ensure all sensors share the same device identifiers
- Restart Home Assistant after configuration changes
- Check for YAML syntax errors

## Additional Information

For more information about MQTT sensor configuration in Home Assistant, see:
- [MQTT Sensor Documentation](https://www.home-assistant.io/integrations/sensor.mqtt/)
- [MQTT Binary Sensor Documentation](https://www.home-assistant.io/integrations/binary_sensor.mqtt/)
- [MQTT Switch Documentation](https://www.home-assistant.io/integrations/switch.mqtt/)

## Support

For device-specific questions, contact Rolectro support or refer to the R12 device manual.
