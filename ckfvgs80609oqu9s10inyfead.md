## PushBullet notifications for Kerberos.io

<span class="s"></span>

# PushBullet notifications for Kerberos.io


[Kerberos.io](https://kerberos.io/) is a cheap video surveillance system (the cheapest on the market I think) that you can install on a RaspberryPI and using it with an (old) USB camera, an IP camera, or with the RPi camera.

![Image for post](https://miro.medium.com/max/34/0*k5Dygy7yzMbyimjt.jpg?q=20)

<noscript><img alt="Image for post" class="t u v gz aj" src="https://miro.medium.com/max/370/0*k5Dygy7yzMbyimjt.jpg" width="185" height="329"/></noscript>

In Kerberos.io you can then configure different kinds of notification when motion is detected.

One of the available methods is the **WebHook** and, using this configuration, I implemented an [Hook project](https://github.com/mmornati/kerberosio-hooks) (extensible by plugins… WIP) which allows actually to have notifications on PushBullet.
The information about the installation and configuration is available on the project README file.

If you want to make a simple test before linking the hook to Kerberos.io, or to debug if anything is not working good, you can make a post-call to the hook project using, for example, Postman

![Image for post](https://miro.medium.com/max/60/0*FQToXvq79WQCGr0L.png?q=20)

<noscript><img alt="Image for post" class="t u v gz aj" src="https://miro.medium.com/max/1930/0*FQToXvq79WQCGr0L.png" width="965" height="356" srcSet="https://miro.medium.com/max/552/0*FQToXvq79WQCGr0L.png 276w, https://miro.medium.com/max/1104/0*FQToXvq79WQCGr0L.png 552w, https://miro.medium.com/max/1280/0*FQToXvq79WQCGr0L.png 640w, https://miro.medium.com/max/1400/0*FQToXvq79WQCGr0L.png 700w" sizes="700px"/></noscript>

The result should be a Pushbullet message on all your linked devices, or on the device, you selected to notify, with the image taken by the Kerberos.io camera.

![Image for post](https://miro.medium.com/max/34/0*sMY1of0rLWMHf6IH.png?q=20)

<noscript><img alt="Image for post" class="t u v gz aj" src="https://miro.medium.com/max/466/0*sMY1of0rLWMHf6IH.png" width="233" height="414"/></noscript>

The plugin system of this project is already developed, and I planned to add other notifications like mail and TextMessage. Before I said it is WIP ’cause I’d like to improve this part forcing the way to develop the plugins with Interfaces and predefined functions.

All is Open… if you test/use it and you have any comments improvements, don’t hesitate to post comments (here or better on [GitHub](https://github.com/mmornati/kerberosio-hooks)).



_Originally published at_ [_https://blog.mornati.net_](https://blog.mornati.net/pushbullet-notifications-for-kerberos-io/) _on September 27, 2016._