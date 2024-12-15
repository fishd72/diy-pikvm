# How I assembled my own Raspberry Pi based IP-KVM

## Overview
I have a small stack of computers at home for work-related testing. I work with build automation so I need access to physical equipment in order to properly test as virtualisation is handy, but not 100% accurate when it comes to OS build and configuration (thinking hardware issues, kernel modules, that sort of thing).

My small stack is connected on a separate VLAN, and uses a multi-port KVM switch to connect 4 devices to a single screen, keyboard and mouse. This is all great, if I'm at home. I obviously can't take this with me, so if I need to travel for any reason, I wanted to be able to access my lab. 

Enter the [PiKVM](https://pikvm.org/).

This is a great piece of equipment.. but here in the UK it's a little on the expensive side for the hobbyist, and rarely in stock with the offical suppliers. There are alternative version (from Geekworm) but I wasn't sure of the quality and getting started with this setup required very little investment (I already had a Pi4 so just needed the HDMI-CSI bridge originally).

I thought I'd document my configuration and setup here, in case anyone finds it useful or I need to revisit it later. I found all of this information online on a number of websites, so I'm collating here for convenience.

## Items needed:
* [Raspberry Pi 4](https://shop.pimoroni.com/products/raspberry-pi-4?variant=29157087412307) (2Gb model is sufficient)
* [HDMI - CSI bridge](https://www.amazon.co.uk/dp/B08TBD68MH/)
* [USB C Power splitter](https://www.tindie.com/products/8086net/usb-cpwr-splitter/)
* [I2C OLED Screen](https://www.amazon.co.uk/dp/B07MHGPNVT/)
* [Case](https://www.thingiverse.com/thing:5203114)
* [Case screws](https://www.amazon.co.uk/dp/B01DIUJO14/)
* [40mm PWM 3-pin fan](https://www.amazon.co.uk/dp/B092ZF995F/)
* [Short USB-C male2male cable](https://www.amazon.co.uk/dp/B0DJ1LKDJH/) (for inside the PiKVM case)
* USB-C male to USB-A male (For connecting to the target device)
* USB-A male to USB-A micro male (optional, for connecting to a multi-port KVM)
* [Multi-port KVM switch](https://www.amazon.co.uk/dp/B095JN4CJY/) (optional, for controlling multiple devices)
* [Smart Power Strip](https://www.amazon.co.uk/dp/B0DBQ26ZNL) (optional, in place of ATX controls, can use this to restart Mini-PCs)
* Cables for connecting the OLED display (I used GPIO cables I had lying around)
* SD Card (I used a 32Gb card to allow it to hold several ISO images)


## Physical Assembly

### OLED Screen

<img src="https://m.media-amazon.com/images/I/61NS3MQntFL._AC_SL1200_.jpg" width="200" />

The OLED screen came bare, so I soldered the 4-pin connector so I could use the female to female GPIO cables. In hindsight I'd use [right-angle pins](https://www.amazon.co.uk/dp/B01461DQ6S) on the OLED screen so it doesn't foul the internal cabling.

**EDIT:** I relented and added the right angle connector. This worked much better.

Connect the OLED display to the Raspberry Pi:
* GND - Pin 9
* VCC - Pin 1
* SDA - Pin 3
* SCL - Pin 5

### Raspberry Pi Power

<img src="https://cdn.tindiemedia.com/images/resize/Fpc0GxVPoO_GJwMBvluj-nA2qvo=/p/fit-in/653x435/filters:fill(fff)/i/820311/products/2020-12-10T10%3A16%3A20.202Z-USB-C_PWR%20Splitter5V-2.jpg?1607566597" width="200" />  

You can power the Raspberry Pi directly from your host device, using a cable plugged into the power/OTG port on the Raspberry Pi... however if your host device loses power for some reason, then your PiKVM will also lose power... not what you might want.

If you power the Pi separately, but then connect a USB cable to your host device separately, then you might get power passed to the Raspberry Pi down the USB port and this could damage your Pi. The easiest way to avoid this is with a USB-C power splitter. This takes two inputs, one data and one power, and then passes them both on safely to your Pi power/OTG port by stripping the power lines from your _data_ USB connection.

There's not a lot of room in the case and the distance from the USB-C port on the Pi to the power splitter isn't very far, so you need a really short, flexible cable. I struggled to find anything really short, so a 10cm cable had to work. I removed the braided outer with a sharp knife and I also removed some of the plastic strain relief to make the connectors shorter. Be careful here. The extra protection shouldn't be needed as the cable is inside the case, and with these parts removed, it fits a lot easier. Connect one end to the Raspberry Pi USB C port (power/OTG port) and the other end to the internal port for the USB-C power splitter.

### HDMI CSI bridge

<img src="https://m.media-amazon.com/images/I/610O5VfhWML._AC_SX679_.jpg" width="200" />  

Originally I used the [Geekworm HDMI-CSI bridge](https://www.amazon.co.uk/gp/product/B0899L6ZXZ/) and it worked very well, however it doesn't physically fit into the case I had 3d printed. So, instead I ordered a new [Waveshare bridge](https://www.amazon.co.uk/gp/product/B08TBD68MH/) and this works and fits just fine... although the cable is a bit long. Fit as per the instructions. 

**EDIT:** I see [thepihut](https://thepihut.com/search?q=Flex+Cable+for+Raspberry+Pi+Camera&narrow_by=&sort_by=relevency&page=1) do 50mm or 100mm FPC cables (instead of the stock 150mm one) so I might try these.

### Case

<img src="https://cdn.thingiverse.com/assets/8d/19/c3/f3/f7/large_display_pikvm-case_2022-jan-16_07-36-58pm-000_customizedv.png" width="200" />  

I had the case above 3D printed by a service in the UK (SurfaceScan.co.uk), I chose to use PETG rather than PLA to give it a little extra heat resistance with a layer thickness of 200.0 µm and 50% infill. Later I will learn what this means.

Assembling the items into the case takes a bit of doing as it's a snug fit. Try to keep the cables short enough if you can to increase airflow and make the likelyhood of trapping anything less.

Add the case fan (as below), then slot each of the components into the case in their appropriate slots. The OLED screen might give you some trouble and try to fall out of it's place (again, might be easier with right-angled connectors?). Maybe a small amount of blu-tack or sugaru would help to hold it. Once the spaghetti of wires is tamed, push the two pieces together and **gently** screw in the case screws. These will be tight, go gently to avoid cracking the case. There are no inserts in this, so repeated opening of the case will cause it to loosen.

### Case fan

<img src="https://m.media-amazon.com/images/I/51nl7x3rx-S._SL1000_.jpg" width="200" />  

Add the fan to the case using the supplied nuts and bolts (shorter bolts would be ideal but they didn't foul anything). Wire the pins to the Raspberry Pi as below:
* Red - Pin 4
* Black - Pin 6
* Blue - Pin 32

**EDIT:** I replaced the stock M3 x 16 silver screws that came with the fans with M3 x 12 black screws, these do not protrude from the fan shroud so make it a bit easier inside for cabling.

### Connections

1. Connect the Pi to your network using an ethernet cable and the regular Pi ethernet jack (you can use WiFi, but ethernet is faster and more stable)
2. Connect an HDMI cable from your device (or the output of the HDMI KVM if you're using one) to the *input* of the HDMI-CSI bridge
3. Connect a USB-A male to USB-C male cable to a USB port on your host device (or the **front** USB port of the HDMI KVM if you're using one) to the DATA port on the USB-C power splitter
4. (Optional) Connect the USB-A male to USB-A Micro cable between a USB 2 port on your Pi and the OTG port on the HDMI KVM
5. Finally, connect the USB-C power supply to the POWER port of the USB-C power splitter. This will power up the device


## Software

First, grab the DIY V2 image from the PiKVM website (https://docs.pikvm.org/flashing_os/). Flash this to your SD card using something like [Raspberry Pi Imager](https://github.com/raspberrypi/rpi-imager/releases) or [Etcher](https://etcher.balena.io/).

Then follow the [**First Steps** guide](https://docs.pikvm.org/first_steps/) from the PiKVM website, this will get the basics of your device up and running. 
>[!IMPORTANT]
>**BE SURE TO CHANGE YOUR PASSWORDS!**

### OLED Screen

To get the OLED screen working, we need to change a couple of things. 

1. Login to your PiKVM either via the website and then the console option, or via SSH if you've enabled it 
2. Change to root using: ```su root```
3. Change the filesystem to read-write mode with the command: ```rw```
4. As per the instructions at [this site](https://windix.medium.com/pikvm-with-raspberry-pi-zero-2-w-b5f9d4a6ff32) I added ```dtparam=i2c_arm=on``` on a new line within `/boot/config.txt`
5. I also added ```i2c-dev``` to a new line within `/etc/modules-load.d/kvmd.conf`
6. (Optional) If you need to rotate the output of the OLED, you can do the [following](https://elforastero.dev/blog/pikvm#wiring-up-the-display):
   - ```mkdir -p /etc/systemd/system/kvmd-oled.service.d```
   - ```
     echo '[Service]
     ExecStart=
     ExecStart=/usr/bin/kvmd-oled --height=32 --clear-on-exit --rotate=2' >> /etc/systemd/system/kvmd-oled.service.d/override.conf
     ```
7. Enable the display service by running: ```systemctl enable --now kvmd-oled kvmd-oled-reboot kvmd-oled-shutdown``` (ignore any errors, it will work after a reboot)
8. Switch to read-only mode using the command: ```ro```
9. Reboot the device

### PWM controlled fan

If you've wired the fan as I listed above, the below should be all you need to do. If you've used different GPIO pins or want different fan speeds or temperature thresholds, then the official documentation or [this site](https://geekworm.com/community/forum/topic/119225/kvmd-fan-not-functional-with-pikvm-a3) might help.

1. Login to your PiKVM either via the website and then the console option, or via SSH if you've enabled it 
2. Change to root using: ```su root```
3. Change the filesystem to read-write mode with the command: ```rw```
4. Run ```systemctl enable --now kvmd-fan```
6. Switch to read-only mode using the command: ```ro```

### Set server metadata

The webUI is likely showing your device name as `localhost@localdomain`. You can personalise this by:

1. Login to your PiKVM either via the website and then the console option, or via SSH if you've enabled it 
2. Change to root using: ```su root```
3. Change the filesystem to read-write mode with the command: ```rw```
4. Edit the file: ```nano /etc/kvmd/meta.yaml``` and change the info after `host:` to reflect what you want
5. Switch to read-only mode using the command: ```ro```
6. Restart the service using ```systemctl restart kvmd```

### (Optional) HDMI KVM Control

If you're using the HDMI KVM to remote control multiple devices, and you have a **supported** KVM device, you can use the PiKVM menu to control the port selection remotely.

Instructions are found in the [official documentation](https://docs.pikvm.org/multiport/) but basically:
1. Login to your PiKVM either via the website and then the console option, or via SSH if you've enabled it 
2. Change to root using: ```su root```
3. Change the filesystem to read-write mode with the command: ```rw```
4. Edit the file: ```nano /etc/kvmd/override.yaml``` and include the following. Note the assumption that the KVM switch is present on `/dev/ttyUSB0`:
   ```
   kvmd:
    gpio:
        drivers:
            ez:
                type: ezcoo
                protocol: 2
                device: /dev/ttyUSB0
        scheme:
            ch0_led:
                driver: ez
                pin: 0
                mode: input
            ch1_led:
                driver: ez
                pin: 1
                mode: input
            ch2_led:
                driver: ez
                pin: 2
                mode: input
            ch3_led:
                driver: ez
                pin: 3
                mode: input
            ch0_button:
                driver: ez
                pin: 0
                mode: output
                switch: false
            ch1_button:
                driver: ez
                pin: 1
                mode: output
                switch: false
            ch2_button:
                driver: ez
                pin: 2
                mode: output
                switch: false
            ch3_button:
                driver: ez
                pin: 3
                mode: output
                switch: false
        view:
            table:
                - ["#Input 1", ch0_led, ch0_button]
                - ["#Input 2", ch1_led, ch1_button]
                - ["#Input 3", ch2_led, ch2_button]
                - ["#Input 4", ch3_led, ch3_button]
   ```
5. Switch to read-only mode using the command: ```ro```
6. Restart the service using ```systemctl restart kvmd```

### (Optional) Remove ATX Controls

<img src="https://docs.pikvm.org/webui/ATX.jpg" width="200" />  

As I'm using a number of devices via the HDMI KVM, and my equipment doesn't support ATX controls, I've disabled them in the WebUI to avoid distractions. I found the instructions [here](https://www.dzombak.com/blog/2021/11/PiKVM-Build.html) but be sure to correctly format the resulting file. Incorrect edits will show unexpected results.

1. Login to your PiKVM either via the website and then the console option, or via SSH if you've enabled it. 
2. Change to root using: ```su root```
3. Change the filesystem to read-write mode with the command: ```rw```
4. Edit the file: ```nano /etc/kvmd/override.yaml``` and include the following.
   ```
   kvmd:
    atx:
        type: disabled
   ```
5. Switch to read-only mode using the command: ```ro```
6. Restart the service using ```systemctl restart kvmd```

> [!WARNING]
> If you've already added the HDMI KVM switching above, do **not** add `kvmd:` a second time... the full file should look like this:
   ```
   kvmd:
    gpio:
        drivers:
            ez:
                type: ezcoo
                protocol: 2
                device: /dev/ttyUSB0
        scheme:
            ch0_led:
                driver: ez
                pin: 0
                mode: input
            ch1_led:
                driver: ez
                pin: 1
                mode: input
            ch2_led:
                driver: ez
                pin: 2
                mode: input
            ch3_led:
                driver: ez
                pin: 3
                mode: input
            ch0_button:
                driver: ez
                pin: 0
                mode: output
                switch: false
            ch1_button:
                driver: ez
                pin: 1
                mode: output
                switch: false
            ch2_button:
                driver: ez
                pin: 2
                mode: output
                switch: false
            ch3_button:
                driver: ez
                pin: 3
                mode: output
                switch: false
        view:
            table:
                - ["#Input 1", ch0_led, ch0_button]
                - ["#Input 2", ch1_led, ch1_button]
                - ["#Input 3", ch2_led, ch2_button]
                - ["#Input 4", ch3_led, ch3_button]
    atx:
        type: disabled
   ```

   
