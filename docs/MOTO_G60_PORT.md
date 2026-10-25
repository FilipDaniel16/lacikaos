Motorola Moto G60 Port Notes

Status: Early research — not bootable.

This document tracks the initial LacikaOS porting work for the Motorola Moto G60. Hardware addresses, firmware targets, and boot images must not be guessed.

1. Device identification

Reported device specifications:

- Device: Motorola Moto G60
- Model: XT2135-2 (verify in Android Settings → About phone)
- RAM: 6 GB
- SoC: Qualcomm Snapdragon 732G

These details still need to be confirmed against the actual device. Do not assume another Motorola model uses the same hardware configuration.

2. First milestone: collect device information

On a computer with Android Platform Tools installed, enable USB debugging, connect the phone, and authorize the computer.

Run:

adb devices
adb shell getprop ro.product.model
adb shell getprop ro.product.device
adb shell getprop ro.board.platform
adb shell getprop ro.boot.hardware
adb shell getprop ro.build.version.release
adb pull /sys/firmware/fdt moto-g60.dtb

The final command may fail if Android does not expose that path or access is denied. If it fails, record the error and investigate extracting the device tree from the phone's boot images or "vendor_boot".

Do not substitute a device tree from a different phone.

3. Porting checklist

- [ ] Confirm the exact model, product codename, board/platform, and Android build.
- [ ] Obtain the device's own device tree (DTB).
- [ ] Inspect the device tree and identify the GICv3 addresses and CPU topology.
- [ ] Determine whether a compatible Project Silicium / Mu-Silicium UEFI port exists or can be developed.
- [ ] Verify bootloader-unlock availability and the required boot workflow.
- [ ] Identify the framebuffer address and a safe memory layout.
- [ ] Generate an initial profile using "common/new-device.sh motog60 /path/to/moto-g60.dtb".
- [ ] Review every generated value; leave unknown values marked TODO.
- [ ] Add and review a Motorola-specific platform entry in the XNU loader.
- [ ] Investigate touchscreen, display, storage, power management, GPU, Wi-Fi, and audio support.
- [ ] Reach a first milestone of loader output and diagnostic logs.
- [ ] Test each stage on the actual device before calling it functional.

4. Important limitations

The existing phone port targets the POCO X5 Pro 5G ("redwood"), not the Moto G60. Its UEFI image, memory addresses, firmware, and hardware drivers must not be assumed to work on this phone.

The first goal is to determine whether the boot chain and loader can be brought up. A working XNU kernel and iOS userspace port is a much larger, unproven task.

Do not flash experimental images during this initial research. Preserve the original Android boot and recovery images, and verify each stage before proceeding.
