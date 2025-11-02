# Example Usage: Publishing MQTT Messages for Rolectro R12

This document provides examples of how to publish and consume MQTT messages for the Rolectro R12 energy monitor.

## Example MQTT Sensor Data Payload

The device publishes JSON data to the topic `homeassistant/sensor/31212256C0629`:

```json
{
  "id": "31212256C0629",
  "ua": 230.135,
  "ub": 230.081,
  "uc": 230.206,
  "ia": 4.083,
  "ib": 4.085,
  "ic": 4.084,
  "uab": 398.5,
  "ubc": 398.3,
  "uca": 398.4,
  "pa": 0.541,
  "pb": 0.5455,
  "pc": 0.5432,
  "pt": 1.6298,
  "qa": -0.763,
  "qb": -0.7627,
  "qc": -0.7635,
  "qt": -2.2893,
  "sa": 935.7785,
  "sb": 938.2056,
  "sc": 937.4752,
  "st": 2811.4597,
  "freq": 50.001,
  "pfa": 0.571,
  "pfb": 0.575,
  "pfc": 0.575,
  "pft": 0.574,
  "pdm": 1.5061,
  "qdm": -2.3902,
  "sdm": 2830.6958,
  "uathd": 2.2,
  "ubthd": 2.2,
  "ucthd": 2.2,
  "iathd": 27.899,
  "ibthd": 28.0,
  "icthd": 27.799,
  "temp": 18.573,
  "iactive": 884.758,
  "eactive": 688.413,
  "ireact": 0.031,
  "ereact": 1542.202,
  "pos_en_a": 290.927,
  "pos_en_b": 295.857,
  "pos_en_c": 298.11,
  "time": "2025-11-02T13:06:00+01:00",
  "isend": "1"
}
```

## Publishing Test Data with mosquitto_pub

You can test the configuration by publishing sample data using `mosquitto_pub`:

```bash
# Publish sensor data
mosquitto_pub -h localhost -t "homeassistant/sensor/31212256C0629" -m '{
  "id":"31212256C0629",
  "ua":230.135,
  "ub":230.081,
  "uc":230.206,
  "ia":4.083,
  "ib":4.085,
  "ic":4.084,
  "pa":0.541,
  "pb":0.5455,
  "pc":0.5432,
  "pt":1.6298,
  "freq":50.001,
  "temp":18.573,
  "iactive":884.758,
  "eactive":688.413,
  "time":"2025-11-02T13:06:00+01:00"
}'

# Publish binary sensor state
mosquitto_pub -h localhost -t "homeassistant/binary_sensor/31212256C0629_input/state" -m "ON"

# Publish switch state
mosquitto_pub -h localhost -t "homeassistant/switch/31212256C0629_relay/state" -m "OFF"
```

## Controlling the Relay Switch

To control the relay switch from Home Assistant, publish to the command topic:

```bash
# Turn relay ON
mosquitto_pub -h localhost -t "homeassistant/switch/31212256C0629_relay/set" -m "ON"

# Turn relay OFF
mosquitto_pub -h localhost -t "homeassistant/switch/31212256C0629_relay/set" -m "OFF"
```

## Subscribing to Topics for Testing

Monitor all messages:

```bash
# Subscribe to all sensor data
mosquitto_sub -h localhost -t "homeassistant/sensor/31212256C0629" -v

# Subscribe to binary sensor state
mosquitto_sub -h localhost -t "homeassistant/binary_sensor/31212256C0629_input/state" -v

# Subscribe to switch state
mosquitto_sub -h localhost -t "homeassistant/switch/31212256C0629_relay/state" -v

# Subscribe to switch commands
mosquitto_sub -h localhost -t "homeassistant/switch/31212256C0629_relay/set" -v
```

## Python Example: Publishing Sensor Data

