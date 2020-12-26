# pi0-gateway
Use the pi0w as advanced usb-network adapter

First, install raspberry pi os on the pi0w, configure usb-tether and ssh. 
Setup networks: 

/etc/network/interfaces:
source-directory /etc/network/interfaces.d
```
allow-hotplug usb0
iface usb0 inet static
        address 192.168.7.2 #setup host as 7.1
        netmask 255.255.255.0
        network 192.168.7.0
        broadcast 192.168.7.255
        gateway 127.0.0.1

allow-hotplug wlan0
iface wlan0 inet dhcp
        wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Setup wpa_supplicant.conf:
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="ssid1"
    psk="real smart passphrase"
}
network={
    ssid="ssid2"
    psk="even smarter passphrase"
}
```

setup /etc/sysctl.conf
`net.ipv4.ip_forward=1` <- uncomment this line

*****setup iptables
```
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i usb0 -o wlan0 -j ACCEPT

backup iptables rules:
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"

add changes to boot, /etc/rc.local, above exit 0
iptables-restore < /etc/iptables.ipv4.nat
