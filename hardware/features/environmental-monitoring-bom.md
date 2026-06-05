# Environmental Monitoring - BOM

Bill of materials for the environmental monitoring system (temperature, humidity, light).

**Master BOM**: [See master BOM index](../bom.md)  
**Project Status**: Not started  
**Feature Status**: Not ordered

---

## Feature Overview

Real-time monitoring of coop environmental conditions:
- Temperature and humidity (DHT22)
- Light level detection (LDR)
- Optional: CO2, air quality sensors (future)

---

## Parts List

### Temperature & Humidity Sensor

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| DHT22 | 1 | Digital temp/humidity | AliExpress | $5-8 | ❌ Not ordered | ±2°C accuracy, outdoor rated |
| Resistor (Pull-up) | 1 | 10kΩ, 1/4W | Local | $0.25 | ❌ Not ordered | DHT22 data line |
| Capacitor | 1 | 100nF | Local | $0.10 | ❌ Not ordered | Data line filtering |

**Subtotal**: $5-9

### Light Sensor

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| LDR (Light Sensor) | 1 | GL5528 or similar | AliExpress | $1-2 | ❌ Not ordered | Detects daylight |
| Resistor | 1 | 10kΩ, 1/4W | Local | $0.25 | ❌ Not ordered | Voltage divider with LDR |

**Subtotal**: $1-3

### Housing & Protection

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Sensor Housing | 1 | 3D printed case | DIY | $0-2 | ❌ Not started | Design in hardware/models/ |
| Desiccant Packets | 1 | Silica gel | Amazon | $2-5 | ❌ Not ordered | Humidity control in enclosure |
| Waterproof Tubing | 0.5m | 4-6mm diameter | Local | $2-3 | ❌ Not ordered | Sensor ventilation |

**Subtotal**: $4-10

### Optional: Additional Sensors

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| CO2 Sensor | - | MH-Z19B (optional) | AliExpress | $15-20 | ❌ Optional | Future air quality monitoring |
| Air Quality | - | BME680 (optional) | Amazon | $15-20 | ❌ Optional | Pressure, altitude, air quality |

**Subtotal (optional)**: $0-40

---

## Total Cost

| Category | Cost |
|----------|------|
| Temperature & Humidity | $5-9 |
| Light Sensor | $1-3 |
| Housing & Protection | $4-10 |
| Optional Sensors | $0 (can add later) |
| **FEATURE TOTAL** | **$10-22** |

---

## Order Tracking

Track your environmental monitoring orders here:

| Part | Qty | Order Date | Supplier | Tracking | Cost | Status | Notes |
|------|-----|-----------|----------|----------|------|--------|-------|
| | | | | | | | |

---

## Wiring Diagram

### DHT22 Connection

```
DHT22 Pin 1 (VCC) → 3.3V
DHT22 Pin 2 (Data) → ESP32 GPIO17
DHT22 Pin 3 (NC) → Not connected
DHT22 Pin 4 (GND) → GND

Pull-up resistor (10kΩ): VCC → Data line
```

### LDR Connection

```
LDR Pin 1 → 3.3V
LDR Pin 2 → ESP32 GPIO32 (ADC input)

Resistor (10kΩ): GPIO32 → GND
(Forms voltage divider)
```

---

## Sensor Specifications

### DHT22

- **Temperature Range**: -40°C to 80°C
- **Humidity Range**: 0-100% RH
- **Accuracy**: ±0.5°C, ±2% RH
- **Resolution**: 0.1°C, 0.1% RH
- **Update Frequency**: 2 seconds minimum
- **Outdoor Rating**: IP65 available

### LDR (GL5528)

- **Light Range**: 1-100,000 lux
- **Response Time**: ~30ms
- **Peak Sensitivity**: ~540nm (green light)
- **Max Power**: 100mW

---

## Assembly Notes

### DHT22 Placement

- Mount in shaded area (avoid direct sunlight)
- Ensure good air circulation for accurate readings
- Keep away from motor/heat sources
- Ideal height: center of coop

### LDR Placement

- Mount on roof or highest point
- Should receive natural daylight
- Avoid shadows from coop structure
- Can be waterproofed with clear epoxy

### Testing Before Assembly

```yaml
# ESPHome config for testing
sensor:
  - platform: dht
    pin: GPIO17
    temperature:
      name: "Test Temperature"
    humidity:
      name: "Test Humidity"
    update_interval: 10s  # Fast for testing

  - platform: adc
    pin: GPIO32
    name: "Test Light Level"
    attenuation: 11db
    update_interval: 10s
```

Monitor values in Home Assistant UI or ESPHome logs. Adjust gain if needed.

---

## Calibration

### DHT22 Calibration

DHT22 is factory calibrated. If readings seem off:

```yaml
sensor:
  - platform: dht
    pin: GPIO17
    temperature:
      name: "Temperature"
      filters:
        - offset: -2.0  # Add/subtract offset if needed
    humidity:
      name: "Humidity"
```

### LDR Calibration

LDR values depend on resistor choice and lighting. Create templates in Home Assistant:

```yaml
template:
  - sensor:
      - name: "Light Level %"
        unique_id: light_level_percent
        unit_of_measurement: "%"
        state: >
          {% set adc = states('sensor.test_light_level') | float(0) %}
          {{ ((adc / 4095) * 100) | round(1) }}
```

---

## Testing Checklist

Before full assembly, test:

- [ ] DHT22 reads temperature and humidity
- [ ] DHT22 values respond to temperature changes
- [ ] LDR reads different values in light/dark
- [ ] Sensors stable (not flickering)
- [ ] Housing keeps sensors dry
- [ ] No interference from other components

---

## Monitoring in Home Assistant

Once integrated, you'll see:

```
Entities:
  sensor.chicken_coop_temperature
  sensor.chicken_coop_humidity
  sensor.light_level
```

Create automations like:

```yaml
automation:
  - alias: "Temperature Alert"
    trigger:
      platform: numeric_state
      entity_id: sensor.chicken_coop_temperature
      below: 5
    action:
      service: notify.mobile_app_user
      data:
        message: "Coop temperature too low: {{ states('sensor.chicken_coop_temperature') }}°C"
```

---

## Supplier Recommendations

- **DHT22**: AliExpress (cheaper, 2-3 week delivery) or Adafruit (local availability)
- **LDR**: Local electronics store or AliExpress
- **Housing**: Print yourself using models in `hardware/models/`

---

## Alternatives & Substitutions

| Component | Alternative | Cost | Pros | Cons |
|-----------|-------------|------|------|------|
| DHT22 | DHT11 | $2-3 | Cheaper | Less accurate (±5°C) |
| DHT22 | BME680 | $15-20 | More features | Overkill, complex |
| LDR | BH1750 | $3-5 | I2C, more accurate | More code complexity |

---

## Integration with ESPHome

See [esphome-setup.md](../../docs/esphome-setup.md#sensor-configuration) for configuration examples.

---

**Feature Owner**: [Name]  
**Last Updated**: June 2026  
**Estimated Cost**: $10-22  
**Status**: Planning phase
