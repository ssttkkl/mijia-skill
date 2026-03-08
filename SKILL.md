---
name: mijia
description: Control Xiaomi Mijia smart home devices via mijiaAPI. Use when the user wants to control, query, or automate Mijia/Mi Home devices including heaters, washers, lamps, plugs, fans, air purifiers, humidifiers, robot vacuums, and smart bands. Supports listing devices, getting/setting properties, running actions, and managing scenes.
---

# Mijia Smart Home Control

Control Xiaomi Mijia smart devices using the third-party `mijiaAPI` Python library ([GitHub](https://github.com/Do1e/mijia-api)).

**Execution methods:** `uv run` (recommended), `pipx`, `pip`, or direct Python.

**Supported devices:** heaters, washers, lamps, plugs, fans, air purifiers, humidifiers/diffusers, robot vacuums, smart bands/watches, and more.

## Setup

Install dependencies using `uv`:

```bash
cd ~/.openclaw/workspace/skills/mijia
uv sync
```

This will install `mijiaapi` and all required dependencies.

## How to Use

### Basic Commands

#### 1. List All Devices
```bash
uv run mijiaapi --list_devices
```

#### 2. Get Device Info by Model
```bash
uv run mijiaapi --get_device_info mibx5.washer.32
```

#### 3. Get Device Property
```bash
uv run mijiaapi get --did <device_id> --prop_name on
```

#### 4. Set Device Property (Turn On/Off)
```bash
# Turn on device
uv run mijiaapi set --did <device_id> --prop_name on --value true

# Turn off device
uv run mijiaapi set --did <device_id> --prop_name on --value false
```

#### 5. Run Device Action
```bash
# For washers: start washing
uv run python -c "from mijiaAPI import mijiaAPI; api = mijiaAPI(); api.run_action({'did': '<device_id>', 'siid': 2, 'aiid': 2})"
```

### Device-Specific Examples

#### Heater (Graphene Heater)
```bash
# Check if heater is on
uv run mijiaapi get --did <heater_did> --prop_name on

# Turn on heater
uv run mijiaapi set --did <heater_did> --prop_name on --value true

# Set target temperature to 26°C
uv run mijiaapi set --did <heater_did> --prop_name target-temperature --value 26

# Get current temperature
uv run mijiaapi get --did <heater_did> --prop_name temperature
```

#### Washer (Washing Machine)

**IMPORTANT**: Starting a washer requires calling the `start-wash` action. The CLI tool doesn't support actions, so you must use Python API.

**⚠️ Action IDs vary by device model!** Different washer models use different `siid`/`aiid` values. Query your specific device model once to get the correct action IDs, then reuse them.

**Standard Start Procedure**:
```bash
# 1. Check device status first
uv run mijiaapi get --did <washer_did> --prop_name status

# 2. Get device info for YOUR model (shows properties and actions)
uv run mijiaapi --get_device_info <your_washer_model>

# 3. Find the action IDs from the output, then start washing
uv run python -c "
from mijiaAPI import mijiaAPI
api = mijiaAPI()
# Replace siid/aiid with values from your device info output
result = api.run_action({'did': '<washer_did>', 'siid': 2, 'aiid': 2})
print(result)
"
```

**Finding Action IDs for Your Model**:
```bash
# Query YOUR specific washer model
uv run mijiaapi --get_device_info mibx5.washer.32

# In the output, look under "actions" section:
# {
#   "start-wash": {"siid": 2, "aiid": 2},  <-- Your model's values
#   "pause": {"siid": 2, "aiid": 3},
#   "stop-washing": {"siid": 2, "aiid": 1}
# }
```

**Other Operations**:
```bash
# Check washing status
uv run mijiaapi get --did <washer_did> --prop_name status
uv run mijiaapi get --did <washer_did> --prop_name run-status

# Get left time
uv run mijiaapi get --did <washer_did> --prop_name left-time

# Set target temperature (before starting)
uv run mijiaapi set --did <washer_did> --prop_name target-temperature --value 40

# Turn on/off (standby mode, not actual washing)
uv run mijiaapi set --did <washer_did> --prop_name on --value true
```

**Common Action Names** (actual IDs vary by model):
- `start-wash` - 开始洗涤
- `pause` - 暂停
- `stop-washing` - 停止洗涤

#### Lamp (Desk Lamp)
```bash
# Turn on lamp
uv run mijiaapi set --did <lamp_did> --prop_name on --value true

# Set brightness (1-100%)
uv run mijiaapi set --did <lamp_did> --prop_name brightness --value 80

# Set color temperature (2700-6500K)
uv run mijiaapi set --did <lamp_did> --prop_name color-temperature --value 4000

# Get current status
uv run mijiaapi get --did <lamp_did> --prop_name on
uv run mijiaapi get --did <lamp_did> --prop_name brightness
```

#### Diffuser/Humidifier (Aroma Diffuser)
```bash
# Turn on diffuser
uv run mijiaapi set --did <diffuser_did> --prop_name on --value true

# Set mist level
uv run mijiaapi set --did <diffuser_did> --prop_name mist-level --value 2

# Get status
uv run mijiaapi get --did <diffuser_did> --prop_name on
```

### Extended Features

#### List Homes/Families
```bash
uv run mijiaapi --list_homes
```

#### List Automation Scenes
```bash
uv run mijiaapi --list_scenes
```

#### Run Scene
```bash
uv run mijiaapi --run_scene "Home Mode"
uv run mijiaapi --run_scene "Sleep Mode" "Away Mode"  # Multiple scenes
```

#### List Consumable Items (Filters, etc.)
```bash
uv run mijiaapi --list_consumable_items
```

## Finding Device Information

### Get All Devices with Status
```bash
uv run mijiaapi --list_devices
```

Output format:
```
Device ID: <device_id>
Name: Mijia Heater
Model: xiaomi.heater.ma8
Status: Online/Offline
...
```

### Get Device Model Info
```bash
uv run mijiaapi --get_device_info <model>
```

This shows:
- All available properties (with siid, piid)
- All available actions (with siid, aiid)
- Property types and ranges


## Natural Language Usage

When using this skill, you can say:

| Say This | Do This |
|----------|---------|
| "List my Mijia devices" | `uv run mijiaapi --list_devices` |
| "What's the status of [device]" | `uv run mijiaapi get --did <did> --prop_name <property>` |
| "Turn on [device]" | `uv run mijiaapi set --did <did> --prop_name on --value true` |
| "Turn off [device]" | `uv run mijiaapi set --did <did> --prop_name on --value false` |
| "Start [device] action" | Use Python API: `uv run python -c "..."` |
| "Set [device] temperature to 26" | `uv run mijiaapi set --did <did> --prop_name target-temperature --value 26` |
| "What devices are online?" | Check `--list_devices` output for Online status |
| "List all homes" | `uv run mijiaapi --list_homes` |
| "List all scenes" | `uv run mijiaapi --list_scenes` |
| "Run [scene name] scene" | `uv run mijiaapi --run_scene "Scene Name"` |

## Common Errors

### "Device Offline"
- **Cause**: Device is not connected to network
- **Solution**:
  1. Check device power
  2. Check WiFi connection
  3. Restart router
  4. Check device status in Mi Home app

### "Could not find device"
- **Cause**: Wrong DID or device is offline
- **Solution**:
  1. Run `uv run mijiaapi --list_devices` to verify DID
  2. Check if device is online
  3. Verify network connectivity

### "Property not found" / "Action not found"
- **Cause**: Wrong property/action name or siid/piid/aiid
- **Solution**:
  1. Run `uv run mijiaapi --get_device_info <model>` to see available properties/actions
  2. Use exact property/action names from the output

### "Authentication failed"
- **Cause**: Login token expired
- **Solution**: 
  1. First run will show QR code
  2. Scan with Mi Home app
  3. Auth data saved in `~/.config/mijia-api/auth.json`

## Package Info

- **Package**: `mijiaapi` version 3.0.5+
- **Runner**: `uv run` (managed by uv)
- **Auth file**: `~/.config/mijia-api/auth.json`
- **Requires**: `uv` installed (`brew install uv`)
- **Upstream**: https://github.com/Do1e/mijia-api

## Full CLI Reference

See all available commands:

```bash
# Show help
uv run mijiaapi --help

# Show device info
uv run mijiaapi --get_device_info <model>

# List devices
uv run mijiaapi --list_devices

# Get property
uv run mijiaapi get --did <did> --prop_name <property>

# Set property
uv run mijiaapi set --did <did> --prop_name <property> --value <value>

# Run action (use Python API)
uv run python -c "from mijiaAPI import mijiaAPI; api = mijiaAPI(); api.run_action({'did': '<did>', 'siid': <siid>, 'aiid': <aiid>})"

# List homes
uv run mijiaapi --list_homes

# List scenes
uv run mijiaapi --list_scenes

# Run scene
uv run mijiaapi --run_scene "Scene Name"

# List consumables
uv run mijiaapi --list_consumable_items
```

## Quick Reference: Common siid/piid/aiid

### Heater (xiaomi.heater.ma8)
- `siid=2, piid=1`: on/off
- `siid=3, piid=3`: current temperature
- `siid=3, piid=5`: target temperature
- `siid=3, piid=6`: power state

### Washer (mibx5.washer.32)
- `siid=2, piid=1`: on/off
- `siid=2, piid=2`: status
- `siid=2, piid=5`: left time
- `siid=3, piid=2`: target temperature
- **Actions** (⚠️ IDs vary by model — query your device):
  - `start-wash` (开始洗涤)
  - `pause` (暂停)
  - `stop-washing` (停止洗涤)

### Diffuser (bwj.diffuser.s5)
- `siid=2, piid=1`: on/off
- `siid=2, piid=2`: status
- `siid=2, piid=3`: mist level

---

*Last updated: 2026-03-09*
