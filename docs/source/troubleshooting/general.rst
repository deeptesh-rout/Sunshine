General
=======

Forgotten Credentials
---------------------
If you forgot your credentials to the web UI, try this.
   .. tab:: General

      .. code-block:: bash

         sunshine --creds {new-username} {new-password}

   .. tab:: AppImage

      .. code-block:: bash

         ./sunshine.AppImage --creds {new-username} {new-password}

   .. tab:: Flatpak

      .. code-block:: bash

         flatpak run --command=sunshine dev.lizardbyte.app.Sunshine --creds {new-username} {new-password}


Web UI Access
-------------
Can't access the web UI?
   #. Check firewall rules.

Controller works on Steam but not in games
------------------------------------------
One trick might be to change Steam settings and check or uncheck the configuration to support Xbox/Playstation
controllers and leave only support for Generic controllers.

Also, if you have many controllers already directly connected to the host, it might help to disable them so that the
Sunshine provided controller (connected to the guest) is the "first" one. In Linux this can be accomplished on USB
devices by finding the device in `/sys/bus/usb/devices/` and writing `0` to the `authorized` file.

Packet loss (Buffer overrun)
----------------------------
If the host PC (running Sunshine) has a much faster connection to the network
than the slowest segment of the network path to the client device (running
Moonlight), massive packet loss can occur: Sunshine emits its stream in bursts
every 16ms (for 60fps) but those bursts can't be passed on fast enough to the
client and must be buffered by one of the network devices inbetween. If the
bitrate is high enough, these buffers will overflow and data will be discarded.

This can easily happen if e.g. the host has a 2.5 Gbit/s connection and the
client only 1 Gbit/s or Wifi. Similarly a 1 Gbps host may be too fast for a
client having only a 100 Mbps interface.

As a workaround the transmission speed of the host NIC can be reduced: 1 Gbps
instead of 2.5 or 100 Mbps instead of 1 Gbps. (A technically more advanced
solution would be to configure traffic shaping rules at the OS-level, so that
only Sunshine's traffic is slowed down.)

Packet loss (MTU)
-----------------
Albeit unlikely, some guests might work better with a lower `MTU
<https://en.wikipedia.org/wiki/Maximum_transmission_unit>`__ from the host. For example, a LG TV was found to have 30-60%
packet loss when the host had MTU set to 1500 and 1472, but 0% packet loss with a MTU of 1428 set in the network card
serving the stream (a Linux PC). It's unclear how that helped precisely so it's a last resort suggestion.
