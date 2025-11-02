# System Architecture

## Overview

This diagram shows how the Rolectro R12 energy monitor integrates with Home Assistant via MQTT.

```
┌─────────────────────────────────────────────────────────────────┐
│                     Rolectro R12 Device                         │
│                   (Model: R12, ID: 31212256C0629)               │
│                                                                 │
│  • Three-phase voltage/current monitoring                      │
│  • Power calculation (Active, Reactive, Apparent)              │
│  • Energy accumulation (Import/Export)                         │
│  • THD measurement                                             │
│  • Temperature monitoring                                      │
│  • Relay control                                               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Publishes JSON payloads
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       MQTT Broker                               │
│                  (Mosquitto / HiveMQ / etc.)                    │
│                                                                 │
│  Topics:                                                        │
│  • homeassistant/sensor/31212256C0629                          │
│  • homeassistant/binary_sensor/31212256C0629_input/state       │
│  • homeassistant/switch/31212256C0629_relay/state              │
│  • homeassistant/switch/31212256C0629_relay/set                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ MQTT Integration
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Home Assistant                              │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │  MQTT Sensor Configuration (mqtt_sensors.yaml)          │  │
│  │  • 46 Sensors (voltage, current, power, energy, etc.)   │  │
│  │  • 1 Binary Sensor (input state)                        │  │
│  │  • 1 Switch (relay control)                             │  │
│  └─────────────────────────────────────────────────────────┘  │
│                              │                                  │
│  ┌───────────────────────────┼─────────────────────────────┐  │
│  │                           │                             │  │
│  ▼                           ▼                             ▼  │
│  Energy Dashboard      Lovelace UI              Automations   │
│  • Import energy       • Gauges                 • Alerts       │
│  • Export energy       • Entity cards           • Protection   │
│  • Cost tracking       • History graphs         • Control      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Optional Integrations                        │
│                                                                 │
│  • InfluxDB - Long-term data storage                           │
│  • Grafana - Advanced visualization                            │
│  • Node-RED - Complex automation flows                         │
│  • Mobile Apps - Remote monitoring and control                 │
└─────────────────────────────────────────────────────────────────┘
```

## Data Flow

### Sensor Data Publication

```
Rolectro R12 → MQTT Broker → Home Assistant Sensors
                   │
                   └─→ Topic: homeassistant/sensor/31212256C0629
                       Payload: {
                         "id": "31212256C0629",
                         "ua": 230.135,
                         "ia": 4.083,
                         "pt": 1.6298,
                         "temp": 18.573,
                         ...
                       }
```

### Relay Control

```
Home Assistant Switch → MQTT Broker → Rolectro R12
                           │
                           └─→ Topic: .../relay/set
                               Payload: "ON" or "OFF"

Rolectro R12 → MQTT Broker → Home Assistant (state feedback)
                   │
                   └─→ Topic: .../relay/state
                       Payload: "ON" or "OFF"
```

## Sensor Organization

### By Type

```
┌─────────────────────────────────────────────────────────┐
│                    All Sensors                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Electrical Measurements                                │
│  ├─ Voltage (6)        ├─ Current (3)                   │
│  ├─ Active Power (5)   ├─ Reactive Power (5)            │
│  └─ Apparent Power (5)                                  │
│                                                         │
│  Energy Accumulation (7)                                │
│  ├─ Import/Export Active Energy                         │
│  ├─ Import/Export Reactive Energy                       │
│  └─ Per-phase Energy                                    │
│                                                         │
│  Quality Metrics (10)                                   │
│  ├─ Power Factor (4)                                    │
│  ├─ THD - Voltage (3)                                   │
│  ├─ THD - Current (3)                                   │
│  ├─ Frequency (1)                                       │
│  └─ Temperature (1)                                     │
│                                                         │
│  System                                                 │
│  ├─ Last Update (1)                                     │
│  ├─ Binary Sensor - Input (1)                           │
│  └─ Switch - Relay (1)                                  │
└─────────────────────────────────────────────────────────┘
```

### By Phase

```
Phase A          Phase B          Phase C          Total/System
───────          ───────          ───────          ────────────
• Voltage Ua     • Voltage Ub     • Voltage Uc     • Total Power Pt
• Current Ia     • Current Ib     • Current Ic     • Frequency
• Power Pa       • Power Pb       • Power Pc       • Temperature
• Reactive Qa    • Reactive Qb    • Reactive Qc    • Import Energy
• Apparent Sa    • Apparent Sb    • Apparent Sc    • Export Energy
• Power Factor   • Power Factor   • Power Factor   • Relay Control
• Voltage THD    • Voltage THD    • Voltage THD
• Current THD    • Current THD    • Current THD
• Energy         • Energy         • Energy
```

