#### Installation

1. Download Raspbian Stretch Lite from
https://www.raspberrypi.org/downloads/raspbian/

2. Copy to SD card (> 4GB)

> dd if=/home/niklas/Downloads/2017-11-29-raspbian-stretch-lite.img | pv | dd of=/dev/sdb
>
> sync

3. Plug SD card in your Pi and power it up. Wait for a few minutes.

4. Plug the SD card back to your PC. Mount the boot partition and edit the "config.txt". Go to the bottom and add:

>dtoverlay=dwc2as
>
>#Overscan Information.
>#overscan_left=-32
>#overscan_right=-32
>overscan_top=10
>#overscan_bottom=-32
>framebuffer_width=960
>framebuffer_height=160
>
>#Enable the lcd, enable custom display sizes with CVT, set as the default >output.
>enable_dpi_lcd=1
>dpi_group=2
>dpi_mode=87 # Hdmi CVT
>display_default_lcd=1
>
>dpi_output_format=458773
>hdmi_timings=960 0 40 48 88 160 0 13 3 32 0 0 0 60 0 32000000 3


5. Save the config.txt file as plain text and then open up cmdline.txt After rootwait (the last word on the first line) add a space and then
>modules-load=dwc2,g_ether

6. Enable SSH. Create a empty file named "ssh" on the boot partition:

>echo "" > ssh

7. Plug in your Pi with a USB Micro cable. Make sure to use the "USB" plug.

8. SSH to your Pi, connected as a network device:
>ssh pi@raspberrypi.local

9. Expand the filesystem using the raspyi tool
>sudo raspi-config

10. On your Pi, install and download few tools. Make sure to share internet with your Pi:
>sudo apt-get install device-tree-compiler
>
>wget https://raw.githubusercontent.com/NiklasFauth/pollin-lcd-dpi/master/pollin-dt-blob.dts

11. Creat a device-tree file that enables all the required GPIOs:
>sudo dtc -I dts -O dtb -o /boot/dt-blob.bin pollin-dt-blob.dts

12. Reboot.
