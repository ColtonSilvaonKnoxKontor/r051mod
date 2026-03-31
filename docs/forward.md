# Forward the Internet from ISP Router going to R051 modem (middleman method)

This method shows you how to forward your internet from ISP router and a Linux PC as middleman to distribute the internet through the R051 modem. This guarantees that not only the internal system of the R051 can access the internet, but also it's WiFi. This is only applicable if the SIM CARD is expired, missing or currently no load balance, and not yet openline.

You can use this if you want to install packages from OpenWrt, or use R051 as your **WiFi Repeater**. Further configuration is required if you also want to deploy on WiFi vendo as AP antenna.

## Initial Setup

1. First connect your device to R051 to access the admin portal of the SMART/PLDT Home WiFi by typing this in URL: 192.168.1.1.

2. Go to **Home Network**. Under **DHCP Settings**, change the LAN IP Address into `192.168.1.254`. (`192.168.1.1` will be used by your host PC, acting as gateway)

3. Also set the **DHCP Server** to **Disabled**.

4. Save and reboot. Now you need to reconnect your WiFi connection to R051, and accessing the dashboard now goes to `192.168.1.254`.

## Configuring our host Linux PC

Before that, you need to connect your ethernet cable from the PC to eth1 of your modem. Now depending on your situation, if you are receiving the internet connection via WiFi, then you may plug the eth cable to your primary ETH port. But if you are receiving the internet via primary ETH port, you need either network card, USB to ETH adapter or other means.

So in this example, we are using this identifier `enx00e04c480251` here as I am using USB to ETH adapter.

1. Assign the new IP address to the `enx00e04c480251`. We do not need to flush the IP here if you have assigned IP address from [recovery via tftp](/docs/bricked.md).

`sudo ip addr add 192.168.1.1/24 dev enx00e04c480251`

`sudo ip link set enx00e04c480251 up`

2. Now enable the forwarding:

`sudo sysctl -w net.ipv4.ip_forward=1`

**OPTIONAL:** If you want to save this configuration, `echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf`

3. Copy-paste the command one by one for NAT Masquerading to be able for the modem to get the host PC's internet connection:

```
sudo iptables -t nat -A POSTROUTING -o end0 -j MASQUERADE
sudo iptables -A FORWARD -i enx00e04c480251 -o end0 -j ACCEPT
sudo iptables -A FORWARD -i end0 -o enx00e04c480251 -m state --state
RELATED,ESTABLISHED -j ACCEPT
```

where `end0` is our primary ETH, and acts as WAN, and `enx00e04c480251` where this is a connection between host PC and r051.

4. Check it via `ip a`, and you should see the following:

```
3: enx00e04c480251: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:e0:4c:48:02:51 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.1/24 scope global enx00e04c480251
       valid_lft forever preferred_lft forever
    inet 10.10.10.3/24 scope global enx00e04c480251
       valid_lft forever preferred_lft forever
```

where `192.168.1.1` is now the gateway address of the host PC, and `10.10.10.3` is used for recovery and debricking the R051 via `tftp`.

# Configuring the R051 router:

Inside the system (via SSH or Serial):

1. We need to execute this `ip route del default` to remove the route pointing to SIM card.

2. Add this `ip route add default via 192.168.1.1 dev br-lan`

3. Set DNS to `echo "nameserver 8.8.8.8" > /etc/resolv.conf`

4. Confirm the connection via pinging to any URL: for example, pinging yandex as `ping dzen.ru`. If you immediately see bunch of response, it works; but if only hangs, the problem is surely either the modem or the host side.
