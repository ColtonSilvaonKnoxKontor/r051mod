# Reflash firmware for Bricked unit

This section helps you recover your bricked R051 unit. Before proceeding with the process of unbricking the unit, you need to [setting up the serial UART](/docs/serial.md) where it also covers about downloading the firmware. This section only shows on how to unbrick the unit due to firmware corruption.

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
