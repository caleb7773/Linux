
EXAMPLE EXAMPLE EXAMPLE

Every time a new client connects to OpenVPN server, `ovpn_connect.sh` script creates set of individual iptables rules based on client common name and content of ccd files.
When client disconnects from OpenVPN server, `ovpn_disconnect.sh` removes these individual rules.
`opvn_run.sh` can be used to launch OpenVPN in docker contaner.

This should be a part of openvpn.conf file for scripts to work
```
# Client configuration
script-security 2
ccd-exclusive
client-config-dir  /etc/openvpn/ccd
client-connect     /etc/openvpn/scripts/ovpn_connect.sh
client-disconnect  /etc/openvpn/scripts/ovpn_disconnect.sh
```

EXAMPLE EXAMPLE EXAMPLE
