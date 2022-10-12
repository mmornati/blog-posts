# HomeAssistant: detect washing machine cycle completion

Washing machines or whatever, there are some devices in our houses that are located far from the living places. So, how to know if it is the time to take of them without checking every 5 minutes? I know, we have a clock and that should be enough, but if you have HomeAssitant you can easily configure an action to notify you when the washing machine cycle is completed.

## Track the consumption
If your washing machine is not connected by itself, we can track it using the plug with power consumption and on the market today there are a lot of devices proposing this function.

### WiFi
Without any other protocol, you can buy a WiFi power adapter with a power meter. On my side, I've a couple of [Konyks](https://konyks.com/produit/priska-mini-3-fr/?gclid=CjwKCAjwqJSaBhBUEiwAg5W9p7qvtkgwNDRcKtSsP5e6uwBqJx7M1h0s6GhbAY79qprzi3zuuL1cJRoC3A8QAvD_BwE) 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665514709478/QTMqoASet.png align="left")

The advanced, more than the protocol, is that it usually this kind of device is quite cheaper. The less interesting part is that you need the WiFi signal in the location you installed your washing machine.  

You can control them in the Tuya/Konyks application or add them to HomeAssistant, which is allowing to have all the start/stop and meter information.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665514818320/JoFOSUy18.png?height=300 align="left")

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665514831148/dpBZg-7k_.png?height=200 align="left")

### ZigBee
If you are using this protocol (the one proposed by Philips Hue) and you would like to use its powerful mesh function there are lot of alternatives proposing it with a price remaining quite low.
For example the [Innr Smart Plug](https://www.innr.com/fr/produit/smart-plug-zigbee30/), I'm using it too and working very well.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665515113654/Ub3kYaHvB.png align="left")

It exposes the same information as the WiFi Tuya One.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665515097877/uEOdJ_LBP.png?height=400 align="left")

I would suggest this kind of device if you already have a lot of other ZigBee devices helping you to extend the signal and reaching easily any remote corner of your home.

### ZWave
It is proposing the same benefits as the ZigBee one, but as constructors need a license to produce devices using this protocol, the final price is very high compared to the others.
Fibaro is producing very good ZWave devices. I used one of them in the last 4 years and it is excellent in everything. It has much more functionalities and configurations available than the two others we have just seen.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665515421826/IXinP8KbX.png?height=400 align="left")

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665515472659/axfvGjJsf.png?height=150 align="left")

**NOTE/WARNING**
Just a thing to take in count when you chose your power plug, never mind the protocol you will use, is the max power the plug can provide (in Watt). If you link a high-consumer device which is requiring more than the max available from the plug, the plug itself will shutdown the device considering it has a problem. I had this for several weeks using the Fibaro ZWave plug on the washing machine, before understanding that it was not the good plug for what I wanted 😅

## Configuring the Notification
Once your washing machine's power consumption is monitored with a power plug you have everything you need to configure Home Assistant. 

First of all, we need to create a `binary_sensor` we will use to detect if the device is working or not. All the plug we have just seen have a ON/OFF information, but this is saying you if the plug is allowing the device to work (giving the power) or not. Is not saying if it is really working.

In your `configuration.yaml` you can create the binary_sensor like the following

```
binary_sensor:
  - platform: template
    sensors:
      washing_machine:
        value_template: "{{ states('sensor.machine_a_laver_power_consumption') | float > 10.0 }}"
        delay_on: 0:00:30
        delay_off: 0:00:30
```

This is saying to check the `sensor.machine_a_laver_power_consumption`, when it is over 10W for more than 30 seconds the binary_sensor is considered `on`. When is moved down to 0 for more than 30 seconds it will be back to `off`.

Then, just using the binary_sensor you can create an automation to do "what you want" when it go on or off. An example to be notified:

```
- id: washing_machine_working
  alias: Machine à laver en fonction
  trigger:
    - entity_id: binary_sensor.washing_machine
      from: "off"
      platform: state
      to: "on"
  action:
    - service: notify.notify
      data:
        message: "Machine à laver en marche {{ states('sensor.machine_a_laver_power_consumption') }}."

- id: washing_machine_finished
  alias: Machine à laver Cycle Terminé
  trigger:
    - entity_id: binary_sensor.washing_machine
      from: "on"
      platform: state
      to: "off"
  action:
    - service: notify.notify
      data:
        message: "Cycle machine à laver terminé."
```

With this, just keeping your phone you will get notifications when the washing machine is starting and once it finish the cycle.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665516635759/r1cblc8MM.png?height=300 align="left")

That's all. You can now imagine some cool Automation with your controlled devices... smart home is very cool and it could help to optimise your consumption!



