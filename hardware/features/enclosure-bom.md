# Enclosure & Structure - BOM

Bill of materials for the physical coop enclosure and mechanical structure.

**Master BOM**: [See master BOM index](../bom.md)  
**Project Status**: Not started  
**Feature Status**: Not ordered

---

## Feature Overview

Physical housing for all components:
- 3D printed frame and parts
- Weatherproof enclosure for electronics
- Door mounting system
- Cable management

---

## Parts List

### 3D Printed Components

| Part | Qty | Material | Print Time | Size | Status | Notes |
|------|-----|----------|-----------|------|--------|-------|
| Coop Frame | 1 | PLA/PETG | 12-16h | See model | ❌ Not started | Main structure enclosure |
| Door Mechanism | 1 | PLA/PETG | 2-4h | See model | ❌ Not started | Door mount & hinges |
| Sensor Housing | 1 | PLA/PETG | 1-2h | See model | ❌ Not started | Weatherproof case |
| Cable Clips | 3 | PLA/PETG | 30min | See model | ❌ Not started | Cable management |

**Print Settings:**
- Layer Height: 0.2mm
- Infill: 20-30% (30% for door mechanism)
- Support: Yes (for overhangs > 45°)
- Material: PLA (easier) or PETG (more durable, weatherproof)

**Filament Needed:** ~1-1.5kg (≈$15-25 depending on quality)

**Subtotal**: $15-25

### Fasteners & Hardware

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| M3 Screws | 20 | 16-20mm | Local | $1-2 | ❌ Not ordered | General assembly |
| M4 Screws | 10 | 20-25mm | Local | $1-2 | ❌ Not ordered | Larger joints |
| Threaded Inserts (M3) | 10 | Heat-set | Amazon | $2-3 | ❌ Not ordered | Repeatable holes |
| Threaded Inserts (M4) | 5 | Heat-set | Amazon | $1-2 | ❌ Not ordered | Door hinge |

**Subtotal**: $5-9

### Weatherproofing

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Silicone Caulk | 1 | Weatherproof sealant | Local | $5-8 | ❌ Not ordered | Seal all joints |
| Rubber Gaskets | 1 | Assorted sizes | Amazon | $2-3 | ❌ Not ordered | Seal gaps |
| Conformal Coat | 1 | Acrylic spray | Amazon | $10-15 | ❌ Not ordered | PCB protection |

**Subtotal**: $17-26

### Enclosure Box

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Plastic Enclosure | 1 | IP65+, 200x150x100mm | Amazon | $10-15 | ❌ Not ordered | Electronics housing |
| Cable Glands | 3 | M20 IP67 | Amazon | $2-4 | ❌ Not ordered | Sealed cable entries |

**Subtotal**: $12-19

### Additional Mechanical

| Part | Qty | Specification | Supplier | Price | Status | Notes |
|------|-----|---------------|----------|-------|--------|-------|
| Door Hinges | 2 | Metal or printed | Local/DIY | $3-5 | ❌ Not ordered | Door pivot |
| Spring Tension | 1 | Door return spring | Local | $2-3 | ❌ Not ordered | Auto-close (optional) |
| L-Brackets | 4 | Aluminum 20mm | Local | $2-4 | ❌ Not ordered | Structure reinforcement |

**Subtotal**: $7-12

---

## Total Cost

| Category | Cost |
|----------|------|
| 3D Printed Components | $15-25 |
| Fasteners & Hardware | $5-9 |
| Weatherproofing | $17-26 |
| Enclosure Box | $12-19 |
| Additional Mechanical | $7-12 |
| **FEATURE TOTAL** | **$56-91** |

---

## Order Tracking

Track your enclosure orders here:

| Part | Qty | Order Date | Supplier | Tracking | Cost | Status | Notes |
|------|-----|-----------|----------|----------|------|--------|-------|
| | | | | | | | |

---

## 3D Printing Guide

### Models Location

