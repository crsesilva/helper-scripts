# Set static ip on interface

```
config system interface
    edit port1
        set ip 192.168.10.99 255.255.255.0
        set allowaccess ping https http ssh
        set alias LAN
        next
    end
    
```

# Set dns servers

```
config system dns
    set primary 8.8.8.8
    set secondary 8.8.4.4
end
```

To display

```
get system dns
```

