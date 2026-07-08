# To set default route via 192.168.10.1

```
config router static
    edit 0
        set dst 0.0.0.0 0.0.0.0
        set gateway 192.168.10.1
        set device port1
    next
end
```