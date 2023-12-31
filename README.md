# Connect to a Glopal Protect VPN that requires SAML authentication using NetworkManager and openconnect

This application allows to create and connect to a Global Protect VPN connection that requires SAML authentication using `nmcli` (NetworkManager) and `openconnect`.

Make sure to have installed `openconnect` and `network-manager-openconnect`.

## Usage

First, install the requirements:

```bash
python -m pip install -r requirements.txt
```

Then the script can be used, this is the help message:

```
usage: connect_to_global_protect_using_nmcli [-h] --connection-name CONNECTION_NAME --vpn-portal VPN_PORTAL [--vpn-user-group {portal,gateway}] [--vpn-os {linux,linux-64,win,mac-intel,android,apple-ios}]
                                             [--openconnect-args OPENCONNECT_ARGS]

Connect to a Glopal Protect VPN connection that requires SAML authenticaton using nmcli and openconnect.

options:
  -h, --help            show this help message and exit
  --connection-name CONNECTION_NAME
                        Name for the connection to add with nmcli if it's not already created.
  --vpn-portal VPN_PORTAL, --vpn-gateway VPN_PORTAL
                        Address of the portal/gateway of the Global Protect VPN.
  --vpn-user-group {portal,gateway}
                        Usergroup to pass to openconnect --usergroup parameter. Options are 'portal' or 'gateway', defaults to 'portal'.
  --vpn-os {linux,linux-64,win,mac-intel,android,apple-ios}
                        OS to pass to openconnect --os parameter.
  --openconnect-args OPENCONNECT_ARGS
                        Extra arguments to pass to openconnect.
```

## Example:

```bash
python connect_to_global_protect_using_nmcli.py --conection-name "Test GP VPN" --vpn-portal "portal.testvpn.com" --vpn-user-group "portal" --vpn-os "linux"
```

The script will automatically check if exists a connection with the name "Test GP VPN" configured for a VPN with the protocol "gp" (GlopalProtect) and for the portal specified, if the connection exists already then it is used to stablish the connection, if not, the script will automatically create a new connection with the parameters specified and use it to connect to the VPN.

After creating/getting the connection that will be used to connect to the VPN, then the script will open a Selenium browser to perform the SAML authentication and get the necessary prelogin cookie and username to get the vpn secrets to then connect  to the VPN via `nmcli`.

![Screenshot_20231123_101957](https://github.com/ahsand97/connect-to-globalprotect-using-nmcli/assets/32344641/4838fd3a-fdde-4e21-9289-67c5e7d82e09)

![Screenshot_20231123_125230](https://github.com/ahsand97/connect-to-globalprotect-using-nmcli/assets/32344641/956e3bec-21b7-40e9-85c4-d4d968de2672)

To delete the cache and do the SAML authentication again, the folder `~/.config/connect_to_global_protect_using_nmcli` can be removed.
