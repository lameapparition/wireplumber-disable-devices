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

## 🚀 Applying the Changes
After saving the file, restart the audio services to see the changes:
```
systemctl --user restart wireplumber pipewire
```
