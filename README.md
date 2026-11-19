<div align="center">

# USBMask

### Turn your Android phone into any USB device you want.

[![GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-yellow.svg)](LICENSE)
![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?logo=android&logoColor=white)
![Status](https://img.shields.io/badge/Status-in%20progress-orange)

</div>

---

## What is USBMask?

**USBMask** is a root Android app that reconfigures the phone's USB peripheral controller on the fly — so a single cable can make your device appear to a PC as whatever is most useful at the moment:

- ⌨️ **HID keyboard / mouse** — inject keystrokes and cursor movement into any connected computer
- 💾 **Mass storage** — expose an image file or partition as a USB drive
- 🌐 **RNDIS / ECM network adapter** — reverse-tether the PC through the phone's internet connection
- 🔀 **Composite devices** — combine multiple functions (e.g. keyboard + storage) at once
- 🧩 **Custom functions** — plumb any ConfigFS-compatible USB function via a simple config

No PC software. No proprietary drivers. Just root and a kernel with USB Gadget support.

---

## Features (planned)

- [x] Device / kernel capability detection
- [x] HID keyboard function (ConfigFS)
- [x] Mass-storage function
- [x] RNDIS network function
- [x] Composite gadget configuration
- [ ] Mouse / consumer-control HID profiles
- [ ] Saved gadget profiles + quick-switch tiles
- [ ] Tasker / automation intents
- [ ] Non-root fallback via ADB (`adb shell`) for debugging

---

## Requirements

| Item | Detail |
|---|---|
| **Root** | Required — writing to `ConfigFS` needs root |
| **Kernel** | Must have USB Gadget / ConfigFS support (`CONFIG_USB_CONFIGFS`) |
| **Android** | 8.0+ (API 26) |
| **Cable** | A **data** cable (not charge-only) |

> ⚠️ USB gadget support depends on your device's kernel. Some builds disable it entirely — USBMask will detect this and tell you if your device is unsupported.

---

## How it works

USBMask writes a gadget configuration to `/config/usb_gadget/` and binds it to the device's USB Device Controller (UDC):

```
/config/usb_gadget/cadence/
├── idVendor / idProduct
├── configs/c.1/
│   ├── functions/hid.usb0   → keyboard
│   ├── functions/mass_storage.usb0
│   └── functions/rndis.usb0
└── UDC → bind to controller
```

Switching profiles tears down the active gadget and rebinds with a new function set — usually in under a second.

---

## Tech stack

- **Kotlin** + **Jetpack Compose** (Material 3)
- Root access via **libsu**
- ConfigFS manipulation through shell / direct filesystem writes
- Coroutines for async gadget state management

---

## Status

This repo is **in active development**. The README defines the product vision and architecture; implementation is being built out. Star / watch for updates.

---

## License

**GPL-3.0** — see [LICENSE](LICENSE). Free and open source, forever.

Developed by [netherish](https://github.com/netherish).
