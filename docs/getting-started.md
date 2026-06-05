# 📖 Getting Started

Complete setup guide for the Smart Chicken Coop project.

## Prerequisites

### Hardware
- ESP32 microcontroller
- Sensors (temperature, humidity, door switch, light)
- 3D printer (optional, for printed parts)
- WiFi router with 2.4GHz support

### Software
- [.NET 10+ SDK](https://dotnet.microsoft.com/download) (for build system)
- [Home Assistant](https://www.home-assistant.io/installation/) (recommended for ESPHome development)
- [OpenSCAD](http://www.openscad.org/) (optional, for editing 3D models)

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/DhrMaes/Chicken-Coop.git
cd Chicken-Coop
```

### 2. Install .NET 10+ SDK

Choose your operating system:

**Windows**
- Download from https://dotnet.microsoft.com/download
- Run the installer
- Verify: `dotnet --version` (should be 10.0.0 or higher)

**macOS**
```bash
brew install dotnet

# Verify (should be 10.0.0 or higher)
dotnet --version
```

**Linux (Ubuntu/Debian)**
```bash
sudo apt-get update
sudo apt-get install dotnet-sdk-10.0

# Verify
dotnet --version
```

### 3. Build the Project

Generate 3D models and validate configurations:

```bash
dotnet run --project build/CoopBuilder
```

For specific tasks:
```bash
# Generate STL files from OpenSCAD models
dotnet run --project build/CoopBuilder -- --generate-stls

# Validate configurations
dotnet run --project build/CoopBuilder -- --validate-esphome --validate-ha
```

See [Build System Documentation](../build/README.md) for more options.

### 4. Set Up Home Assistant & ESPHome

**Recommended Approach** for development: Use Home Assistant with the ESPHome add-on.

See [ESPHome Setup Guide](./esphome-setup.md) for:
- Installation options (Docker, bare metal, Home Assistant OS)
- Development workflow
- Configuration structure
- Testing & debugging tips

### 5. Integrate with Home Assistant

Once the ESP32 is connected and configured, Home Assistant will auto-discover the device.

For custom automations and dashboards, see [Home Assistant Integration Guide](./home-assistant-integration.md).

## Quick Start

### Minimal Setup (15 min)

1. **Install Home Assistant** - Docker is fastest
   ```bash
   docker run -d -p 8123:8123 -v homeassistant:/config homeassistant/home-assistant:latest
   ```

2. **Install ESPHome add-on**
   - Open Home Assistant UI: `http://localhost:8123`
   - Go to Settings → Add-ons → Add-on Store
   - Search and install "ESPHome"

3. **Create device in ESPHome**
   - Open ESPHome Web UI: `http://localhost:6052`
   - Click "Create New Device"
   - Select ESP32 board
   - Connect ESP32 via USB
   - Click "Install"

4. **View in Home Assistant**
   - Device will appear automatically
   - Check Settings → Devices & Services

### Full Production Setup

See [ESPHome Setup Guide](./esphome-setup.md) for:
- Configuration organization with packages
- Live logging & debugging
- OTA wireless updates
- Sensor testing before assembly
- Configuration validation

## Project Structure

```
Chicken-Coop/
├── firmware/              # ESPHome controller code
│   ├── esphome/          # Configuration files
│   └── README.md
├── hardware/              # 3D models & schematics
│   ├── models/           # OpenSCAD source
│   ├── stl/              # Generated STL files
│   └── README.md
├── integration/          # Home Assistant config
│   ├── automations/
│   ├── scripts/
│   └── lovelace/
├── docs/                 # Documentation
│   ├── getting-started.md
│   ├── esphome-setup.md
│   ├── home-assistant-integration.md
│   ├── hardware-assembly.md
│   ├── troubleshooting.md
│   └── blog/
├── build/               # .NET build system
│   └── CoopBuilder/
└── README.md
```

## Troubleshooting

### .NET SDK Version Too Old
```bash
# Verify .NET 10+
dotnet --version

# If too old, update from https://dotnet.microsoft.com/download
```

### OpenSCAD Not Found (for STL generation)
**Windows**: Add OpenSCAD to PATH or install from http://www.openscad.org/

**macOS**: `brew install openscad`

**Linux**: `sudo apt-get install openscad`

### ESP32 Not Connecting
- Check USB cable (must be data cable, not power-only)
- Install CH340 driver if using clone boards: https://sparks.gogo.co.nz/ch340.html
- Try different USB port

### Home Assistant Can't Find ESP32
- Ensure ESP32 and Home Assistant are on same WiFi network
- Check firewall settings
- Restart Home Assistant and ESP32

### ESPHome Configuration Errors
- Use `esphome validate firmware/esphome/main-controller.yaml`
- Check YAML syntax (indentation, colons, no tabs)
- See [troubleshooting guide](./troubleshooting.md)

## Next Steps

1. **Follow ESPHome Setup Guide** - Choose development approach
2. **Configure Sensors** - Set up DHT22, door sensor, etc.
3. **Create Home Assistant Automations** - Door schedules, alerts
4. **Build Hardware** - Print components, assemble coop
5. **Monitor & Optimize** - Adjust settings based on real-world data

## Resources

- [ESPHome Documentation](https://esphome.io/)
- [Home Assistant Docs](https://www.home-assistant.io/docs/)
- [ESPHome Setup Guide](./esphome-setup.md) - Recommended development workflow
- [Project Blog](./blog/) - Development progress & tips

---

**Need Help?** Check the [troubleshooting guide](./troubleshooting.md) or review the [project blog](./blog/) for common issues.