All OpenSCAD models are in `hardware/models/`:
- `coop-frame.scad`
- `door-mechanism.scad`
- `sensor-housing.scad`
- `cable-clips.scad`

### Generate STLs

```bash
# Automatic generation
dotnet run --project build/CoopBuilder -- --generate-stls

# Manual conversion (requires OpenSCAD)
openscad -o hardware/stl/coop-frame.stl hardware/models/coop-frame.scad
```

### Print Settings by Part

| Part | Infill | Support | Orientation | Time |
|------|--------|---------|-------------|------|
| Coop Frame | 30% | Yes | Flat | 12-16h |
| Door Mechanism | 30% | Yes | Motor axis vertical | 2-4h |
| Sensor Housing | 20% | Yes | Opening up | 1-2h |
| Cable Clips | 15% | No | Any | 30min |

### Material Choice

- **PLA**: Easier to print, good for prototyping, biodegradable, less UV resistant
- **PETG**: More durable, better weathering, requires higher temps, more warping risk

Recommendation: Use **PETG** for outdoor parts.

---

## Assembly Guide

### Step 1: Prepare Printed Parts

1. Remove support material carefully
2. Sand rough edges (80-150 grit)
3. Clean with compressed air

### Step 2: Install Threaded Inserts

Use heat-set inserts for repeatable assembly:

1. Heat soldering iron to 200°C
2. Press insert into hole (5-10 seconds)
3. Let cool before removing iron

### Step 3: Assemble Frame

1. Connect frame sections with M3 screws
2. Apply silicone caulk to all internal joints
3. Let cure 24 hours

### Step 4: Install Door

1. Mount hinges on frame and door
2. Hang door with M4 screws
3. Test smooth opening/closing

### Step 5: Weatherproof

1. Apply silicone caulk to all external joints
2. Add rubber gaskets where cables enter
3. Apply conformal coat to PCBs (if exposed)

### Step 6: Final Assembly

1. Mount enclosure box inside frame
2. Route cables through management clips
3. Install cable glands at entry points
4. Seal all cable entries with silicone

---

## Testing Checklist

Before deployment, verify:

- [ ] Frame is sturdy and doesn't flex excessively
- [ ] Door opens and closes smoothly
- [ ] No gaps or openings for predators
- [ ] Enclosure box stays dry in water test
- [ ] All cables protected and routed neatly
- [ ] Sensor housing ventilates properly
- [ ] Caulk is fully cured (24-48 hours)

---

## Weatherproofing Details

### Outdoor Considerations

- **UV Protection**: Paint frame with UV-resistant paint (optional)
- **Ventilation**: Ensure sensor housing has airflow
- **Drainage**: Slight slope for water runoff
- **Insulation**: Consider adding foam in winter regions

### Water Testing

Before final deployment:
1. Spray entire structure with garden hose
2. Check for water intrusion
3. Reapply caulk to any leaks
4. Wait 48 hours before use

---

## Supplier Recommendations

- **Plastic Enclosure**: Amazon (fast shipping, good selection)
- **Fasteners**: Local hardware store (no shipping, various options)
- **Weatherproofing**: Local hardware store (see products before buying)
- **3D Printing Filament**: Amazon or local 3D shop

---

## Alternatives

### 3D Printing Alternatives

- **Print Externally**: Local maker space or online service (Shapeways, Protolabs)
- **Pre-made Parts**: Aluminum enclosures (more expensive but professional)

### Weatherproofing Alternatives

- **Polyurethane Sealant**: More durable than silicone, harder to clean
- **Epoxy Coating**: Professional finish, expensive
- **Paint**: Adds weathering but not waterproof

---

## Cost Optimization

- Print overnight (cheaper power) if using maker space
- Order fasteners in bulk (100-pack is cheaper)
- Reuse old enclosure boxes if available
- DIY door hinges with printed parts

---

**Feature Owner**: [Name]  
**Last Updated**: June 2026  
**Estimated Cost**: $56-91  
**Status**: Planning phase
