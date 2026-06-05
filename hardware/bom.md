# Bill of Materials (BOM) - Master Index

Master bill of materials for the Smart Chicken Coop project. Parts are organized by feature.

**See individual feature BOMs for detailed information:**
- [Door Control System](./features/door-control-bom.md)
- [Environmental Monitoring](./features/environmental-monitoring-bom.md)
- [Enclosure & Structure](./features/enclosure-bom.md)
- [Power & Electronics](./features/power-electronics-bom.md)

---

## Quick Summary by Feature

| Feature | Parts | Est. Cost | Status |
|---------|-------|-----------|--------|
| **Door Control** | Motor, driver, switches | $40-60 | Not ordered |
| **Environmental Monitoring** | Sensors (temp, humidity, light) | $15-25 | Not ordered |
| **Enclosure & Structure** | 3D printed parts, fasteners, housing | $40-60 | Not ordered |
| **Power & Electronics** | Power supplies, resistors, capacitors | $40-60 | Not ordered |
| **Development & Testing** | ESP32, breadboard, jumpers | $15-25 | Not ordered |
| **TOTAL** | | **$150-230** | |

---

## Overall Cost Tracker

| Category | Budgeted | Ordered | Delivered | Tested |
|----------|----------|---------|-----------|--------|
| Door Control | $50 | $0 | $0 | $0 |
| Environmental Monitoring | $20 | $0 | $0 | $0 |
| Enclosure & Structure | $50 | $0 | $0 | $0 |
| Power & Electronics | $50 | $0 | $0 | $0 |
| Development | $20 | $0 | $0 | $0 |
| **TOTAL** | **$190** | **$0** | **$0** | **$0** |

---

## How to Use This Structure

1. **Plan**: Check individual feature BOMs for what you need
2. **Order**: Use order tracking in each feature file
3. **Track**: Update status as items arrive
4. **Build**: Reference the feature docs as you assemble

Each feature BOM includes:
- Detailed parts list with specs
- Supplier links
- Ordering tips
- Order tracking table
- Assembly notes

---

## Feature Breakdown

### 🚪 Door Control System
Core functionality: automated door opening/closing

**Components:**
- 12V DC Motor with gearbox
- Motor driver (L298N)
- Door sensors (magnetic switch)
- Mechanical mounting hardware

**See**: [door-control-bom.md](./features/door-control-bom.md)

### 🌡️ Environmental Monitoring
Sensor suite for real-time coop conditions

**Components:**
- DHT22 temperature/humidity sensor
- Light sensor (LDR)
- Additional monitoring sensors (optional)

**See**: [environmental-monitoring-bom.md](./features/environmental-monitoring-bom.md)

### 📦 Enclosure & Structure
Housing and mechanical assembly

**Components:**
- 3D printed frame and parts
- Fasteners and threaded inserts
- Weatherproof enclosure box
- Cable management

**See**: [enclosure-bom.md](./features/enclosure-bom.md)

### ⚡ Power & Electronics
Power distribution and control circuits

**Components:**
- USB 5V power supply (ESP32)
- 12V power supply (motor)
- Capacitors, resistors, connectors
- Surge protection

**See**: [power-electronics-bom.md](./features/power-electronics-bom.md)

### 🔧 Development & Testing
Tools and components for prototyping

**Components:**
- ESP32 DevKit
- Breadboard and jumper wires
- USB cables
- Test components

**See**: [development-bom.md](./features/development-bom.md)

---

## Master Order Tracking

Track all orders across all features:

| Date | Supplier | Feature | Parts | Cost | Tracking | Status |
|------|----------|---------|-------|------|----------|--------|
| | | | | | | |

---

## Ordering Strategy

### Phase 1: Development (Week 1)
Get core components for testing firmware
- ESP32, breadboard, power supplies
- Cost: $20-30

### Phase 2: Sensors (Week 2)
Test environmental monitoring
- DHT22, light sensor, resistors
- Cost: $15-20

### Phase 3: Motor & Control (Week 3)
Test door automation
- Motor, motor driver, door sensor
- Cost: $40-50

### Phase 4: Mechanical (Week 4)
Assemble physical structure
- 3D printed parts, fasteners, enclosure
- Cost: $40-60

### Phase 5: Integration (Week 5)
Final assembly and testing
- Connectors, cables, miscellaneous parts
- Cost: $15-20

---

## Notes

- Each feature can be developed and tested independently
- This modular approach allows for incremental ordering and assembly
- Check individual feature BOMs for specific part requirements
- Update master tracker after placing orders
- See [ORDER_TRACKING.md](./ORDER_TRACKING.md) for global order status

---

**Last Updated**: June 2026  
**Total Budget**: $190  
**Total Spent**: $0
