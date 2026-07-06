4) Enable SSH for remote access:
    set service ssh port '22'

5) Configure DHCP server for LAN network
    set service dhcp-server disabled 'false'
    set service dhcp-server shared-network-name LAN subnet 192.168.3.0/24 default-router '192.168.3.1'
    set service dhcp-server shared-network-name LAN subnet 192.168.3.0/24 dns-server '192.168.3.1'
    set service dhcp-server shared-network-name LAN subnet 192.168.3.0/24 domain-name 'lan-network'
    set service dhcp-server shared-network-name LAN subnet 192.168.3.0/24 lease '86400'
    set service dhcp-server shared-network-name LAN subnet 192.168.0.0/24 start 192.168.3.100 stop '192.168.3.254'