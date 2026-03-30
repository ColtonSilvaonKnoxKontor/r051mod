# Setting Up the Serial Console

Before anything else, you need to configure your PC to communicate with modem via serial UART and to host a backup or modded firmware. To do this, first we need to install these following: `screen ,tftp, tftpd-hpa` for debian-based or `tftp-server` if you are a fedora based user.

## TFTP Configuration

1. Create a file named tftpd-hpa using nano:
`sudo nano /etc/default/tftpd-hpa`

2. Paste this:

```
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/srv/tftp"
TFTP_ADDRESS=":69"
TFTP_OPTIONS="--secure --create"
```

3. Next are these:

`sudo chown -R tftp:tftp  /srv/tftp`


4. And then you may download the firmware by [rapain](https://github.com/rapain/r051). For simplicity of the filename, you may rename it to `firmware.bin`

5. Then copy the .bin firmware file to the root directory of `/srv/tftp` as sudo.

6. And then do these:

`sudo chown tftp:tftp /srv/tftp/firmware.bin`

`sudo chmod 644 /srv/tftp/firmware.bin`

`sudo systemctl restart tftpd-hpa`

# Serial Configuration

Before configuring the serial, we need to solder the r051 serial pins by first disassembling the unit, and find the serial port located at the front facing motherboard and at the left where you will see 4 punch out pins, with one square and three circles. If you have 3 GPIO header, only solder them to the circle, never do that in square because that is a VCC and we should not power the motherboard directly to it.

Then connect the serial pins to the target machine. The RX and TX must be cross connected. Refer to the pinout below:

```
ROUTER                |  CLIENT MACHINE
1st Square - VCC, 3V  |  DO NOT CONNECT!
1st Circle - RX       |  TX
2nd Circle - TX       |  RX
3rd Circle - GND      | GND
```

Next, we need to determine the tty* of your system depending on what machine or accessories you are using. The easiest is `dmesg | grep tty` where it lists out the available serial device. Usually for USB-to-TTL adapter, it uses `/dev/ttyUSB*` while most SBCs uses `/dev/ttyS*`. It might uses `/dev/ttyACM*`.

After we determined the serial device, now execute this:

`screen /dev/tty* 57600`

And plug the R051 DC adapter to power on the unit, and look at the output. If you read the output, it means it works; if there is no output, change the `*` from `0` to `1` or up until it shows the output.

# Basic `screen` usage

Use `ctrl + A` for every operation below:

To exit, `k`
To scroll up and down, `esc`
