# To set static ip address

```
/ip address add address=192.168.1.2/24 interface=ether1 network=192.168.1.0
```

# To set dns server

```
/ip dns set servers=8.8.8.8,8.8.4.4 allow-remote-requests=yes
```

# To set default route

```
/ip route add dst-address=0.0.0.0/0 gateway=192.168.1.1
```

# To set nat overload(masquerade)

```
/ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade
```

