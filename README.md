# WirePlumber Custom Device Rules (Ubuntu 26.04+)
This configuration allows you to declutter your Linux audio setup by completely disabling unwanted sound cards (like HDMI audio or Motherboard chips) and forcing USB DACs to hide unused "Digital/IEC958" outputs.

>[!NOTE]
>Tested with Ubuntu 26.04 and libwireplumber 0.5.13

## 📂 Configuration Path
Create the directory if it doesn't exist and place the `.conf` file inside:
```
~/.config/wireplumber/wireplumber.conf.d/50-custom-audio-rules.conf
```

## 🛠 How to find your Hardware Info

### 1. Find the Device ID:
Run the following command to see your current audio devices:
```
wpctl status
```

### 2. Inspect the Device:
Use the ID number from the step above (e.g., 52) to see its internal properties:
```
wpctl inspect 52
```

### 3. Choose the right Property:
+ PCI Devices (Internal/GPU): Use `device.name` (e.g., alsa_card.pci-0000_09_00.4). These are stable based on your motherboard slots.
+ USB Devices: Use `device.product.name` or `device.nick`. This ensures the rule follows the device even if you move it to a different USB port.

## 🖊️ Modify your conf file to correctly select devices and apply rules
```
monitor.alsa.rules = [
  {
    # RULE 1: COMPLETELY DISABLE UNWANTED HARDWARE
    # Use this for internal motherboard audio or GPU HDMI ports you don't use.
    matches = [
    # Match by exact device name (best for PCI devices)
      {
        device.name = "alsa_card.pci-0000_09_00.4"
      },
      {
	    device.name = "alsa_card.pci-0000_07_00.1"
      }
    ]
    actions = {
      update-props = {
         device.disabled = true
      }
    }
  },
  
  {
    # RULE 2: HIDE DIGITAL OUTPUTS (ANALOG-ONLY MODE)
    # Use this for USB DACs/Speakers to remove the "Digital Stereo (IEC958)" clutter.
    # The '~' allows wildcard matching to ensure it works across different USB ports if you want to use device.name 
    matches = [
      {
        device.product.name = "FiiO K11 R2R"
      },
      {
	    device.product.name = "Buildwin Media-Player"
      }
    ]
    actions = {
      update-props = {
        # Forces WirePlumber to ignore digital profiles and only create Analog Sinks
        "device.profile-set" = "analog-only.conf"
      }
    }
  }
]
```

## 🚀 Applying the Changes
After saving the file, restart the audio services to see the changes:
```
systemctl --user restart wireplumber pipewire
```
