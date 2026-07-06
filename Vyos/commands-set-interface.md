1) Configure network Interface eth0 (internet)
    set interfaces ethernet eth0 address dhcp
    set interfaces ethernet eth0 description 'OUTSIDE'

2) Configure network interface eth1 (dmz)
    set interfaces ethernet eth1 address '192.168.2.1/24'
    set interfaces ethernet eth1 description 'DMZ'

3) Configure network interface eth2 (lan)
    set interfaces ethernet eth1 address '192.168.3.1/24'
    set interfaces ethernet eth1 description ‘LAN’