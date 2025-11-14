# MQTT Sensor Configurations List

This document lists all available sensor configurations for the Rolectro R12 energy monitoring device (ID: 31212256C0630).

## Sensor Configurations (44 total)

### Voltage Sensors (6)
- `31212256C0630_ua.json` - Phase A voltage (V)
- `31212256C0630_ub.json` - Phase B voltage (V)
- `31212256C0630_uc.json` - Phase C voltage (V)
- `31212256C0630_uab.json` - Line A-B voltage (V)
- `31212256C0630_ubc.json` - Line B-C voltage (V)
- `31212256C0630_uca.json` - Line C-A voltage (V)

### Current Sensors (3)
- `31212256C0630_ia.json` - Phase A current (A)
- `31212256C0630_ib.json` - Phase B current (A)
- `31212256C0630_ic.json` - Phase C current (A)

### Active Power Sensors (5)
- `31212256C0630_pa.json` - Phase A active power (kW)
- `31212256C0630_pb.json` - Phase B active power (kW)
- `31212256C0630_pc.json` - Phase C active power (kW)
- `31212256C0630_pt.json` - Total active power (kW)
- `31212256C0630_pdm.json` - Demand active power (kW)

### Reactive Power Sensors (5)
- `31212256C0630_qa.json` - Phase A reactive power (kvar)
- `31212256C0630_qb.json` - Phase B reactive power (kvar)
- `31212256C0630_qc.json` - Phase C reactive power (kvar)
- `31212256C0630_qt.json` - Total reactive power (kvar)
- `31212256C0630_qdm.json` - Demand reactive power (kvar)

### Apparent Power Sensors (5)
- `31212256C0630_sa.json` - Phase A apparent power (VA)
- `31212256C0630_sb.json` - Phase B apparent power (VA)
- `31212256C0630_sc.json` - Phase C apparent power (VA)
- `31212256C0630_st.json` - Total apparent power (VA)
- `31212256C0630_sdm.json` - Demand apparent power (VA)

### Energy Sensors (7)
- `31212256C0630_iactive.json` - Active energy import (kWh)
- `31212256C0630_eactive.json` - Active energy export (kWh)
- `31212256C0630_ireact.json` - Reactive energy import (kvarh)
- `31212256C0630_ereact.json` - Reactive energy export (kvarh)
- `31212256C0630_pos_en_a.json` - Phase A positive energy (kWh)
- `31212256C0630_pos_en_b.json` - Phase B positive energy (kWh)
- `31212256C0630_pos_en_c.json` - Phase C positive energy (kWh)

### Power Factor Sensors (4)
- `31212256C0630_pfa.json` - Phase A power factor
- `31212256C0630_pfb.json` - Phase B power factor
- `31212256C0630_pfc.json` - Phase C power factor
- `31212256C0630_pft.json` - Total power factor

### Total Harmonic Distortion Sensors (6)
- `31212256C0630_uathd.json` - Phase A voltage THD (%)
- `31212256C0630_ubthd.json` - Phase B voltage THD (%)
- `31212256C0630_ucthd.json` - Phase C voltage THD (%)
- `31212256C0630_iathd.json` - Phase A current THD (%)
- `31212256C0630_ibthd.json` - Phase B current THD (%)
- `31212256C0630_icthd.json` - Phase C current THD (%)

### Other Sensors (3)
- `31212256C0630_freq.json` - Frequency (Hz)
- `31212256C0630_temp.json` - Temperature (°C)
- `31212256C0630_lastupdate.json` - Last update timestamp

## Binary Sensor (1)
- `31212256C0630_input.json` - Input state (ON/OFF)

## Switch (1)
- `31212256C0630_relay.json` - Relay control (ON/OFF)

## Total Count
- **44 Sensors**
- **1 Binary Sensor**
- **1 Switch**
- **46 Total Configurations**

## MQTT Topic Structure

All sensor data is published to:
```
homeassistant/sensor/31212256C0630
```

Configuration topic pattern:
```
homeassistant/sensor/31212256C0630_{sensor_id}/config
```

Example for Qt (reactive power total):
```
homeassistant/sensor/31212256C0630_qt/config
```
