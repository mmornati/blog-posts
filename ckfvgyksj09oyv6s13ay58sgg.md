# Install Z-Wave Plus Z-Stick GEN5 — Aeon Labs in Home Assistant (RPi version)

# Install Z-Wave Plus Z-Stick GEN5 — Aeon Labs in Home Assistant (RPi version)

In this post, you can find how you can configure a Z-Wave device in your Home Assistant smart home controller and how you can then create your network adding new Z-Wave devices.

Some months ago I decided to look for a Smart Home box to be ready to add devices to my home (until there I had only Philips Hue devices). In the end, I choose (because I’m a geek? :)) [Home Assistant](https://www.home-assistant.io/): it is OpenSource and easily extensible just adding devices on the computer on which you installed HAssio (in my case is my old RPi 2).

Winter is coming and I started making my home climate-smart: I have a central boiler and for the attic (where there is my daughter room) we have an electric heater. So: let’s start with this latest one. After a long time looking around to choose which device and which protocol to use I decided to go with Z-Wave (I will talk about the heater Z-Wave device in another post) and to use it I added to my Raspberry PI a Z-Wave Stick: [Z-Stick GEN5 — Aeon Labs](https://www.amazon.fr/Contr%C3%B4leur-Z-Wave-Plus-Z-Stick-GEN5/dp/B00YETCNOE/ref=asc_df_B00YETCNOE/?tag=googshopfr-21&linkCode=df0&hvadid=51050688126&hvpos=1o1&hvnetw=g&hvrand=5946787651369239016&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9040859&hvtargid=pla-124038450486&psc=1).

It is automatically recognized by Home Assistant and you can configure it very quickly. Let’s check together how you can install it.

# Detect the Device name

To be ready to configure the USB stick you need before to identify the name/path of your Stick. It can be different based on your hardware.

**First Method**

* Connect the USB Stick to the RPi
    
* Connect to the RPi/Hassio via SSH
    
* Run the `dmesg` command You should see an output like the following:
    

```yaml
[ 5.227539] cdc_acm 1-1.5:1.0: ttyACM0: USB ACM device [ 5.229467] usbcore: registered new interface driver cdc_acm [ 5.229487] cdc_acm: USB Abstract Control Model driver for USB modems and ISDN adapters</span>
```

Which is giving you the name of the USB Stick: `ttyACM0` (and the device path `/dev/ttyACM0`

**Second Method**

* Connect the USB Stick to the RPi
    
* Connect to the RPi/Hassio via SSH
    
* Run `hassio hw info`. This command is giving the information of the full hw of your PC
    

```yaml
{ "result": "ok", "data": { "serial": [ "/dev/ttyAMA0", "/dev/ttyACM0" ], "input": [], "disk": [], "gpio": [ "gpiochip0" ], "audio": { "0": { "name": "bcm2835 - bcm2835 ALSA", "type": "ALSA", "devices": { "0": "digital audio playback", "1": "digital audio playback" } } } } }</span>
```

This one is giving you the list of USB devices (two in my case) so you should know before to link the USB stick the name of what you already have.

# Configure Home Assistant

Once you have the information about your USB Stick you are ready to configure your Home Assistant installation. As you can see in the [documentation](https://www.home-assistant.io/docs/z-wave/installation/), it is quite simple. Add the following line (take care only to the network\_key property !) to your *configuration.yml* file

```yaml
zwave: usb_path: /dev/ttyACM0 debug: true network_key: "0xA5, 0xDC, 0x21, 0xE1, 0xB6, 0x1D, 0x26, 0xAA, 0x15, 0x47, 0xE9, 0x12, 0x9F, 0x46, 0x1D, 0x5A"</span>
```

The usb\_ *path* is the one you discovered in the previous step.

The *network\_key* is useful and recommended to secure your Z-Wave installation.

> *Security Z-Wave devices require a network key before being added to the network using the Add Secure Node button in the Z-Wave Network Management card. You must set the network\_key configuration variable to use a network key before adding these devices.*

As shown into the documentation you can simply generate a random one with a cli command:

```yaml
$ cat /dev/urandom | tr -dc '0-9A-F' | fold -w 32 | head -n 1 | sed -e 's/\(..\)/0x\1, /g' -e 's/, $//'</span>
```

All is done. You just need to restart your Hassio Host and all should work. You will have a new ZWave entry into the configuration menu.

![Image for post](https://miro.medium.com/max/60/0*ZLCcODuPFB9Nm3eY.png?q=20 align="left")

![Image for post](https://miro.medium.com/max/1326/0*ZLCcODuPFB9Nm3eY.png align="left")

# Add a new ZWave Device

Add new devices to your Z-wave network is really easy and it can be done directly in the Hassio configuration menu.

![Image for post](https://miro.medium.com/max/60/0*isFu2iZHZa76ZQsl.png?q=20 align="left")

![Image for post](https://miro.medium.com/max/1502/0*isFu2iZHZa76ZQsl.png align="left")

Put your new device in “discovery” mode: normally should be enough to start it up but, in case, there is a button on the device to force a network discover.

Then click on **Add Secure Node** button into Hassio. You should see the new node(s) into the Nodes combo box.

![Image for post](https://miro.medium.com/max/60/0*mWdZDEtYXGdpcQTm.png?q=20 align="left")

![Image for post](https://miro.medium.com/max/1228/0*mWdZDEtYXGdpcQTm.png align="left")

All is done. Enjoy your new Z-Wave network.

*Originally published at* [*https://blog.mornati.net*](https://blog.mornati.net/install-zwave-home-assistant/) *on September 11, 2018.*