8) Local Firewall Rules
    * State Policy (accept established,related; drop invalid)
    * Accept ICMP echo-request (ping)
    * Accept DHCP request
    * Accept DNS requests
    * Limit SSH connections to 3 per minute per IP address and accept SSH from management networks
    * Accept SNMP from management networks

    - Create group objects:
        set firewall group network-group NET-DMZ network '192.168.2.0/24'
        set firewall group network-group NET-LAN network '192.168.3.0/24'
        set firewall group network-group NET-MANAGEMENT network '192.168.3.2/32'
        set firewall group network-group NET-MANAGEMENT network '192.168.3.3/32'
        set firewall group network-group NET-WAN network '192.168.1.0/24'

    - Create a named local policy for each interface:
            
        set firewall name WAN-LOCAL default-action 'drop'
        set firewall name WAN-LOCAL rule 1010 action 'accept'
        set firewall name WAN-LOCAL rule 1010 state established 'enable'
        set firewall name WAN-LOCAL rule 1010 state related 'enable'
        set firewall name WAN-LOCAL rule 1011 action 'drop'
        set firewall name WAN-LOCAL rule 1011 state invalid 'enable'
        set firewall name WAN-LOCAL rule 1020 action 'accept'
        set firewall name WAN-LOCAL rule 1020 icmp type-name 'echo-request'
        set firewall name WAN-LOCAL rule 1020 protocol 'icmp'
        set firewall name WAN-LOCAL rule 1020 state new 'enable'
        set firewall name WAN-LOCAL rule 1110 action 'accept'
        set firewall name WAN-LOCAL rule 1110 destination port '161'
        set firewall name WAN-LOCAL rule 1110 protocol 'udp'
        set firewall name WAN-LOCAL rule 1110 source group network-group 'NET-MANAGEMENT'
        set firewall name WAN-LOCAL rule 1110 state new 'enable'
        
        =====================================================================

        set firewall name LAN-LOCAL default-action 'drop'
        set firewall name LAN-LOCAL rule 1010 action 'accept'
        set firewall name LAN-LOCAL rule 1010 state established 'enable'
        set firewall name LAN-LOCAL rule 1010 state related 'enable'
        set firewall name LAN-LOCAL rule 1011 action 'drop'
        set firewall name LAN-LOCAL rule 1011 state invalid 'enable'
        set firewall name LAN-LOCAL rule 1020 action 'accept'
        set firewall name LAN-LOCAL rule 1020 icmp type-name 'echo-request'
        set firewall name LAN-LOCAL rule 1020 protocol 'icmp'
        set firewall name LAN-LOCAL rule 1020 state new 'enable'
        set firewall name LAN-LOCAL rule 1030 action 'accept'
        set firewall name LAN-LOCAL rule 1030 destination port '67'
        set firewall name LAN-LOCAL rule 1030 protocol 'udp'
        set firewall name LAN-LOCAL rule 1030 state new 'enable'
        set firewall name LAN-LOCAL rule 1040 action 'accept'
        set firewall name LAN-LOCAL rule 1040 destination port '53'
        set firewall name LAN-LOCAL rule 1040 protocol 'tcp_udp'
        set firewall name LAN-LOCAL rule 1040 state new 'enable'
        set firewall name LAN-LOCAL rule 1100 action 'drop'
        set firewall name LAN-LOCAL rule 1100 destination port '22'
        set firewall name LAN-LOCAL rule 1100 protocol 'tcp'
        set firewall name LAN-LOCAL rule 1100 recent count '4'
        set firewall name LAN-LOCAL rule 1100 recent time '60'
        set firewall name LAN-LOCAL rule 1100 source group network-group 'NET-MANAGEMENT'
        set firewall name LAN-LOCAL rule 1100 state new 'enable'
        set firewall name LAN-LOCAL rule 1101 action 'accept'
        set firewall name LAN-LOCAL rule 1101 destination port'22'
        set firewall name LAN-LOCAL rule 1101 protocol 'tcp'
        set firewall name LAN-LOCAL rule 1101 source group network-group 'NET-MANAGEMENT'
        set firewall name LAN-LOCAL rule 1101 state new 'enable'
        set firewall name LAN-LOCAL rule 1110 action 'accept'
        set firewall name LAN-LOCAL rule 1110 destination port '161'
        set firewall name LAN-LOCAL rule 1110 protocol 'udp'
        set firewall name LAN-LOCAL rule 1110 source group network-group 'NET-MANAGEMENT'
        set firewall name LAN-LOCAL rule 1110 state new 'enable'

        ========================================================================

        set firewall name DMZ-LOCAL default-action 'drop'
        set firewall name DMZ-LOCAL rule 1010 action 'accept'
        set firewall name DMZ-LOCAL rule 1010 state established 'enable'
        set firewall name DMZ-LOCAL rule 1010 state related 'enable'
        set firewall name DMZ-LOCAL rule 1011 action 'drop'
        set firewall name DMZ-LOCAL rule 1011 state invalid 'enable'
        set firewall name DMZ-LOCAL rule 1020 action 'accept'
        set firewall name DMZ-LOCAL rule 1020 icmp type-name 'echo-request'
        set firewall name DMZ-LOCAL rule 1020 protocol 'icmp'
        set firewall name DMZ-LOCAL rule 1020 state new 'enable'
        set firewall name DMZ-LOCAL rule 1040 action 'accept'
        set firewall name DMZ-LOCAL rule 1040 destination port '53'
        set firewall name DMZ-LOCAL rule 1040 protocol 'tcp_udp'
        set firewall name DMZ-LOCAL rule 1040 state new 'enable'
        set firewall name DMZ-LOCAL rule 1100 action 'drop'
        set firewall name DMZ-LOCAL rule 1100 destination port '22'
        set firewall name DMZ-LOCAL rule 1100 protocol 'tcp'
        set firewall name DMZ-LOCAL rule 1100 recent count '4'
        set firewall name DMZ-LOCAL rule 1100 recent time '60'
        set firewall name DMZ-LOCAL rule 1100 source group network-group 'NET-MANAGEMENT'
        set firewall name DMZ-LOCAL rule 1100 state new 'enable'
        set firewall name DMZ-LOCAL rule 1101 action 'accept'
        set firewall name DMZ-LOCAL rule 1101 destination port '22'
        set firewall name DMZ-LOCAL rule 1101 protocol 'tcp'
        set firewall name DMZ-LOCAL rule 1101 source group network-group 'NET-MANAGEMENT'
        set firewall name DMZ-LOCAL rule 1101 state new 'enable'
        set firewall name DMZ-LOCAL rule 1110 action 'accept'
        set firewall name DMZ-LOCAL rule 1110 destination port '161'
        set firewall name DMZ-LOCAL rule 1110 protocol 'udp'
        set firewall name DMZ-LOCAL rule 1110 source group network-group 'NET-MANAGEMENT'
        set firewall name DMZ-LOCAL rule 1110 state new 'enable'
       
        =======================================================================

    - apply the policy to each interface:
        set interfaces ethernet eth0 firewall local name 'WAN-LOCAL'
        set interfaces ethernet eth1 firewall local name 'DMZ-LOCAL'
        set interfaces ethernet eth2 firewall local name 'LAN-LOCAL'

