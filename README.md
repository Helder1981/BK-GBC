# BK-GBC
Burger King Gameboy Color Toy PCBs for use with a Raspberry Pi Zero W and a SPI 2.0inch LCD linked below:<br>
https://www.aliexpress.com/item/QVGA320240-2-inch-SPI-serial-screen-320-240-12pin-ST7789V-2-0-inch-LCD/32906389474.html

Installation of Drivers (Run each command one line at a time in the terminal unless specified):

`sudo nano /etc/modules-load.d/fbtft.conf`<br>
add these 2 lines to the file<br>
`spi-bcm2835`<br>
`fbtft_device`<br>
Ctrl + O to save then Ctrl + X to exit

-------------------------------------------------

`sudo nano /etc/modprobe.d/fbtft.conf`<br>

copy entire code below into the opened file<br>

`options fbtft_device custom name=fb_st7789v speed=81000000 rotate=270 fps=60 buswidth=8  gpios=reset:25,dc:24 modprobe flexfb setaddrwin=0 width=240 height=320 init=-1,0x11,-2,120,-1,0x36,0x00,-1,0x3A,0x05,-1,0xB2,0x08,0x08,0x00,0x33,0x33,-1,0xB7,0x35,-1,0xBB,0x1A,-1,0xC2,0x01,-1,0xC3,0x3F,-1,0xC4,0x26,-1,0xC6,0x0E,-1,0xD0,0xA4,0xA1,-1,0x21,-1,0xE0,0x00,0x19,0x1E,0x0A,0x09,0x15,0x3D,0x44,0x51,0x12,0x03,0x00,0x3F,0x3F,-1,0xE1,0x00,0x18,0x1E,0x0A,0x09,0x25,0x3F,0x43,0x52,0x33,0x03,0x00,0x3F,0x3F,-1,0x29,-3`<br>
<br>
Ctrl + O to save then Ctrl + X to exit

-------------------------------------------------

Now it's time to install the FBCP software (enter 1 line at a time)

`sudo apt-get update`<br>
`sudo apt-get install cmake git`<br>
`git clone https://github.com/tasanakorn/rpi-fbcp`<br>
`cd rpi-fbcp/`<br>
`mkdir build`<br>
`cd build/`<br>
`cmake ..`<br>
`make`<br>
`sudo install fbcp /usr/local/bin/fbcp`<br>
<br>
Ctrl + O to save then Ctrl + X to exit

----------------------------------------------------

We need to add the fbcp to boot everytime to initiate the LCD so we are going to edit a file and add a line of code for this 

`sudo echo "/usr/local/bin/fbcp &" >> /etc/rc.local`

----------------------------------------------------


We need to edit the config.txt and add these lines at the end of the text file.

`sudo nano /boot/config.txt`<br><br>
`hdmi_force_hotplug=1`<br>
`hdmi_group=2`<br>
`hdmi_mode=87`<br>
`hdmi_cvt=640 480 60 1 0 0 0`<br>
`disable_overscan=1`<br>
`dtparam=spi=on`<br>
<br>
Ctrl + O to save then Ctrl + X to exit

Finally Reboot the Pi and see the LCD work by typing:
`sudo reboot -h now`
