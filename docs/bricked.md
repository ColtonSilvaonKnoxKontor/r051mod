# Reflash firmware for Bricked unit

This section helps you recover your bricked R051 unit. Before proceeding with the process of unbricking the unit, you need to [setting up the serial UART](/docs/serial.md) where it also covers about downloading the firmware. This section only shows on how to unbrick the unit due to firmware corruption.

# Configure The Network

1. To be able to transmit the firmware going to R051, we need to configure the Host PC network via Static address of `10.10.10.3`. It is recommended to use secondary eth port if your PC has one, or use USB to ETH adapter, so that you don't need to mess up the main configuration of your primary ethernet port (If you are currently using it), else if you love to mess up everything, then go.

> Some units has different server and device IP, so you may see it via powering on the unit, and then after it shows:
> ```
> Please choose the operation: 
>  1: Load system code to SDRAM via TFTP. 
>  2: Load system code then write to Flash via TFTP. 
>  3: Boot system code via Flash (default).
>  4: Entr boot command line interface.
>  6: spi flash mcp test.
>  7: Load Boot Loader code then write to Flash via Serial. 
>  9: Load Boot Loader code then write to Flash via TFTP.                                                                   
>```
>
> Select `4` until it shows: `MT7628 #`
>
> And then enter the command `printenv` to output:
>```
> MT7628 # printenv
> bootcmd=bootp; setenv bootargs root=/dev/nfs nfsroot=${serverip}:${rootpath} ip=${ipaddr}:${serverip}:${gatewayip}:${netmask}:${hostname}::off; bootm
> bootdelay=5
> baudrate=115200
> ethaddr=00:AA:BB:CC:DD:10
> ipaddr=10.10.10.123
> serverip=10.10.10.3
> bootargs=console=ttyS1,57600n8 root=/dev/mtdblock5
> load_firmware_addr=0xbc050000
> wifi_ate=on
> stdin=serial
> stdout=serial
> stderr=serial
>
> Environment size: 378/4092 bytes
>```

3. If you don't know how to do this, execute this first:

```
ip a
```

to know the interface name of your ethernet network. For generic naming it should show `eth0` for primary, or `eth1` if you have adapter.

So in this example, we are using the interface name as `enx00e04c480251` since we are using USB to ETH adapter.

3. Execute the Following:

`sudo ip addr flush dev enx00e04c480251`

`sudo ip addr add 10.10.10.3/24 dev enx00e04c480251`

`sudo ip link set enx00e04c480251 up`

4. Re-executing the `ip a` should show;

``` bash
3: enx00e04c480251: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:e0:4c:48:02:51 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.3/24 scope global enx00e04c480251
       valid_lft forever preferred_lft forever

```

# Flashing Firmware

1. After entering into serial via `screen /dev/tty* 57600' and outputs UBoot logs, it should show this menu:

```
 ##### The CPU freq = 575 MHZ #### 
 estimate memory size =64 Mbytes
RESET MT7628 PHY!!!!!!
Please choose the operation: 
   1: Load system code to SDRAM via TFTP. 
   2: Load system code then write to Flash via TFTP. 
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   6: spi flash mcp test.
   7: Load Boot Loader code then write to Flash via Serial. 
   9: Load Boot Loader code then write to Flash via TFTP.                                                                   
```
Enter `2` quickly to directly flash the `firmware.bin` from the PC (or SBC if you are using it) going to the serial UART of the R051 unit. Wait for it to transfer the file and flashing process until it automatically boots. A successful boot should show the command shell by pressing Enter and it should not output any fatal error. 
