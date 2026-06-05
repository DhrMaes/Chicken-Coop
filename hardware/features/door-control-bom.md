# Door Control System - BOM

Bill of materials for the automated door control feature.

**Master BOM**: [See master BOM index](../bom.md)  
**Project Status**: Not started  
**Feature Status**: Not ordered

---

## Feature Overview

Automated door opening and closing system for the chicken coop. Includes:
- 12V DC motor with gearbox
- Motor control via ESP32
- Door position detection (magnetic sensor)
- Mechanical drive system

---

## Parts List

### Motor & Actuation

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| DC Motor | 1 | 12V, 100-300 RPM, gearbox | Amazon | $15-25 | ❌ Not ordered | High torque for door |
| Motor Mount | 1 | Aluminum L-bracket | Amazon | $5-8 | ❌ Not ordered | Mechanical attachment |
| Motor Coupling | 1 | Flexible shaft coupler | Amazon | $3-5 | ❌ Not ordered | Connects motor to door |

**Subtotal**: $23-38

### Motor Control

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| Motor Driver | 1 | L298N dual H-bridge | AliExpress | $2-4 | ❌ Not ordered | Controls motor direction/speed |
| Capacitor | 2 | 100µF, 50V | Local | $0.50 | ❌ Not ordered | Noise filtering |
| Resistor | 2 | 10kΩ, 1/4W | Local | $0.25 | ❌ Not ordered | Gate pull-ups |

**Subtotal**: $3-5

### Door Sensor

| Part | Qty | Model/Spec | Supplier | Price | Status | Notes |
|------|-----|-----------|----------|-------|--------|-------|
| Magnetic Switch | 1 | Reed switch, NC/NO | Amazon | $3-6 | ❌ Not ordered | Detects door position |
| Magnet | 1 | 10mm neodymium | Amazon | $1-2 | ❌ Not ordered | Pairs with switch |
| Resistor (Pull-up) | 1 | 10kΩ | Local | $0.25 | ❌ Not ordered | GPIO pull-up |

**Subtotal**: $4-9

### Mechanical Hardware

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| M3 Screws | 10 | 20mm | Local | $1-2 | ❌ Not ordered | Motor mount |
| M4 Screws | 5 | 25mm | Local | $1-2 | ❌ Not ordered | Door hinge |
| Threaded Inserts (M3) | 5 | Heat-set | Amazon | $2-3 | ❌ Not ordered | Repeatable holes |
| Door Hinges | 2 | Printed or metal | Amazon | $3-5 | ❌ Not ordered | Door pivot |

**Subtotal**: $7-12

---

## Total Cost

| Category | Cost |
|----------|------|
| Motor & Actuation | $23-38 |
| Motor Control | $3-5 |
| Door Sensor | $4-9 |
| Mechanical Hardware | $7-12 |
| **FEATURE TOTAL** | **$37-64** |

---

## Order Tracking

Track your door control orders here:

| Part | Qty | Order Date | Supplier | Tracking | Cost | Status | Notes |
|------|-----|-----------|----------|----------|------|--------|-------|
| | | | | | | | |

---

## Assembly Notes

### Motor Wiring

```
ESP32 GPIO12 → L298N Input 1
ESP32 GPIO13 → L298N Input 2 (optional for reverse)
L298N Output → DC Motor terminals
12V Supply → L298N (+12V)
GND → Common ground
```

### Door Sensor Wiring

```
ESP32 GPIO16 → Door Switch
3.3V → Pull-up resistor (10kΩ)
GND → Door Switch GND
```

### Speed Control (Optional)

Add PWM control for variable door speed:

```yaml
# ESPHome config
output:
  - platform: ledc
    pin: GPIO12
    frequency: 1000 Hz
    id: door_motor

switch:
  - platform: output
    name: "Door Motor"
    output: door_motor
```

---

## Testing Checklist

Before full assembly, test:

- [ ] Motor spins in both directions with L298N
- [ ] Door sensor detects magnetic switch
- [ ] Motor draws reasonable current (< 2A)
- [ ] No excessive noise from motor/gearbox
- [ ] Door moves smoothly through full range

---

## Supplier Recommendations

- **Motor**: Amazon (fast, easy returns) or AliExpress (cheaper)
- **Motor Driver**: AliExpress (very cheap, reliable)
- **Mechanical parts**: Local hardware store (no shipping delays)
- **Fasteners**: Local or bulk online (comes cheaper in 100-packs)

---

## Alternatives & Substitutions

### Motor Options

| Option | Cost | Pros | Cons |
|--------|------|------|------|
| 12V Gearbox Motor | $15-25 | Good torque, simple | Slower, needs driver |
| Servo Motor | $20-40 | Precise positioning | Lower torque |
| Linear Actuator | $30-50 | Most force | Complex, expensive |

### Door Sensor Options

| Option | Cost | Pros | Cons |
|--------|------|------|------|
| Magnetic Reed Switch | $3-6 | Simple, reliable | Must be aligned |
| Limit Switch | $5-8 | Durable, physical | Bulky |
| Proximity Sensor | $8-12 | No contact needed | Susceptible to metal |

---

## Assembly Guide

See [hardware-assembly.md](../../docs/hardware-assembly.md#door-control-assembly) for detailed assembly instructions.

---

## Integration with ESPHome

See [esphome-setup.md](../../docs/esphome-setup.md) for firmware configuration examples.

---

**Feature Owner**: [Name]  
**Last Updated**: June 2026  
**Estimated Cost**: $37-64  
**Status**: Planning phase
