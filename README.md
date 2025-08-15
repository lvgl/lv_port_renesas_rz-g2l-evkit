# LVGL ported to Renesas RZ/G2L-EVKIT

**:rocket: This repository is available in the [LVGL Project Creator](https://lvgl.io/tools/project-creator), making it easy to create and customize a new project in just a few clicks.**

## Overview

The Renesas RZ/G2L is a general-purpose MPU with a Cortex-A55 (1.2 GHz) and a 3D GPU
with video codecs.
This evaluation kit consists of
both a SoM (System on Module) which carries the RZ/G2L MPU, as well as a carrier board
which carries the SoM. The SoM complies with the SMARC v2.1 stardard. The carrier board
features IO useful for MPU applications such as Micro-HDMI, 2 ethernet ports,
headphone and microphone jacks, USB serial terminal, USB host, camera input,
an additional Micro SD card slot, and exposed GPIO headers.

## Buy

You can purchase the RZ/G2L-EVKIT from several sources. The RZ/G2L is also sold as
SoM-only if you are using a different carrier board or already have one.

- [Renesas's website](https://www.renesas.com/en/products/microcontrollers-microprocessors/rz-mpus/rzg2l-evkit-evaluation-board-kit-rzg2l-mpu)
- [Mouser](https://www.mouser.com/ProductDetail/Renesas-Electronics/RTK9744L23S01000BE?qs=7D1LtPJG0i2Uv%2FGoPVv6xw%3D%3D)
- [DigiKey](https://www.digikey.com/en/products/detail/renesas-electronics-corporation/RTK9744L23S01000BE/15290121)

## Benchmark

The benchmark was run with the v8.3 Wayland driver.

<a href="https://www.youtube.com/watch?v=oeuVvB7y-QA">
    <img src="https://github.com/user-attachments/assets/b183aa29-fc2a-48aa-b195-d961d5222ec9" width="75%">
</a>

| Name                      | Avg. CPU | Avg. FPS | Avg. time | render time | flush time |
| :------------------------ | -------: | -------: | --------: | ----------: | ---------: |
| Empty screen              | 95%      | 30       | 32        | 2           | 30         |
| Moving wallpaper          | 98%      | 26       | 40        | 6           | 34         |
| Single rectangle          | 9%       | 33       | 0         | 0           | 0          |
| Multiple rectangles       | 85%      | 30       | 31        | 1           | 30         |
| Multiple RGB images       | 99%      | 30       | 31        | 5           | 26         |
| Multiple ARGB images      | 99%      | 20       | 48        | 14          | 34         |
| Rotated ARGB images       | 99%      | 13       | 92        | 57          | 35         |
| Multiple labels           | 99%      | 25       | 40        | 8           | 32         |
| Screen sized text         | 9%       | 33       | 2         | 0           | 2          |
| Multiple arcs             | 86%      | 24       | 42        | 8           | 34         |
| Containers                | 93%      | 31       | 28        | 6           | 22         |
| Containers with overlay   | 99%      | 21       | 47        | 20          | 27         |
| Containers with opa       | 86%      | 28       | 32        | 10          | 22         |
| Containers with opa_layer | 99%      | 21       | 46        | 18          | 28         |
| Containers with scrolling | 98%      | 21       | 48        | 19          | 29         |
| Widgets demo              | 85%      | 22       | 42        | 12          | 30         |
| All scenes avg.           | 83%      | 25       | 36        | 11          | 25         |

## Specification

### CPU and Memory
- **MCU:** Arm Cortex-A55 (1.2 GHz)
- **RAM:** 2GB DDR4 SDRAM
- **Flash:** Micro SD card, 64GB eMMC, 64MB QSPI flash
- **GPU:** Arm Mali-G31 (has 3D acceleration)

### Display and Touch
- **Resolution:** Tested on 1920x1080
- **Interface:** Micro-HDMI
- **Color Depth:** 32-bit
- **Touch Pad:** USB touch input supported

### Connectivity
- 2 ethernet ports
- headphone and microphone jacks
- USB port
- camera input
- additional Micro SD card slot
- GPIO headers
- USB serial terminal

## Getting started

### Hardware setup
- Insert a prepared Micro SD card in the slot on the SoM (not the slot on the carrier board).
  See the next section for details about obtaining an OS image for the Micro SD card.
- Connect an HDMI display with a Micro-HDMI cable. If you use an HDMI-powered display, ensure
  the board's power adapter provides enough power for both the board and the display.
- Connect a USB mouse to interract with LVGL UIs. Optionally use a USB hub to connect
  a keyboard too, if you want to use the desktop terminal.
- Optionally connect an ethernet cable to either of the two ethernet ports.
- Optionally connect the micro USB port marked "SER 3 UART"/"CN14" to your computer.
- Connect a power adapter to the USB-C port. It was tested with a 5V-3A power supply and
  also with a PC USB-C port. See the next section about turning the board on and off.

### Software setup

These steps are based on the [Renesas RZ/G HMI SDK Getting Started guide](https://renesas-rz.github.io/rzg_hmi_sdk/latest/getting_started/).

1. Download [RZ/G HMI SDK Version 2.3.0.0](https://www.renesas.com/en/document/sws/rzg-hmi-software-development-kit-version-2300)
2. Extract the zip file.
3. Navigate into the extracted directory and extract the nested zip file called `image-file_rzg2l_hmi-sdk_v2.3.0.0.zip`
   which contains `core-image-weston-smarc-rzg2l.wic.bmap` and `core-image-weston-smarc-rzg2l.wic.gz`.
4. Insert a Micro SD card into your PC and flash the image to it.
   - On a Linux PC, first ensure all partitions on the card are unmounted.
     See [the officical guide](https://renesas-rz.github.io/rzg_hmi_sdk/latest/getting_started/) for more details about that.
     Use `bmaptool` (`sudo apt install bmap-tools`) to flash the SD card with the command
     ```shell
     sudo bmaptool copy core-image-weston-smarc-rzg2l.wic.gz <path to block device>
     ```
     Where `<path to block device>` is the Micro SD card block device. It may be `/dev/mmcblk0`. Take care to ensure
     you do not accidentally overwrite a system drive.
   - On a Windows PC, use Win32 Disk Imager or balenaEtcher (you can also use this on a Linux PC, but `bmaptool` may be faster)
     to flash the file called `core-image-weston-smarc-rzg2l.wic.gz`.
5. Set the DIP switches as in [the officical guide](https://renesas-rz.github.io/rzg_hmi_sdk/latest/getting_started/).
   SW1 is `11` and SW11 is `1101`
6. Set up the board as in the "Hardware setup" section. Press the red power button (SW9) for 1 second.

### Run the project

1. Prepare a Linux development environment on a Linux PC or in a Docker container, WSL, or similar. Ensure the packages
   `python3`, `xz`, `file`, and `make` are installed. I.e.
   ```shell
   sudo apt install python3 xz-utils file make
   ```
2. In the extracted SDK zip from the previous section, extract the nested zip file called
   `toolchain-installer_rzg2l_hmi-sdk_v2.3.0.0.zip`.
3. Run the below command and follow the prompts to install the cross-compiler.
   ```shell
   sudo sh poky-glibc-x86_64-core-image-weston-aarch64-smarc-rzg2l-toolchain-3.1.31.sh
   ```
4. From now on, every time you open a new terminal, you will need to run the following command to configure your shell's
   environment variables for building applications for RZ/G2L.
   ```shell
   source /opt/poky/3.1.31/environment-setup-aarch64-poky-linux
   ```
5. Build the application.
   ```shell
   make -j$(nproc)
   ```
6. Now you can transfer the binary application `lvgl_demo_benchmark` to the board. You can do it any of the following ways.
   - If connected to ethernet you can transfer the file over LAN.
     ```shell
     scp lvgl_demo_benchmark root@<ip-address>:/home/root/
     ```
     where `<ip-address>` is replaced by the local IP address of the board. You can find it with
     ```shell
     ip a
     ```
     and inspect the `eth0`/`eth1`->`inet` field.
     The file will be sent to `/home/root/`.
   - You can mount the SD card on your PC and put the file into the image's filesystem anywhere you like.
     There are two partitions on the SD card. Go to the partition called `root`. A good place to put the application is
     `/home/root/`. You may need need root privileges on your PC to access this directory. Make sure you unmount
     the SD card before removing to ensure the changes are synced fully.
   - Use a USB drive to transfer the file.
7. To delete all the build outputs, run
   ```shell
   make clean
   ```
   or
   ```shell
   git clean -Xf .
   ```

### Debugging

Unfortunately `gdb` and `gdbserver` are not installed in the image. "`printf`" debugging will have to suffice.

## Notes

See these official Renesas resources.

- [Renesas RZ/G HMI SDK Getting Started guide](https://renesas-rz.github.io/rzg_hmi_sdk/latest/getting_started/)
- [`rzg_hmi_sdk` GitHub repo](https://github.com/renesas-rz/rzg_hmi_sdk)

Continue reading for advanced extra info.

The `rzg_hmi_sdk` repo builds apps by _dynamically_ linking to the `lvgl` and `lv_drivers` system libraries which are resident
in the G2L's image at `/usr/lib/`. Dynamic linking reduces the size of the executable since the executable
does not actually contain LVGL's internal functions. There are other benefits but it's out of the scope of this README.
In contrast, this `lv_port_renesas_rz-g2l-evkit` repo _statically_ links `lvgl` and `lv_drivers` because the versions
of `lvgl` and `lv_drivers` here are different than the ones installed in the G2L image.
The `lvgl` here ports the v9 benchmark to v8.3 and the `lv_drivers` here has a bugfix for the v8.3 Wayland driver. 

The `lv_conf.h` and `lv_drv_conf.h` are similar to the ones in
`/opt/poky/3.1.31/sysroots/aarch64-poky-linux/usr/include/lvgl/`.
The `rzg_hmi_sdk` repo uses those headers when compiling.

## Contribution and Support

If you find any issues with the development board feel free to open an Issue in this repository. For LVGL related issues (features, bugs, etc) please use the main [lvgl repository](https://github.com/lvgl/lvgl).

If you found a bug and found a solution too please send a Pull request. If you are new to Pull requests refer to [Our Guide](https://docs.lvgl.io/master/CONTRIBUTING.html#pull-request) to learn the basics.

