# To set static ip address

```
configure terminal
interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit
```

# To set dns server

```
ip name-server 8.8.8.8
ip name-server 8.8.4.4
```

# To set default route

```
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

# To set nat overload(masquerade)

Define an interface inside (WAN):

```
interface GigabitEthernet0/0
 ip nat outside
exit
```

Define an interface inside (LAN):

```
interface GigabitEthernet0/1
 ip nat inside
exit
```
Create an access-list to match nat traffic 

```
access-list 1 permit 192.168.1.0 0.0.0.255
```

Create nat rule

```
ip nat inside source list 1 interface GigabitEthernet0/0 overload
```

