# How I assembled my own Raspberry Pi based IP-KVM

## Overview
I have a small stack of computers at home for work-related testing. I work with build automation so I need access to physical equipment in order to properly test as virtualisation is handy, but not 100% accurate when it comes to OS build and configuration (thinking hardware issues, kernel modules, that sort of thing).

My small stack is connected on a separate VLAN, and uses a multi-port KVM switch to connect 4 devices to a single screen, keyboard and mouse. This is all great, if I'm at home. I obviously can't take this with me, so if I need to travel for any reason, I wanted to be able to access my lab. 

Enter the PiKVM (https://pikvm.org/).

This is a great piece of equipment.. but here in the UK it's a little on the expensive side for the hobbyist, and rarely in stock with the offical suppliers. There are alternative version (from Geekworm) but I wasn't sure of the quality and getting started with this setup required very little investment (I already had a Pi4 so just needed the HDMI-CSI bridge originally).

I thought I'd document my configuration and setup here, in case anyone finds it useful or I need to revisit it later.

## Items needed:
* Raspberry Pi 4 (2Gb model is sufficient: https://shop.pimoroni.com/products/raspberry-pi-4?variant=29157087412307)
* HDMI - CSI bridge (https://www.amazon.co.uk/dp/B08TBD68MH/)
* USB C Power splitter (https://www.tindie.com/products/8086net/usb-cpwr-splitter/)
* I2C OLED Screen (https://www.amazon.co.uk/dp/B07MHGPNVT/)
* Case (https://www.thingiverse.com/thing:5203114)
* Case screws (https://www.amazon.co.uk/dp/B01DIUJO14/)
* 40mm PWM 3-pin fan (https://www.amazon.co.uk/dp/B092ZF995F/)
* Short USB-C male2male cable (for inside the PiKVM case: https://www.amazon.co.uk/dp/B0DJ1LKDJH/)
* USB-C male to USB-A male (For connecting to the target device)
* USB-A male to USB-A micro male (optional, for connecting to a multi-port KVM)
* Multi-port KVM switch (optional, for controlling multiple devices: https://www.amazon.co.uk/dp/B095JN4CJY/)
* Cables for connecting the OLED display (I used GPIO cables I had lying around)
* SD Card (I used a 32Gb card to allow it to hold several ISO images)


## Physical Assembly

### OLED Screen
The OLED screen came bare, so I soldered the 4-pin connector so I could use the female to female GPIO cables. In hindsight I'd use right-angle pins (https://www.amazon.co.uk/dp/B01461DQ6S) on the OLED screen so it doesn't foul the internal cabling.

Connect the OLED display to the Raspberry Pi:
* GND - Pin 9
* VCC - Pin 1
* SDA - Pin 3
* SCL - Pin 5

### Raspberry Pi Power

You can power the Raspberry Pi directly from your host device, using a cable plugged into the power/OTG port on the Raspberry Pi... however if your host device loses power for some reason, then your PiKVM will also lose power... not what you might want.

If you power the Pi separately, but then connect a USB cable to your host device separately, then you might get power passed to the Raspberry Pi down the USB port and this could damage your Pi. The easiest way to avoid this is with a USB-C power splitter. This takes two inputs, one data and one power, and then passes them both on safely to your Pi power/OTG port by stripping the power lines from your `data` USB connection.

There's not a lot of room in the case and the distance from the USB-C port on the Pi to the power splitter isn't very far, so you need a really short, flexible cable. I struggled to find anything really short, so a 10cm cable had to work. I removed the braided outer with a sharp knife and I also removed some of the plastic strain relief to make the connectors shorter. Be careful here. The extra protection shouldn't be needed as the cable is inside the case, and with these parts removed, it fits a lot easier. Connect one end to the Raspberry Pi USB C port (power/OTG port) and the other end to the internal port for the USB-C power splitter.

### HDMI CSI bridge

Originally I used the Geekworm HDMI-CSI bridge (https://www.amazon.co.uk/gp/product/B0899L6ZXZ/) and it worked very well, however it doesn't physically fit into the case I had 3d printed. So, instead I ordered a new Waveshare bridge (https://www.amazon.co.uk/gp/product/B08TBD68MH/) and this works and fits just fine... although the cable is a bit long. Fit as per the instructions.

### Case
I had the case above 3D printed by a service in the UK (SurfaceScan.co.uk), I chose to use PETG rather than PLA to give it a little extra heat resistance with a layer thickness of 200.0 µm and 50% infill. Later I will learn what this means.

Assembling the items into the case takes a bit of doing as it's a snug fit. Try to keep the cables short enough if you can to increase airflow and make the likelyhood of trapping anything less.

Add the case fan (as below), then slot each of the components into the case in their appropriate slots. The OLED screen might give you some trouble and try to fall out of it's place (again, might be easier with right-angled connectors?). Maybe a small amount of blu-tack or sugaru would help to hold it. Once the spaghetti of wires is tamed, push the two pieces together and **gently** screw in the case screws. These will be tight, go gently to avoid cracking the case. There are no inserts in this, so repeated opening of the case will cause it to loosen.

### Case fan
Add the fan to the case using the supplied nuts and bolts (shorter bolts would be ideal but they didn't foul anything). Wire the pins to the Raspberry Pi as below:
* Red - Pin 4
* Black - Pin 6
* Blue - Pin 32

## Software

