Iptables role depends upon the following variables

    firewall_opts:
        service: 'iptables' | 'firewalld' (Required) 
        enabled: 'yes' | 'no' (Required)
        state: 'started' | 'stopped'  (Required)
        masquerade: interface (optional)
        
        # Lists of rules for each table/chain in iptables (optional)
        filter_table:
           input:  
            - "-A INPUT -m state --state NEW -m tcp -p tcp -s 130.118.160.194 --dport 5666 -j ACCEPT"
           forward:
           output:
        nat_table:
            prerouting:
            postrouting:
            output:

        special_nets:
            -  declaration: ":ALLOWED_NETS - [0:0]"
               ranges:
                    - "-A INPUT -s 10.208.100.0/22 -j ALLOWED_NETS"
                    - "-A INPUT -s 10.210.56.0/21 -j ALLOWED_NETS"
                    - "-A INPUT -s 10.213.11.0/24 -j ALLOWED_NETS"
               rules:
                    - "-A ALLOWED_NETS -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT"
            
        # list of rules for firewalld (optional)
        rules:
            immediate: 
            permanent: 
            port: 
            service: 
            source: 
            state: 
            zone:        

Iptables will control the service configuration and write a new iptables rule set     
it also makes sure the main iptables file is standardized
as:

    # Firewall configuration written by ansible configuration
    # Updated on: 
    *filter
    :INPUT ACCEPT [0:0]
    :FORWARD ACCEPT [0:0]
    :OUTPUT ACCEPT [0:0]
    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    -A INPUT -p icmp -j ACCEPT
    -A INPUT -i lo -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp -s 130.118.160/22 --dport 22 -j ACCEPT
    -A INPUT -j REJECT --reject-with icmp-host-prohibited
    -A FORWARD -j REJECT --reject-with icmp-host-prohibited
    COMMIT
    

                