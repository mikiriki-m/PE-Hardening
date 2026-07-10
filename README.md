# PE Hardening

![Alt Text](images/Topology.png)

Using the existing Topology from the Inter-AS-MPLS-VPN Lab, security measures were implemented on PE1 so as to harden the router against cyber attacks. 

### Measures implemented:
1. BGP GTSM (Generalised TTL Security Mechanism)
2. MD5 Authentication
3. CoPP (Control Plane Policing)
4. uRPF (Unicast Reverse Path Forwarding) Strict Mode
5. iACL Access Control Lists

### BGP GTSM

BGP GTSM is a security mechanism which is designed to protect against CPU-utilisation attacks by validating the TTL (Time To Live) value. The TTL value determines the lifespan of the data. When the TTL reaches 0, the packet is discarded. GTSM works by verifying the TTL value (which is usually set at 255) and the TTL hop value (usually set to 1).
-GTSM is configured on both ends of a BGP connection
-Valid TTL hops must match on both ends of the connection

```
router bgp 1
 address-family ipv4 vrf VPN1
  neighbor 10.1.11.1 ttl-security hops 1
 exit-address-family

 address-family ipv4 vrf VPN2
  neighbor 10.1.12.1 ttl-security hops 1
 exit-address-family
```

### MD5 Authentication

A PSK (pre-shared key) is configured to every BGP neighbour. The matching passwords must be configured on CE1, CE2, and ASBR1 otherwise the BGP sessions will be dropped.

```
router bgp 1
 address-family ipv4 vrf VPN1
   neighbor 10.1.11.1 password Ce1BgpPassword!
 exit-address-family

 address-family ipv4 vrf VPN2
  neighbor 10.1.12.1 password Ce2BgpPassword!
 exit-address-family
```

### CoPP

```

```

### uRPF

uRPF is a security mechanism on Cisco routers to prevent IP spoofing and malicious traffic. It validates the source IP address by checking if the address exists, with its corresponsing matching interface, in its routing tables. It discards packets without a valid source IP address, thereby mitigating DoS attacks.

```
interface GigabitEthernet2/0
 description Connects to CE1 (VPN1)
 ip verify unicast source reachable-via rx

interface GigabitEthernet3/0
 description Connects to CE2 (VPN2)
 ip verify unicast source reachable-via rx
```

### iACL
