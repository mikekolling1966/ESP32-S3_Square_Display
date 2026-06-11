Boating with the Baileys
Feb 2026

ESP32 customisable display using data from SignalK. 
------------------------------

SD CARD — CRITICAL INFORMATION
------------------------------

The display requires a FAT32 SD card formatted with an **MBR (not GPT) partition table**.
Cards formatted by Windows or modern tools often use GPT and will NOT be read by the display.

A ready-to-flash disk image is stored in this repo (via Git LFS):

    SDCARD/waveshare_square_sdcard.img

This image contains the correct MBR/FAT32 layout plus all assets and screen configs.
Flash it to a new card with:

    sudo dd if=waveshare_square_sdcard.img of=/dev/rdiskX bs=1m

Replace `/dev/rdiskX` with your SD card device (use `diskutil list` on Mac to find it).

> **WARNING:** If a card mounts as `GUID_partition_scheme` / `Microsoft Basic Data` on Mac,
> it is GPT and the display will ignore it. Use `diskutil partitionDisk /dev/diskX MBR "MS-DOS FAT32" UNTITLED 100%`
> to reformat it as MBR before copying files.

Individual asset files are also stored in:
- `SDCARD/assets/` — background `.bin` images and icon PNGs
- `SDCARD/config/` — screen config files and SignalK paths

OneDrive backup: `Projects/ESP32-S3_Square_Display_SDCARD/`

------------------------------

It features:
- Customisable background images,
- Custom icons, 
- Alerting through icon colours and built in buzzer
- Global or Per Screen buzzer
- WebUI driven
- Touch and non Touch options
- Number display with different display options, Single large display, Dual screen, Quad Screen
- Guage with added number display (Wind instrument style)
- Graph display with the option for an additional data source & data retained while device is powered
- Navigation Display with heading & Dual data feilds
- Position and Time display
- AIS Radar display

ESP32-S3 Square Display

The hardware used (Supports V3 & V4 boards):
ESP32-S3 4inch Display Development Board, 480×480, 32-Bit LX7 Dual-Core Processor, Up To 240MHz Frequency, Supports WiFi & Bluetooth, With Onboard Antenna, ESP32 With Display https://www.waveshare.com/esp32-s3-touch-lcd-4.htm

This folder contains files and documentation for building the ESP32-S3 based square display.

Contents:
- PlatformIO project sources (refer to root `src/`)

Build and upload instructions:
1. Install PlatformIO and required toolchains.
2. Open the project root and run `pio run` then `pio run --target upload`.

See the main project root for full source code and assets.

Use an SD card for your icons and images. Store icons and PNGs (ideally monochrome images) so you can recolor them in the UI. Convert larger background images to `.bin` files. There is a conversion script in the project to help with this.

The display is 480x480, so make your background images this size.

Use 70x70 for icons.

I used Figma to create my images.

Save them on the SD card in a folder called `/assets`.

On first boot, the display creates an SSID called ESP32-SquareDisplay with the password: 12345678 - Browse to 192.168.4.1 to configure your display. 

Here is a video of the device running the code and the different display options - DIY ESP32 Marine MFD – Square Multifunction Boat Display using SignalK Data https://youtu.be/FAPvdz6oN7A

The Baileys






How to convert PNG backgrounds
------------------------------

This project uses raw RGB565 `.bin` background files on the SD card. The workflow we used is:

- Convert `*.png` to RGB565 `.bin` using `convert_png_to_rgb565.py` (or run `batch_convert.sh` which calls it for the assets).
- Copy the produced `.bin` files to the SD card `assets/` folder on the display.

Example (from project root):

```bash
# convert a single PNG to a .bin
python3 convert_png_to_rgb565.py assets/Rev_Counter.png assets/Rev_Counter.bin

# or run the batch helper (installs Pillow if needed)
./batch_convert.sh
```
