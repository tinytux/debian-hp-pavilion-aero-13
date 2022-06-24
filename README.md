# Running Linux (Debian 11 "Bullseye") on HP Pavilion Aero 13

Notes about running Debian on an HP Pavilion Aero 13-be0750nz laptop.

## What works

Most features work out of the box with Debian 11.3 (Kernel 5.10.0-13):

* Display
* Trackpad
* Keyboard backlight
* Audio
* Webcam
* USB
* Battery
* Suspend/resume
* HDMI (2560x1440@60Hz)

Working with Kernel 5.18:

* WiFi

Not working:

* Bluetooth

Not tested:

* Fingerprint reader

## Fixing WiFi

The Realtek RTL8852AE WiFi 6 802.11ax adapter requires the rtw89_8852ae driver
with firmware v0.13.33.0 and Linux 5.18 (or later):

- [Debian Kernel linux-image-5.18.0-1-amd64_5.18.2-1_amd64.deb or later](http://ftp.debian.org/debian/pool/main/l/linux-signed-amd64/?C=S;O=D)
- [Realtek RTL8852AE firmware](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/commit/rtw89/rtw8852a_fw.bin?id=4a7e1f5cabaebe5c044b94b91149f35471d1e983)

Firmware update:
```
root@debian:/mnt# ls -l
total 69272
-rw-r--r-- 1 root root 69467824 Jun 12 08:28 linux-image-5.18.0-1-amd64_5.18.2-1_amd64.deb
-rw-r--r-- 1 root root  1463264 Jun 12 08:28 rtw8852a_fw.bin
root@debian:/mnt# cp -v rtw8852a_fw.bin /usr/lib/firmware/rtw89/
'rtw8852a_fw.bin' -> '/usr/lib/firmware/rtw89/rtw8852a_fw.bin'
```

Kernel update:
```
root@debian:/mnt# dpkg -i linux-image-5.18.0-1-amd64_5.18.2-1_amd64.deb
Selecting previously unselected package linux-image-5.18.0-1-amd64.
(Reading database ... 272720 files and directories currently installed.)
Preparing to unpack linux-image-5.18.0-1-amd64_5.18.2-1_amd64.deb ...
Unpacking linux-image-5.18.0-1-amd64 (5.18.2-1) ...
Setting up linux-image-5.18.0-1-amd64 (5.18.2-1) ...
I: /vmlinuz is now a symlink to boot/vmlinuz-5.18.0-1-amd64
I: /initrd.img is now a symlink to boot/initrd.img-5.18.0-1-amd64
/etc/kernel/postinst.d/dkms:
dkms: running auto installation service for kernel 5.18.0-1-amd64:.
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-5.18.0-1-amd64
W: Possible missing firmware /lib/firmware/amdgpu/arcturus_gpu_info.bin for module amdgpu
W: Possible missing firmware /lib/firmware/amdgpu/ip_discovery.bin for module amdgpu
...
W: Possible missing firmware /lib/firmware/amdgpu/vangogh_dmcub.bin for module amdgpu
W: Possible missing firmware /lib/firmware/amdgpu/navy_flounder_dmcub.bin for module amdgpu
/etc/kernel/postinst.d/zz-update-grub:
Generating grub configuration file ...
Found background image: /usr/share/images/desktop-base/desktop-grub.png
Found linux image: /boot/vmlinuz-5.18.0-1-amd64
Found initrd image: /boot/initrd.img-5.18.0-1-amd64
Found linux image: /boot/vmlinuz-5.10.0-13-amd64
Found initrd image: /boot/initrd.img-5.10.0-13-amd64
Found Windows Boot Manager on /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for EFI firmware configuration
done
```

## System details

```
System:    Host: debian Kernel: 5.18.0-1-amd64 x86_64 bits: 64 Console: N/A Distro: Debian GNU/Linux 11 (bullseye)
Machine:   Type: Laptop System: HP product: HP Pavilion Aero Laptop 13-be0xxx v: N/A serial: x
           Mobo: HP model: 8916 v: 79.30 serial: x UEFI: AMI v: F.08 date: 03/09/2022
Battery:   ID-1: BAT0 charge: 21.1 Wh condition: 42.2/42.2 Wh (100%)
CPU:       Info: 8-Core model: AMD Ryzen 7 5800U with Radeon Graphics bits: 64 type: MT MCP L2 cache: 4 MiB
           Speed: 1397 MHz min/max: 1600/1900 MHz Core speeds (MHz): 1: 1397 2: 1360 3: 1397 4: 1397 5: 1396 6: 1395 7: 1397
           8: 1396 9: 1395 10: 1396 11: 1396 12: 1397 13: 1397 14: 1395 15: 1397 16: 1425
Graphics:  Device-1: Advanced Micro Devices [AMD/ATI] Cezanne driver: amdgpu v: kernel
           Device-2: Chicony HP Wide Vision HD Camera type: USB driver: uvcvideo
           Display: wayland server: X.Org 1.20.11 driver: loaded: amdgpu note: n/a (using device driver)
           resolution: 2560x1600~60Hz
           OpenGL: renderer: AMD RENOIR (DRM 3.46.0 5.18.0-1-amd64 LLVM 11.0.1) v: 4.6 Mesa 20.3.5
Audio:     Device-1: Advanced Micro Devices [AMD/ATI] driver: snd_hda_intel
           Device-2: Advanced Micro Devices [AMD] Raven/Raven2/FireFlight/Renoir Audio Processor driver: snd_rn_pci_acp3x
           Device-3: Advanced Micro Devices [AMD] Family 17h HD Audio driver: snd_hda_intel
           Sound Server: ALSA v: k5.18.0-1-amd64
Network:   Device-1: Realtek driver: rtw89_8852ae
           IF: wlp1s0 state: up mac: x
Bluetooth: Device-1: Realtek Bluetooth Radio type: USB driver: btusb
           Report: ID: hci0 state: up running bt-v: 3.0 address: x
Drives:    Local Storage: total: 953.87 GiB used: 716.42 GiB (75.1%)
           ID-1: /dev/nvme0n1 vendor: Western Digital model: PC SN530 SDBPNPZ-1T00-1006 size: 953.87 GiB
Sensors:   Message: No sensors data was found. Is sensors configured?
Info:      Processes: 346 Uptime: 3m Memory: 15.05 GiB used: 1.93 GiB (12.8%) Init: systemd runlevel: 5 Shell: Bash
           inxi: 3.3.01
```
