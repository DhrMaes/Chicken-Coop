# Power & Electronics - BOM

Bill of materials for power distribution and electronic components.

**Master BOM**: [See master BOM index](../bom.md)  
**Project Status**: Not started  
**Feature Status**: Not ordered

---

## Feature Overview

Power delivery and control electronics:
- 5V power for ESP32 logic
- 12V power for motor
- Protection and filtering components
- Electrical connectors and wiring

---

## Parts List

### Power Supplies

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| USB Power Supply | 1 | 5V 2A minimum | Amazon | $5-10 | ❌ Not ordered | ESP32 logic power |
| 12V Power Supply | 1 | 12V 2A minimum | Amazon | $10-15 | ❌ Not ordered | Motor power |

**Subtotal**: $15-25

### Filtering & Stability

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Capacitor (470µF) | 1 | 16V+ electrolytic | Local | $1-2 | ❌ Not ordered | ESP32 power stability |
| Capacitor (100µF) | 2 | 50V electrolytic | Local | $0.50 | ❌ Not ordered | Motor noise filtering |
| Ferrite Bead | 5 | 6mm | AliExpress | $1-2 | ❌ Not ordered | EMI filtering on signal lines |

**Subtotal**: $3-5

### Connectors & Wiring

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Jumper Wires | 50 | Breadboard format | AliExpress | $2-3 | ❌ Not ordered | Development only |
| Dupont Connectors | 20 | 2.54mm header kit | AliExpress | $3-5 | ❌ Not ordered | Repeatable connections |
| JST Connectors | 10 | 2-pin and 3-pin | Amazon | $3-5 | ❌ Not ordered | Waterproof connections |
| USB Connector | 1 | IP67 USB-B waterproof | Amazon | $5-8 | ❌ Not ordered | Outdoor power entry |
| Heat Shrink Tubing | 1 | Assorted sizes | Amazon | $3-5 | ❌ Not ordered | Wire insulation |

**Subtotal**: $19-31

### Resistors

| Part | Qty | Value | Specification | Supplier | Price | Status | Notes |
|------|-----|-------|---------------|----------|-------|--------|-------|
| Resistor Pack | 1 | Various 1/4W | 50-piece assortment | AliExpress | $2-3 | ❌ Not ordered | Pull-ups, voltage dividers |
| Resistor (10kΩ) | 10 | 1/4W 5% | Individual | Local | $0.50 | ❌ Not ordered | Pull-ups |

**Subtotal**: $2-4

### Protective Components

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Fuse (5A) | 2 | 5mm holder + fuse | Local | $1-2 | ❌ Not ordered | Motor circuit protection |
| Polyfuse | 1 | PTC resettable | Local | $1-2 | ❌ Not ordered | USB circuit protection |
| Diode (1N4007) | 5 | General purpose | Local | $0.50 | ❌ Not ordered | Back-EMF protection |

**Subtotal**: $2-5

### Development Components

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Breadboard | 1 | 400-hole | AliExpress | $2-3 | ❌ Not ordered | Prototyping only |
| LED (5mm) | 5 | Red, green, blue | AliExpress | $1-2 | ❌ Not ordered | Debug indicators |
| Resistor (220Ω) | 5 | 1/4W | Local | $0.25 | ❌ Not ordered | LED current limiting |

**Subtotal**: $3-6

---

## Total Cost

| Category | Cost |
|----------|------|
| Power Supplies | $15-25 |
| Filtering & Stability | $3-5 |
| Connectors & Wiring | $19-31 |
| Resistors | $2-4 |
| Protective Components | $2-5 |
| Development Components | $3-6 |
| **FEATURE TOTAL** | **$44-76** |

---

## Order Tracking

Track your power/electronics orders here:

| Part | Qty | Order Date | Supplier | Tracking | Cost | Status | Notes |
|------|-----|-----------|----------|----------|------|--------|-------|
| | | | | | | | |

---

## Wiring Diagram

### Power Distribution

```
┌─ 5V USB Supply → [470µF Cap] → ESP32 (VCC, GND)
│
├─ 12V Supply → [100µF Cap] → L298N → Motor
│
└─ Common GND (all components)
```

### Logic Signal Protection

```
ESP32 GPIO → [Ferrite Bead] → Sensor/Motor Driver
                                    ↓
                            [Pull-up Resistor]
                                    ↓
                                   GND
```

---

## Component Specifications

### Capacitors

**Main ESP32 Power (470µF)**
- Voltage: 16V or higher
- Purpose: Smooth voltage spikes
- Placement: Very close to ESP32 VCC

**Motor Filtering (100µF)**
- Voltage: 50V
- Purpose: Filter motor current spikes
- Placement: Near motor driver

### Ferrite Beads

- Impedance: 100-1000Ω at 1MHz
- Use on: DHT data line, motor signal wires
- Purpose: EMI filtering

### Resistors

**Pull-up (10kΩ)**
- Used for: Digital inputs, I2C lines
- Pulls line to VCC when not driven

**LED Limiting (220Ω)**
- Limits current through LED
- For 3.3V: 3-20mA typical

---

## Assembly Notes

### Breadboard Layout (Development)

```
┌────────────────────────┐
│ ESP32 (USB left)       │
├────────────────────────┤
│ L298N Motor Driver     │
├────────────────────────┤
│ Resistors, Capacitors  │
├────────────────────────┤
│ Sensor Connections     │
└────────────────────────┘
```

### Soldered Circuit (Final)

1. Design PCB or build on perfboard
2. Solder components in order of height
3. Use heat shrink for protection
4. Apply conformal coat for weatherproofing

### Wiring Best Practices

- Use color-coded wires (red=VCC, black=GND, others=signal)
- Label all wires with heat shrink
- Use ferrite beads near ESP32
- Keep motor wires away from sensor lines
- Shortest path for power connections

---

## Testing Checklist

Before assembly, test:

- [ ] USB supply outputs stable 5V
- [ ] 12V supply outputs stable 12V
- [ ] No shorts between power rails
- [ ] Capacitors smooth voltage dips
- [ ] Motor draws expected current (< 2A)
- [ ] Signal lines have proper pull-ups
- [ ] No noise/interference on sensor readings

---

## Troubleshooting Guide

### Power Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| ESP32 won't boot | Insufficient power | Use better power supply, add capacitor |
| Voltage sags on motor | Poor power distribution | Add larger capacitor, separate grounds |
| Resets when motor runs | Ground loops | Separate power supplies, common GND point |

### Signal Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Noisy sensor readings | EMI from motor | Add ferrite bead, separate wires |
| GPIO doesn't respond | No pull-up resistor | Add 10kΩ pull-up to VCC |
| Communication fails | Wrong voltage levels | Check supply voltage, check ESP32 pins |

---

## Cost Optimization

- Buy resistor/capacitor packs instead of individual
- Reuse old USB power supplies if available
- Use breadboard for development (no soldering)
- Wait for sales on AliExpress (18 June, 11.11)

---

## Supplier Recommendations

- **Power Supplies**: Amazon (fast, tested quality)
- **Capacitors/Resistors**: Local electronics store or AliExpress
- **Connectors**: AliExpress (cheapest) or Amazon (faster)
- **Protective Components**: Local hardware store

---

## Electrical Safety

⚠️ **Important:**
- Never work on live circuits
- Use fuses to protect from shorts
- Double-check polarity before connecting power
- Use polyfuses for USB power
- Wear ESD protection when handling components

---

**Feature Owner**: [Name]  
**Last Updated**: June 2026  
**Estimated Cost**: $44-76  
**Status**: Planning phase
