6) Configure Source NAT for our "LAN" & "DMZ" network. 
    set nat source rule 100 outbound-interface 'eth0'
    set nat source rule 100 source address '192.168.0.0/16'
    set nat source rule 100 translation address masquerade

7) And a DNS forwarder 
    set service dns forwarding cache-size '0'
    set service dns forwarding listen-on 'eth1'
    set service dns forwarding listen-on 'eth2'
    set service dns forwarding name-server '8.8.8.8'
    set service dns forwarding name-server '8.8.4.4'