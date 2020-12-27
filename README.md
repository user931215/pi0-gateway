# pi0-gateway
Use the pi0w as advanced usb-network adapter. Currently works as usb hotplug for wifi access.

## Dependencies:
Pi0w with raspberry pi os lite, usb-ethernet, ssh

## Setup networks: 
Edit files `/etc/wpa_supplicant/wpa_supplicant.conf` and `/etc/network/interfaces`

## Setup routing
add routing and iptables from file `routing`

### Future plans:
- add vpn option
- iptables control
- web gui