```python
import paho.mqtt.client as mqtt
import json
from datetime import datetime

# MQTT broker configuration
BROKER = "localhost"
PORT = 1883
TOPIC = "homeassistant/sensor/31212256C0629"

# Sample sensor data
sensor_data = {
    "id": "31212256C0629",
    "ua": 230.135,
    "ub": 230.081,
    "uc": 230.206,
    "ia": 4.083,
    "ib": 4.085,
    "ic": 4.084,
    "pa": 0.541,
    "pb": 0.5455,
    "pc": 0.5432,
    "pt": 1.6298,
    "qa": -0.763,
    "qb": -0.7627,
    "qc": -0.7635,
    "qt": -2.2893,
    "freq": 50.001,
    "temp": 18.573,
    "iactive": 884.758,
    "eactive": 688.413,
    "time": datetime.now().isoformat()
}

# Create MQTT client
client = mqtt.Client()

# Connect to broker
client.connect(BROKER, PORT)

# Publish data
payload = json.dumps(sensor_data)
client.publish(TOPIC, payload)

print(f"Published sensor data to {TOPIC}")

# Disconnect
client.disconnect()
```

## Node-RED Flow Example

For Node-RED users, here's a simple flow to publish sensor data:

```json
[
  {
    "id": "mqtt_out",
    "type": "mqtt out",
    "topic": "homeassistant/sensor/31212256C0629",
    "qos": "0",
    "retain": "false",
    "broker": "mqtt_broker"
  },
  {
    "id": "inject_data",
    "type": "inject",
    "payload": "{\"id\":\"31212256C0629\",\"ua\":230.135,\"ub\":230.081,\"uc\":230.206,\"ia\":4.083,\"ib\":4.085,\"ic\":4.084,\"pt\":1.6298,\"temp\":18.573}",
    "payloadType": "json",
    "repeat": "60",
    "once": false
  },
  {
    "id": "mqtt_broker",
    "type": "mqtt-broker",
    "broker": "localhost",
    "port": "1883"
  }
]
```

## Home Assistant Automation Examples

### Alert on High Power Consumption

```yaml
automation:
  - alias: "High Power Alert"
    trigger:
      - platform: numeric_state
        entity_id: sensor.pt
        above: 10
    action:
      - service: notify.mobile_app
        data:
          message: "Total power consumption exceeds 10 kW"
```

### Turn Off Relay on Over-Temperature

```yaml
automation:
  - alias: "Temperature Protection"
    trigger:
      - platform: numeric_state
        entity_id: sensor.temp
        above: 50
    action:
      - service: switch.turn_off
        target:
          entity_id: switch.relay
      - service: notify.mobile_app
        data:
          message: "Relay turned off due to high temperature"
```

### Energy Dashboard Configuration

Add to your `configuration.yaml` for the Energy Dashboard:

```yaml
energy:
  sources:
    - type: grid
      name: "Grid Import"
      flow_from:
        - entity: sensor.iactive
      flow_to:
        - entity: sensor.eactive
```

## Lovelace Dashboard Card Example

```yaml
type: entities
title: Power Monitor - 31212256C0629
entities:
  - entity: sensor.ua
    name: "Phase A Voltage"
  - entity: sensor.ub
    name: "Phase B Voltage"
  - entity: sensor.uc
    name: "Phase C Voltage"
  - entity: sensor.ia
    name: "Phase A Current"
  - entity: sensor.ib
    name: "Phase B Current"
  - entity: sensor.ic
    name: "Phase C Current"
  - entity: sensor.pt
    name: "Total Power"
  - entity: sensor.freq
    name: "Frequency"
  - entity: sensor.temp
    name: "Temperature"
  - entity: switch.relay
    name: "Relay Control"
```

## Gauge Card for Power Display

```yaml
type: gauge
entity: sensor.pt
name: Total Power
unit: kW
min: 0
max: 20
severity:
  green: 0
  yellow: 10
  red: 15
```

## Energy Distribution Card

```yaml
type: energy-distribution
title: Energy Distribution
entities:
  grid_consumption:
    - entity: sensor.iactive
  grid_production:
    - entity: sensor.eactive
```

## Testing Checklist

- [ ] MQTT broker is running and accessible
- [ ] Home Assistant MQTT integration is configured
- [ ] Sensor configuration is loaded in Home Assistant
- [ ] Test publish sample data to sensor topic
- [ ] Verify sensors appear in Home Assistant
- [ ] Test binary sensor state changes
- [ ] Test switch command and state topics
- [ ] Add sensors to Lovelace dashboard
- [ ] Configure Energy Dashboard (if applicable)
- [ ] Set up automations as needed