9) Filter traffic between networks
    - Create the default "incoming" policy
        set firewall name LAN-IN default-action 'drop'
        set firewall name LAN-IN rule 1010 action 'accept'
        set firewall name LAN-IN rule 1010 state established 'enable'
        set firewall name LAN-IN rule 1010 state related 'enable'
        set firewall name LAN-IN rule 1011 action 'drop'
        set firewall name LAN-IN rule 1011 state invalid 'enable'
        set firewall name LAN-IN rule 9000 action 'accept'
        set firewall name LAN-IN rule 9000 source group network-group 'NET-LAN'
        set firewall name LAN-IN rule 9000 state new 'enable'

        set firewall name DMZ-IN default-action 'drop'
        set firewall name DMZ-IN rule 1010 action 'accept'
        set firewall name DMZ-IN rule 1010 state established 'enable'
        set firewall name DMZ-IN rule 1010 state related 'enable'
        set firewall name DMZ-IN rule 1011 action 'drop'
        set firewall name DMZ-IN rule 1011 state invalid 'enable'
        set firewall name DMZ-IN rule 9000 action 'accept'
        set firewall name DMZ-IN rule 9000 source group network-group 'NET-DMZ'
        set firewall name DMZ-IN rule 9000 state new 'enable'

    - Apply these policies to their interfaces:
        set interfaces ethernet eth1 firewall in name 'DMZ-IN'
        set interfaces ethernet eth2 firewall in name 'LAN-IN'
    
    ====================================================================================

    - create the “outgoing” policy for the LAN and DMZ
        set firewall name LAN-OUT default-action 'drop'
        set firewall name LAN-OUT rule 1010 action 'accept'
        set firewall name LAN-OUT rule 1010 state established 'enable'
        set firewall name LAN-OUT rule 1010 state related 'enable'
        set firewall name LAN-OUT rule 1011 action 'drop'
        set firewall name LAN-OUT rule 1011 state invalid 'enable'
        set firewall name LAN-OUT rule 1020 action 'accept'
        set firewall name LAN-OUT rule 1020 icmp type-name 'echo-request'
        set firewall name LAN-OUT rule 1020 protocol 'icmp'
        set firewall name LAN-OUT rule 1020 state new 'enable'

        set firewall name DMZ-OUT default-action 'drop'
        set firewall name DMZ-OUT rule 1010 action 'accept'
        set firewall name DMZ-OUT rule 1010 state established 'enable'
        set firewall name DMZ-OUT rule 1010 state related 'enable'
        set firewall name DMZ-OUT rule 1011 action 'drop'
        set firewall name DMZ-OUT rule 1011 state invalid 'enable'
        set firewall name DMZ-OUT rule 1020 action 'accept'
        set firewall name DMZ-OUT rule 1020 icmp type-name 'echo-request'
        set firewall name DMZ-OUT rule 1020 protocol 'icmp'
        set firewall name DMZ-OUT rule 1020 state new 'enable'
        set firewall name DMZ-OUT rule 1100 action 'accept'
        set firewall name DMZ-OUT rule 1100 source group network-group 'NET-LAN'
        set firewall name DMZ-OUT rule 1100 state new 'enable'

    - apply these to their interfaces:
        set interfaces ethernet eth1 firewall out name 'DMZ-OUT'
        set interfaces ethernet eth2 firewall out name 'LAN-OUT'

10) Enable logging
    - log configuration changes
        set firewall config-trap 'enable'

    - log traffic that reaches the default rule of a named policy (in our case the default drop)
        set firewall name LAN-IN 'enable-default-log'
        set firewall name LAN-OUT 'enable-default-log'
        set firewall name DMZ-IN 'enable-default-log'
        set firewall name DMZ-OUT 'enable-default-log'

11) Enable global configuration default
    set firewall all-ping 'enable'
    set firewall broadcast-ping 'disable'
    set firewall ip-src-route 'disable'
    set firewall log-martians 'enable'
    set firewall receive-redirects 'disable'
    set firewall send-redirects 'disable'
    set firewall source-validation 'disable'
    set firewall syn-cookies 'enable'