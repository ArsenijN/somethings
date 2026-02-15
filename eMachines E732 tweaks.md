- Recent thing that I found - you **can** control the fan speeds!!!!!!

- How?

Follow the instruction: I'm lazy to do it myself so there it is

# Fan Control for eMachines E732 (and Similar Acer Laptops) on Linux

This guide shows how to control your laptop fan speed on Linux using NBFC (NoteBook FanControl), enabling quieter operation at idle and better thermal management.

## Tested On
- **Laptop**: eMachines E732 (i3-370M)
- **OS**: Debian 12
- **Should work on**: Similar-era Acer laptops (2009-2012), Ubuntu, Mint, and other Debian-based distros

## Why Do This?
- **Constant fan noise** even at idle
- Fan runs 24/7 at same speed regardless of temperature
- Want the fan to actually turn off or slow down when cool

## Installation

### 1. Download and Install NBFC-Linux

**For Debian/Ubuntu-based systems:**

```bash
# Download the latest version
wget https://github.com/nbfc-linux/nbfc-linux/releases/download/0.3.19/nbfc-linux_0.3.19_amd64.deb

# Install it
sudo apt install ./nbfc-linux_0.3.19_amd64.deb
```

**For other distros:**
- **Arch Linux**: `yay -S nbfc-linux` or download from [releases](https://github.com/nbfc-linux/nbfc-linux/releases/tag/0.3.19)
- **Fedora**: Download `.rpm` from [releases](https://github.com/nbfc-linux/nbfc-linux/releases/tag/0.3.19)
- **Build from source** (if needed): See the [official README](https://github.com/nbfc-linux/nbfc-linux#installation)

### 2. Find Your Config

List available configs and find one close to your model:

```bash
nbfc config -l | grep -i acer
```

For **eMachines E732**, the following configs are known to work:
- `Acer Aspire 5749` (recommended)
- `Acer Aspire 5745G`
- `Acer Aspire 5738G`

### 3. Apply Config and Start Service

```bash
sudo nbfc config -a "Acer Aspire 5749"
sudo nbfc start
```

### 4. Check Status

```bash
nbfc status
```

You should see:
- `Auto Control Enabled: true`
- `Current Fan Speed` showing a value
- `Temperature` reading your CPU temp

### 5. Test Manual Control (Optional)

```bash
# Set fan to 50% speed
sudo nbfc set -f 0 -s 50

# Set to 100%
sudo nbfc set -f 0 -s 100

# Return to automatic control
sudo nbfc set -f 0 -a
```

### 6. Enable on Boot

```bash
sudo systemctl enable nbfc_service
sudo systemctl start nbfc_service
```

## Verify It's Working

Watch fan speed in real-time:

```bash
watch -n 1 nbfc status
```

Create CPU load to test (install `stress` first: `sudo apt install stress`):

```bash
stress --cpu 4 --timeout 60s
```

You should see the fan speed increase as temperature rises.

## Expected Behavior

- **Idle (30-40°C)**: Fan off or very low speed (silent!)
- **Light load (40-55°C)**: Fan at 30-50%
- **Heavy load (55-70°C)**: Fan ramps up to 70-100%

## Troubleshooting

### Service won't start
```bash
sudo nbfc start
# Check for errors in output
```

### No fan control
Try a different config from the list above. Each uses slightly different EC (embedded controller) registers.

### Fan speed shows negative or weird values
This is normal for some configs - the important part is whether the fan *actually* changes speed when you set it manually.

### Want to stop the service
```bash
sudo systemctl stop nbfc_service
sudo systemctl disable nbfc_service
```

## Other Compatible Models

NBFC has configs for many laptops. Check the full list:

```bash
nbfc config -l
```

Try configs from laptops with similar:
- Chipset (HM55, HM57 for i3-370M era)
- Brand (Acer, Gateway, eMachines were related)
- Release year (2009-2012)

## Alternative: Arduino Hardware Solution

If NBFC doesn't work for your model, you can build a hardware fan controller using an Arduino. This gives you complete control but requires:
- Arduino Nano (~$5)
- Thermistor + MOSFET
- Basic soldering/electronics skills

*(I [meant the Claude] can provide Arduino guide if there's interest)*

## Credits

- **NBFC-Linux**: https://github.com/nbfc-linux/nbfc-linux
- Original Windows NBFC project for the fan control profiles

## License

This guide is released under CC0 (public domain). Do whatever you want with it.

---

**Enjoy your quiet laptop!** 🔇
