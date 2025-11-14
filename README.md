- 👋 Hi, I'm @RonaldTromp
- 👀 I'm interested in energy monitoring 
- 🌱 I'm currently learning MQTT and Home Assistant integration
- 💞️ I'm looking to collaborate on energy monitoring projects
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: I work with three-phase power monitoring!

## MQTT Energy Monitoring Configurations

This repository contains MQTT sensor configurations for Home Assistant integration with Rolectro R12 energy monitoring devices. The configurations enable monitoring of three-phase electrical systems with comprehensive measurements.

### Directory Structure

```
mqtt_configs/
├── sensor/              # Sensor configurations
├── binary_sensor/       # Binary sensor configurations
├── switch/              # Switch configurations
└── examples/           # Example MQTT payload data
```

### Device: Rolectro R12 (ID: 31212256C0630)

The Rolectro R12 is a three-phase energy monitoring device that provides real-time measurements of electrical parameters.

#### Monitored Parameters

**Voltage Measurements:**
- Phase voltages (Ua, Ub, Uc) - Phase-to-neutral voltages
- Line voltages (Uab, Ubc, Uca) - Line-to-line voltages
- Unit: Volts (V)
- Device class: VOLTAGE

**Current Measurements:**
- Phase currents (Ia, Ib, Ic)
- Unit: Amperes (A)
- Device class: CURRENT

**Power Measurements:**
- Active power per phase (Pa, Pb, Pc) and total (Pt)
- Reactive power per phase (Qa, Qb, Qc) and total (Qt)
- Apparent power per phase (Sa, Sb, Sc) and total (St)
- Demand power (Pdm, Qdm, Sdm)
- Units: kW, kvar, VA
- Device classes: POWER, REACTIVE_POWER, APPARENT_POWER

**Energy Counters:**
- Active energy import/export (iActive, eActive)
- Reactive energy import/export (iReact, eReact)
- Per-phase energy (Pos_en_a, Pos_en_b, Pos_en_c)
- Units: kWh, kvarh
- State class: TOTAL_INCREASING

**Power Quality:**
- Power factor per phase (Pfa, Pfb, Pfc) and total (Pft)
- Total harmonic distortion for voltage (UaTHD, UbTHD, UcTHD)
- Total harmonic distortion for current (IaTHD, IbTHD, IcTHD)
- Unit: % for THD

**Other Measurements:**
- Frequency (Freq) - Hz
- Temperature (Temp) - °C
- Last update timestamp

**Control:**
- Binary sensor: Input state
- Switch: Relay control

### MQTT Topics

All sensor data is published to a single topic with JSON payload:
```
homeassistant/sensor/31212256C0630
```

Individual sensor configurations are published to:
```
homeassistant/sensor/31212256C0630_{sensor_id}/config
```

Binary sensor:
```
homeassistant/binary_sensor/31212256C0630_input/state
```

Switch:
```
homeassistant/switch/31212256C0630_relay/set
homeassistant/switch/31212256C0630_relay/state
```

### Example MQTT Payload

```json
{
  "id": "31212256C0630",
  "ua": 230.5, "ub": 231.2, "uc": 229.8,
  "ia": 5.2, "ib": 5.4, "ic": 5.1,
  "pa": 1.2, "pb": 1.25, "pc": 1.18, "pt": 3.63,
  "qa": -0.32, "qb": -0.33, "qc": -0.32, "qt": -0.97,
  "freq": 50.02,
  "temp": 19.261,
  "pfa": 0.726, "pfb": 0.729, "pfc": 0.728, "pft": 0.728,
  "iactive": 1176.935,
  "eactive": 737.859,
  "time": "2025-11-14T20:06:00+01:00"
}
```

### Usage

1. Copy the configuration files to your Home Assistant configuration directory
2. Configure your MQTT broker connection in Home Assistant
3. The device will auto-discover in Home Assistant once configurations are published
4. Sensors will appear under the device "31212256C0630" (Rolectro R12)

### Icons Used

- Voltage: `mdi:flash-triangle-outline` (phase), `mdi:flash-triangle` (line)
- Current: Default
- Power: Default
- Reactive power: `mdi:beer-outline`
- Apparent power: `mdi:beer`
- Power factor: `mdi:angle-acute`
- THD: `mdi:waveform`
- Temperature: `mdi:thermometer`
- Energy import: `mdi:home-import-outline`
- Energy export: `mdi:home-export-outline`

<!---
RonaldTromp/RonaldTromp is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
