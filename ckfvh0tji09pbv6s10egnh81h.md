## Qubino ZMNHJD1 and Home Assistant installation

# Qubino ZMNHJD1 and Home Assistant installation


The first device added to my Smart Home installation after the addition of the ZWave protocol is the [**Qubino ZMNHJD1**](https://www.amazon.co.uk/ZMNHJD1-Pilot-Module-Electric-Radiator/dp/B01HHCCVGY/ref=sr_1_1?ie=UTF8&qid=1541363277&sr=8-1&keywords=ZMNHJD1)module.

![Image for post](https://miro.medium.com/max/60/0*YtmUpzsaHP6Kl9mP.jpg?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/916/0*YtmUpzsaHP6Kl9mP.jpg" width="458" height="458" srcSet="https://miro.medium.com/max/552/0*YtmUpzsaHP6Kl9mP.jpg 276w, https://miro.medium.com/max/916/0*YtmUpzsaHP6Kl9mP.jpg 458w" sizes="458px"/></noscript>

This is a pilot wire module for an electric radiator working on the ZWave+ network, which means you can add a temperature sensor and use it to script the radiator control.
You can link this module to an electric radiator with 4 or 6 orders.

![Image for post](https://miro.medium.com/max/60/0*2JdxUAumH385qHvX.jpg?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/450/0*2JdxUAumH385qHvX.jpg" width="225" height="225"/></noscript>

![Image for post](https://miro.medium.com/max/60/0*LcA18KHK1bfeBwOc.jpg?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/1844/0*LcA18KHK1bfeBwOc.jpg" width="922" height="614" srcSet="https://miro.medium.com/max/552/0*LcA18KHK1bfeBwOc.jpg 276w, https://miro.medium.com/max/1104/0*LcA18KHK1bfeBwOc.jpg 552w, https://miro.medium.com/max/1280/0*LcA18KHK1bfeBwOc.jpg 640w, https://miro.medium.com/max/1400/0*LcA18KHK1bfeBwOc.jpg 700w" sizes="700px"/></noscript>

The Qubino will allow the remote control (via the ZWave signal) this part of your radiator and not directly the temperature… yes, for sure based on the order you can change the temperature.

# INSTALLATION

The global installation is really easy and the instructions you have with the Qubino module or internet can help you to reach your goal.

In the end, just link the power to the Qubino module on the “L” and “N” connectors and then the Pilote Wire to the “Q” one.

Yeah I know, I didn’t use the right cables… but I had nothing else at home and I wanted to use it as soon as possible.
In my case, I even added to the temperature sensor [ZMNHEA1](https://www.amazon.co.uk/Qubino-Temperature-Sensor-Z-Wave-ZMNHEA1/dp/B00R6FH410/ref=pd_sim_201_1?_encoding=UTF8&pd_rd_i=B00R6FH410&pd_rd_r=1f91ae0a-e070-11e8-a8ce-4d34901cd81c&pd_rd_w=heHcY&pd_rd_wg=SEkcB&pf_rd_i=desktop-dp-sims&pf_rd_m=A3P5ROKL5A1OLE&pf_rd_p=1e3b4162-429b-4ea8-80b8-75d978d3d89e&pf_rd_r=QFJ5KK9FF34ZK3SCTN8Q&pf_rd_s=desktop-dp-sims&pf_rd_t=40701&psc=1&refRID=QFJ5KK9FF34ZK3SCTN8Q) to the module.

# HOME ASSISTANT CONFIGURATION

![Image for post](https://miro.medium.com/max/60/0*WZlVVwLsW0inKbXn.png?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/2784/0*WZlVVwLsW0inKbXn.png" width="1392" height="464" srcSet="https://miro.medium.com/max/552/0*WZlVVwLsW0inKbXn.png 276w, https://miro.medium.com/max/1104/0*WZlVVwLsW0inKbXn.png 552w, https://miro.medium.com/max/1280/0*WZlVVwLsW0inKbXn.png 640w, https://miro.medium.com/max/1400/0*WZlVVwLsW0inKbXn.png 700w" sizes="700px"/></noscript>

Once completed you can synchronize the module and your ZWave installation simply with a click on the module button for 5 seconds (all the instructions are inside the module itself) and then a similar operation on your ZWave “server” (home assistant in my case).

![Image for post](https://miro.medium.com/max/60/0*5gxg2zW_ltxiOD97.png?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/2428/0*5gxg2zW_ltxiOD97.png" width="1214" height="244" srcSet="https://miro.medium.com/max/552/0*5gxg2zW_ltxiOD97.png 276w, https://miro.medium.com/max/1104/0*5gxg2zW_ltxiOD97.png 552w, https://miro.medium.com/max/1280/0*5gxg2zW_ltxiOD97.png 640w, https://miro.medium.com/max/1400/0*5gxg2zW_ltxiOD97.png 700w" sizes="700px"/></noscript>

Click on the **Add Node Secure** button in the ZWave configuration section to start the modules search.
If all worked well you will find the new Qubino device in the list of linked nodes.

# QUBINO CONFIGURATION

![Image for post](https://miro.medium.com/max/60/0*ygKR_s_ZzAyi8W3D.png?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/2500/0*ygKR_s_ZzAyi8W3D.png" width="1250" height="1008" srcSet="https://miro.medium.com/max/552/0*ygKR_s_ZzAyi8W3D.png 276w, https://miro.medium.com/max/1104/0*ygKR_s_ZzAyi8W3D.png 552w, https://miro.medium.com/max/1280/0*ygKR_s_ZzAyi8W3D.png 640w, https://miro.medium.com/max/1400/0*ygKR_s_ZzAyi8W3D.png 700w" sizes="700px"/></noscript>

The last remaining thing is the configuration to allow the usage through HAssio of your electric radiator.
The list of available sensors and commands, visible in the node panel of the Qubino device, should be like the following one:

**pilot_wire_level**: it is the value actually configured on the pilot wire which allows to identify the order set.
**pilot_wire_switch**: used to set the desired order sending a correct value on the pilot wire
**pilot_wire_temperature**: if you linked the temperature sensor, you will find the temperature value checking on this sensor
The other controls are useful only if you linked your qubino using the optional “buttons”

![Image for post](https://miro.medium.com/max/60/0*yCme20J1CVvAQ1vv.jpg?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/1568/0*yCme20J1CVvAQ1vv.jpg" width="784" height="593" srcSet="https://miro.medium.com/max/552/0*yCme20J1CVvAQ1vv.jpg 276w, https://miro.medium.com/max/1104/0*yCme20J1CVvAQ1vv.jpg 552w, https://miro.medium.com/max/1280/0*yCme20J1CVvAQ1vv.jpg 640w, https://miro.medium.com/max/1400/0*yCme20J1CVvAQ1vv.jpg 700w" sizes="700px"/></noscript>

**Yes, but what are the correct values to set for each order?**
In the following table, available on the net, you can imagine what do to…

Maybe… Ok, definitely not easy to know which are the right values. In my case, I spent several hours before to know what to put in the configuration (and once again, the internet was my friend) and a couple of months to be sure about the configuration because I installed it at the end of the summer and the radiator wasn’t working yet.

Here what I configured for my 6 orders radiator, into the **automations.yaml** file.


```
- alias: Set Qubino to Comfort 
   initial_state: 'off' 
   trigger: 
     platform: state 
     entity_id: input_select.qubino to: 'Comfort' 
     action: service: light.turn_on entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 100 id: 322e1962112842dab4defab990286212 - alias: Set Qubino to Comfort -1 initial_state: 'off' trigger: platform: state entity_id: input_select.qubino to: 'Comfort -1' action: service: light.turn_on entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 45 id: bb19039062934ca5ba4f26ead890b4ee - alias: Set Qubino to Comfort -2 initial_state: 'off' trigger: platform: state entity_id: input_select.qubino to: 'Comfort -2' action: service: light.turn_on entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 35 id: ee3069bd1f16476ea33ff4b1a875575a - alias: Set Qubino to Eco initial_state: 'off' trigger: platform: state entity_id: input_select.qubino to: 'Eco' action: service: light.turn_on entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 25 id: 8397b8f4cccd4dca90996ba38e760ba4 - alias: Set Qubino to Anti Freeze initial_state: 'off' trigger: platform: state entity_id: input_select.qubino to: 'Anti Freeze' action: service: light.turn_on entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 15 id: 04d972cb89ba4fce96b669c95e4e4e48 - alias: Set Qubino to Stop initial_state: 'off' trigger: platform: state entity_id: input_select.qubino to: 'Stop' action: service: light.turn_off entity_id: light.qubino_zmnhjd1_flush_dimmer_pilot_wire_level data: brightness: 0 id: 9d3e9fcf237449eaac1bd771e1509b0b
```


I then add a simple input to allow a quick configuration of the radiator status:


```
input_select: 
  qubino: 
    name: Qubino Modes 
    options: 
      - Off 
      - Anti-Freeze 
      - Eco 
      - Comfort -2 
      - Comfort -1 
      - Comfort
    initial: Comfort
```


![Image for post](https://miro.medium.com/max/60/0*LkwbPe4ik82Hluku.png?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/1880/0*LkwbPe4ik82Hluku.png" width="940" height="384" srcSet="https://miro.medium.com/max/552/0*LkwbPe4ik82Hluku.png 276w, https://miro.medium.com/max/1104/0*LkwbPe4ik82Hluku.png 552w, https://miro.medium.com/max/1280/0*LkwbPe4ik82Hluku.png 640w, https://miro.medium.com/max/1400/0*LkwbPe4ik82Hluku.png 700w" sizes="700px"/></noscript>

Which is giving as result a combo box with the allowed values for your Qubino.

![Image for post](https://miro.medium.com/max/58/0*yFRDa8ppqqo0MbDM.png?q=20)

<noscript><img alt="Image for post" class="t u v ha aj" src="https://miro.medium.com/max/1980/0*yFRDa8ppqqo0MbDM.png" width="990" height="1022" srcSet="https://miro.medium.com/max/552/0*yFRDa8ppqqo0MbDM.png 276w, https://miro.medium.com/max/1104/0*yFRDa8ppqqo0MbDM.png 552w, https://miro.medium.com/max/1280/0*yFRDa8ppqqo0MbDM.png 640w, https://miro.medium.com/max/1400/0*yFRDa8ppqqo0MbDM.png 700w" sizes="700px"/></noscript>

And then for sure, you can add the temperature sensor somewhere in your interface. A complete configuration using all the commands can be something like the following:

If you want you can find the whole configuration I’m using at home on my GitHub repository: [https://github.com/mmornati/home-assistant-config](https://github.com/mmornati/home-assistant-config)

Are you ready for winter? :)



_Originally published at_ [_https://blog.mornati.net_](https://blog.mornati.net/qubino-device-for-electric-radiator/) _on November 4, 2018._