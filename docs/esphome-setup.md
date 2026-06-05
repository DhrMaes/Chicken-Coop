# ESPHome Configuration & Setup

Guide for configuring and flashing ESPHome firmware on the ESP32.

## Development Workflow

### Recommended Approach: Home Assistant + ESPHome Integration

The best development workflow combines Home Assistant with ESPHome's integration for live iteration:

```
Local Development Cycle:
1. Edit config in `firmware/esphome/`
2. Flash via Home Assistant UI (USB or OTA)
3. Monitor logs in real-time
4. Iterate quickly on changes
```

### Setup Options

#### Option A: Home Assistant (Recommended) ⭐

**Why this is best for development:**
- Live log viewing while testing
- Easy validation before flashing
- Visual editor with config hints
- One-click OTA updates
- All-in-one solution with automations

**Setup:**

1. Install Home Assistant on your machine:
   - **Docker** (recommended): `docker run -d -p 8123:8123 -v homeassistant:/config homeassistant/home-assistant:latest`
   - **Bare Metal**: [Instructions](https://www.home-assistant.io/installation/)
   - **Home Assistant OS**: Install on Raspberry Pi or NUC

2. Install ESPHome add-on:
   - Settings → Add-ons → Add-on Store
   - Search for "ESPHome"
   - Click "Install"

3. Start ESPHome:
   - Settings → Add-ons → ESPHome
   - Click "Start"
   - Open Web UI at `http://homeassistant.local:6052`

4. Create new device:
   - Click "Create New Device"
   - Select "ESP32"
   - Give it a name: "Chicken Coop"
   - Generate API encryption key (automatically added)
   - Connect ESP32 via USB
   - Click "Install"

#### Option B: ESPHome Standalone

**Best for:** CI/CD, Docker containers, command-line only

```bash
# Install ESPHome
pip install esphome

# Create device
esphome wizard

# Validate config
esphome config firmware/esphome/main-controller.yaml

# Compile
esphome compile firmware/esphome/main-controller.yaml

# Flash (USB)
esphome run firmware/esphome/main-controller.yaml
```

#### Option C: Web ESPHome (esphome.io)

**Best for:** Quick tests without installation

1. Visit https://web.esphome.io/
2. Connect ESP32 via USB
3. Click "Connect"
4. Select device and upload config
5. Flash directly in browser

---

## Configuration Structure

### Main Controller

**File**: `firmware/esphome/main-controller.yaml`

```yaml
esphome:
  name: chicken-coop
  friendly_name: "Chicken Coop Controller"
  min_version: 2024.11.0

esp32:
  board: esp32-devkitc-v4
  framework:
    type: arduino

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

api:
  encryption:
    key: !secret api_key

ota:
  - platform: esphome

logger:
  level: INFO

# Import sensor configs
packages:
  sensors: !include packages/sensors.yaml
  switches: !include packages/switches.yaml
```

### Organizing with Packages

Keep configs modular and maintainable:

**File**: `firmware/esphome/packages/sensors.yaml`

```yaml
sensor:
  - platform: dht
    pin: GPIO17
    temperature:
      name: "Temperature"
      filters:
        - offset: 0
    humidity:
      name: "Humidity"
    update_interval: 60s

binary_sensor:
  - platform: gpio
    pin: GPIO16
    name: "Door State"
    device_class: door
```

**File**: `firmware/esphome/packages/switches.yaml`

```yaml
output:
  - platform: gpio
    pin: GPIO12
    name: "Door Motor"

switch:
  - platform: output
    name: "Open Door"
    output: "door_motor"
```

---

## Development Tips

### Live Logging

**In Home Assistant UI:**
- Settings → Add-ons → ESPHome
- Find your device
- Click "Logs" button (bottom right)
- See real-time debug output

**Command Line:**

```bash
esphome logs firmware/esphome/main-controller.yaml
```

### Testing Sensors Before Assembly

Quick validation without full hardware:

```yaml
# Test DHT22 reading
sensor:
  - platform: dht
    pin: GPIO17
    temperature:
      name: "Test Temperature"
    humidity:
      name: "Test Humidity"
    update_interval: 5s  # Fast for testing
```

Monitor in Home Assistant or logs. Once working, increase `update_interval` for production.

### Validate Config Without Flashing

```bash
esphome validate firmware/esphome/main-controller.yaml
```

Great for CI/CD pipelines to catch errors early.

### Compile Without Flashing

```bash
esphome compile firmware/esphome/main-controller.yaml
```

Build output is in `.esphome/build/chicken-coop/.pioenvs/`

---

## Configuration Files

### Secrets File

**File**: `firmware/esphome/secrets.yaml`

```yaml
wifi_ssid: "Your WiFi Network"
wifi_password: "Your WiFi Password"
api_key: "abcdef0123456789abcdef0123456789"
```

> ⚠️ **Never commit `secrets.yaml` to git** (it's in `.gitignore`)

Create from template:
```bash
cp firmware/esphome/secrets.yaml.example firmware/esphome/secrets.yaml
```

### Example: Complete Configuration

```yaml
esphome:
  name: chicken-coop
  friendly_name: "Chicken Coop"

esp32:
  board: esp32-devkitc-v4
  framework:
    type: arduino

# WiFi with fallback AP
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  
  ap:
    ssid: "Coop Fallback Hotspot"
    password: "12345678"

api:
  encryption:
    key: !secret api_key

ota:
  - platform: esphome

logger:
  level: INFO

# DHT22 Sensor
sensor:
  - platform: dht
    pin: GPIO17
    temperature:
      name: "Temperature"
    humidity:
      name: "Humidity"
    update_interval: 60s

# Door Sensor
binary_sensor:
  - platform: gpio
    pin: GPIO16
    name: "Door"
    device_class: door

# Door Motor
output:
  - platform: gpio
    pin: GPIO12
    name: "Motor"

switch:
  - platform: output
    name: "Door Control"
    output: "Motor"

# Home Assistant service calls
service:
  - service: door_open
    then:
      - switch.turn_on: door_control
```

---

## OTA Updates

### First Time Flash

**Via USB:**
```bash
esphome run firmware/esphome/main-controller.yaml --device /dev/ttyUSB0
```

Or use Home Assistant UI (recommended).

### Subsequent Updates

**Wireless (OTA):**

```bash
esphome run firmware/esphome/main-controller.yaml --upload-port 192.168.1.100
```

Or click "Install" in Home Assistant UI.

---

## Troubleshooting

### USB Port Not Recognized

**Solution**:
- Install CH340 driver: https://sparks.gogo.co.nz/ch340.html
- Try different USB port
- Ensure USB cable is data cable (not power-only)
- Check: `ls /dev/tty*` (Linux/macOS) or Device Manager (Windows)

### Connection Timeout

**Solution**:
- Check WiFi SSID and password in `secrets.yaml`
- Verify WiFi is 2.4GHz (ESP32 doesn't support 5GHz)
- Check ESP32 is in range of router
- Restart ESP32

### Sensor Not Reading

**Solution**:
- Verify pin assignment matches config
- Check wiring with multimeter
- Try different GPIO pin
- Add pull-up resistor for DHT22 (10kΩ on DATA line)

### Home Assistant Doesn't Discover Device

**Solution**:
- Restart Home Assistant: Settings → Developer Tools → Restart
- Check ESP32 is on same network as Home Assistant
- Check Home Assistant logs for API errors
- Verify `api` section in config with encryption key

### Config Validation Errors

**Solution**:
- Use `esphome validate` to check syntax
- Check YAML indentation (spaces, not tabs!)
- Look for typos in platform names
- Reference [ESPHome docs](https://esphome.io/components/)

---

## Resources

- [ESPHome Full Docs](https://esphome.io/index.html)
- [Component Reference](https://esphome.io/components/)
- [Home Assistant ESPHome Integration](https://www.home-assistant.io/integrations/esphome/)
- [ESPHome Configuration Examples](https://esphome.io/guides/getting_started_command_line.html)

---

**Next**: Set up [Home Assistant automations](./home-assistant-integration.md)
