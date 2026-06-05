# Build System - CoopBuilder

The **CoopBuilder** is a cross-platform .NET-based build system for automating project tasks:
- 🖨️ Generate STL files from OpenSCAD models
- ✅ Validate ESPHome YAML configurations
- ✅ Validate Home Assistant configurations
- 🧹 Clean generated artifacts

## Prerequisites

- [.NET 10+ SDK](https://dotnet.microsoft.com/download)
- [OpenSCAD](http://www.openscad.org/) (for STL generation)

## Installation

### Windows
```bash
# Install .NET 10+ SDK
# Download from: https://dotnet.microsoft.com/download

# Verify installation
dotnet --version
```

### macOS
```bash
brew install dotnet

# Verify
dotnet --version
```

### Linux (Ubuntu/Debian)
```bash
sudo apt-get update
sudo apt-get install dotnet-sdk-10.0

# Verify
dotnet --version
```

## Usage

Navigate to the repository root and run:

```bash
# Full build (generate STLs, validate all configs)
dotnet run --project build/CoopBuilder

# Generate only STL files
dotnet run --project build/CoopBuilder -- --generate-stls

# Validate only ESPHome config
dotnet run --project build/CoopBuilder -- --validate-esphome

# Validate only Home Assistant config
dotnet run --project build/CoopBuilder -- --validate-ha

# Clean generated files
dotnet run --project build/CoopBuilder -- --clean

# Show help
dotnet run --project build/CoopBuilder -- --help
```

## Configuration

Edit `build/CoopBuilder/config.json` to customize:

```json
{
  "openScadPath": "openscad",
  "modelsDirectory": "hardware/models",
  "stlOutputDirectory": "hardware/stl",
  "esphomeDirectory": "firmware/esphome",
  "haConfigDirectory": "integration"
}
```

## Architecture

### Project Structure
```
build/CoopBuilder/
├── CoopBuilder.csproj          # Project file
├── Program.cs                  # Entry point & CLI
├── config.json                 # Build configuration
├── Tasks/
│   ├── GenerateStlsTask.cs    # OpenSCAD → STL conversion
│   ├── ValidateEspHomeTask.cs # ESPHome validation
│   ├── ValidateHaConfigTask.cs# Home Assistant validation
│   └── CleanTask.cs            # Cleanup
└── Utilities/
    ├── ProcessRunner.cs        # Execute external commands
    └── Logger.cs               # Logging utilities
```

### Adding New Tasks

Create a new task by implementing `ITask`:

```csharp
public class MyTask : ITask
{
    public string Name => "my-task";
    public string Description => "Description of what the task does";

    public async Task<bool> Execute()
    {
        // Your logic here
        return true;
    }
}
```

Then register in `Program.cs`:

```csharp
var tasks = new Dictionary<string, ITask>
{
    ["my-task"] = new MyTask(),
    // ...
};
```

## CI/CD Integration

The build system runs in GitHub Actions workflows:

- **Validate**: Runs on every push (validates all configs)
- **Generate STLs**: Runs on push to `main` (generates & commits STLs)
- **Full Build**: Runs on PR creation

See `.github/workflows/` for workflow definitions.

## Troubleshooting

### .NET SDK version too old
**Error**: `.NET 10+ is required`

**Solution**:
- Update .NET: https://dotnet.microsoft.com/download
- Check version: `dotnet --version` (should be 10.0.0 or higher)

### OpenSCAD not found
**Error**: `openscad: command not found`

**Solution**:
- **Windows**: Add OpenSCAD to PATH or set `openScadPath` in config.json
- **macOS**: `brew install openscad`
- **Linux**: `sudo apt-get install openscad`

### YAML validation fails
**Error**: ESPHome config validation errors

**Solution**: Check the error message and see [docs/troubleshooting.md](../docs/troubleshooting.md)

## Performance

- **STL Generation**: ~2-10s per model (depends on complexity)
- **Validation**: <1s for each config file

For large projects, consider running specific tasks instead of full build:
```bash
dotnet run --project build/CoopBuilder -- --generate-stls
```

## Development

### Build the project
```bash
dotnet build build/CoopBuilder
```

### Run tests (when added)
```bash
dotnet test build/CoopBuilder.Tests
```

### Debug mode
```bash
dotnet run --project build/CoopBuilder -c Debug
```

---

**Next**: Read [ESPHome Setup](../docs/esphome-setup.md) or [Home Assistant Integration](../docs/home-assistant-integration.md)
