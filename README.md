# R051 Mod
This repository provides you on how to modify your untouched SMART BRO/PLDT prepaid home wifi.

# WARNING
You should have experience with:

- Flashing firmware just in case you fuck up the modem
- Knows the basic usage of Linux and UBoot.

Or else, the modem will be rendered useless; so take risk.

# Required Equipment and Accessories

- Linux is preferred here, Unix based system is okay but Windows just sucks!

- For serial communication, you need only one of these following:
  
  1. USB to TTL serial adapter to be plugged in to usb port of your PC, the very cheapest of all.

  2. Single Board Computers with GPIO pins, any model and brands (RPi, OPi, Banana Pi, LubanCat , etc).

  3. Microcontrollers, any model and brands (Pico, Arduino, NodeMCU, etc) to be also plugged in to USB of your PC.

- DuPont Wire and 3 GPIO header to be soldered to the modem

# Storage Space Limitations

R051 modem has a very limited of usable storage space if you intended to install additional programs within R051 modem as it has only `1.4MB` of total space, and it used about more than `300k` with original firmware installed. So basically you have to install what you only need.

# SSH/Telnet Access

R051 also let's you access the internal system via SSH (if available) and Telnet, but only if the R051 is not bricked. The usual credential is:
`root` and a password `$Passworld143`

# Sections

- [Communicating with Serial](/docs/serial.md)

- [Forwarding Internet from ISP to Modem](/docs/forward.md)

- [Recovery - For bricked R051](/docs/bricked.md)