## Integration Points

### Home Assistant Components

1. **MQTT Integration**
   - Connects to MQTT broker
   - Subscribes to sensor topics
   - Publishes commands to device

2. **Sensor Platform**
   - Parses JSON payloads
   - Extracts individual values
   - Provides device classes and state classes

3. **Energy Dashboard**
   - Uses `total_increasing` sensors
   - Tracks import/export
   - Calculates costs

4. **Automation Engine**
   - Triggers on sensor states
   - Controls relay
   - Sends notifications

### Device Information

All sensors share common device information:
```yaml
device:
  identifiers: ["31212256C0629"]
  name: "31212256C0629"
  model: "R12"
  manufacturer: "Rolectro"
```

This ensures all sensors appear under one device in Home Assistant.

## Customization Options

### 1. Template Sensors
Create calculated values:
- Average voltage across phases
- Total THD
- Phase imbalance percentage

### 2. Automation Triggers
- High power consumption
- Voltage imbalance
- Temperature warnings
- Overcurrent protection

### 3. Custom Cards
- Real-time power gauge
- Energy flow diagram
- Phase balance visualization
- Historical graphs

### 4. External Integrations
- InfluxDB for long-term storage
- Grafana for advanced dashboards
- Telegram/Email notifications
- IFTTT/Webhooks for external actions

## File Structure

```
RonaldTromp/
│
├── mqtt_sensors.yaml              ← Main sensor definitions
│   └── Used by: Home Assistant MQTT integration
│
├── mqtt_discovery_config.json     ← MQTT discovery payloads
│   └── Used by: MQTT discovery protocol
│
├── configuration_example.yaml     ← Complete HA config example
│   └── Shows: Integration, automation, templates
│
├── MQTT_SENSORS_README.md         ← Full documentation
│   └── Contains: Setup, configuration, troubleshooting
│
├── USAGE_EXAMPLES.md              ← Practical examples
│   └── Contains: Scripts, automations, dashboards
│
├── QUICK_REFERENCE.md             ← Quick lookup guide
│   └── Contains: Sensor list, topics, common tasks
│
└── README.md                      ← Project overview
    └── Entry point for the repository
```

## Deployment Workflow

```
1. Review Documentation
   └─→ Read README.md and MQTT_SENSORS_README.md

2. Copy Configuration
   └─→ Copy mqtt_sensors.yaml to Home Assistant config

3. Update configuration.yaml
   └─→ Add MQTT broker settings
   └─→ Include mqtt_sensors.yaml

4. Configure MQTT Broker
   └─→ Ensure broker is accessible
   └─→ Configure authentication if needed

5. Connect Device
   └─→ Configure Rolectro R12 MQTT settings
   └─→ Verify device publishes to correct topics

6. Restart Home Assistant
   └─→ Reload configuration
   └─→ Check for errors in logs

7. Verify Sensors
   └─→ Check Developer Tools → States
   └─→ Confirm all sensors are available

8. Configure Dashboard
   └─→ Add sensors to Lovelace
   └─→ Set up Energy Dashboard

9. Create Automations
   └─→ Add alerts and controls
   └─→ Test automation triggers

10. Monitor and Optimize
    └─→ Review sensor data
    └─→ Adjust thresholds and settings
```

## Security Considerations

1. **MQTT Authentication**
   - Use username/password
   - Consider TLS/SSL encryption
   - Limit broker access

2. **Network Security**
   - Use VLANs for IoT devices
   - Firewall rules
   - Regular firmware updates

3. **Home Assistant**
   - Secure with strong password
   - Enable 2FA if available
   - Regular backups

## Performance

- **Update Rate**: Configurable per device (typically 1-60 seconds)
- **MQTT Overhead**: Minimal, JSON payloads ~500 bytes
- **Home Assistant Load**: Low, sensors use event-driven updates
- **Database Impact**: Energy sensors with `total_increasing` are efficient

## Maintenance

- Monitor MQTT broker logs
- Check sensor availability regularly
- Review energy data for anomalies
- Update configurations as needed
- Backup Home Assistant regularly
