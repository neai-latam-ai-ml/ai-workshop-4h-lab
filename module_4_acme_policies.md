# Policies

The following document states the configuration policies of the devices of the ACME company.

## Addressing Schema

1. Routable addresses used on physical Interfaces (E.g: Ethernet 1/2), Port-Chanels (port-channel1) or SVI Interfaces must be taken from the CIDR 10.1.0.0/16

    Example: 
    ```
    interface vlan 123
        no switchport
        ip address 10.1.1.254/24
    ```

2. IP Addresses assigned to Router IDs (RID) of routing protocols such as OSPF, EIGRP or BGP must be taken from the CIDR 172.20.0.0/16

    Example: 
    ```
    router ospf OSPF1
        router-id  172.20.1.5
    ```

## Interface Description

1. Interfaces must have a description that matches the following convention: `<type>_<speed>_<peer_id>`, where `type` is `L2` for switch interfaces, `L3` for routed interfaces, and `PC` for interfaces that are members of a port-channel.

    ```
    interface ethernet1/1
        no switchport
        description L3_10G_DMZ Router  
        ip address 10.1.2.254/24
    ```

## VLANs

1. All VLANs must have a unique VLAN ID and name reflecting their purpose. The format should be: `<Department>_<Purpose>_<VLAN_ID>`.

    ```
    vlan 100
        name IT_SERVERS_100
    ```

2. Reserved VLANs for management and infrastructure:  
   - Management VLAN: 10  
   - Voice VLAN: 20  
   - Guest VLAN: 30

## Routing Protocols

1. OSPF areas must be designed hierarchically with backbone area 0. Redistribution between areas must be explicitly defined.  

    ```
    router ospf 1
        network 10.1.0.0 0.0.255.255 area 0
    ```

2. BGP sessions must use MD5 authentication and be explicitly documented with neighbor IPs and AS numbers.

    ```
    router bgp 65001
        neighbor 172.20.1.2 remote-as 65002
        neighbor 172.20.1.2 password <MD5-password>
    ```

## Quality of Service (QoS)

1. All edge interfaces must implement QoS policies to prioritize voice and critical applications.  

    ```
    policy-map EDGE_POLICY
        class VOICE
            priority 500
        class CRITICAL_APP
            bandwidth percent 30
    ```

2. Default traffic should be marked as Best Effort (BE).

## Security Protocols

1. The only valid hashing protocol for payload integrity is MD5. This applies for SNMP configurations and authenticated BGP peerings.

    ```
    snmp-server user MYUSER MYGROUP v3 auth sha <password> priv aes 256 <password>
    ```

2. The only valid authentication protocol for payload encryption is AES 256.

## Logging and Monitoring

1. All devices must send logs to a centralized syslog server.  

    ```
    logging host 10.1.255.5
    logging trap informational
    ```

2. SNMP monitoring must be configured for all network devices using SNMPv3.

## Backup and Maintenance

1. Configuration backups must be taken daily and stored in a secure location.  
2. Firmware updates must follow the company change management procedure and be documented.  
3. Maintenance windows must be communicated to affected teams at least 48 hours in advance.

