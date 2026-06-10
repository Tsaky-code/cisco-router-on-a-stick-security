# cisco-router-on-a-stick-security

# Cisco Router-on-a-Stick Inter-VLAN Routing & Department Security 

Project Overview
This project features a complete enterprise sub-network designed and simulated inside Cisco Packet Tracer. It uses a **Router-on-a-Stick (ROAS)** design to pass traffic between segmented business units. The deployment contains dedicated DHCP address pools and structural Access Control Lists (ACLs) to enforce security boundaries between departments.

Network Topology Design
The infrastructure separates end-user machines into distinct logical broadcast domains based on their business roles. 

HR Department Subnet: `192.168.10.0/24` (Assigned to VLAN 10)
IT Department Subnet: `192.168.20.0/24` (Assigned to VLAN 20)
Core Gateway Engine: Cisco 2911 ISR Router
Access Layer Infrastructure: Cisco Catalyst 2960-24TT Switch


Core Engineering Implementations

1. Router-on-a-Stick Gateway Configuration
Virtual sub-interfaces were created on the router's single physical connection (`GigabitEthernet0/0`). This allows it to trunk and route data for multiple internal networks simultaneously:
`GigabitEthernet0/0.10`: Default gateway for the HR division (`192.168.10.1`) utilizing IEEE 802.1Q tag 10.
`GigabitEthernet0/0.20`: Default gateway for the IT division (`192.168.20.1`) utilizing IEEE 802.1Q tag 20.

2. Automatic Address Provisioning (DHCP Engine)
Configured the core gateway router to dynamically manage and distribute local network addresses.
Excluded Zones: Preserved addresses `.1` across both subnets to protect core infrastructure.
Department Pools: Created separate pools (`ip dhcp pool HR` and `ip dhcp pool IT`) supplying unique network addresses, local default gateways, and external DNS pointers (`8.8.8.8`).
3. Access Switch Port Assignments & Trunking
Configured the local access switch to maintain proper department segmentation and forward traffic cleanly to the gateway:
Department Ports: Assigned access interfaces `Fa0/1 - Fa0/2` to VLAN 10 (HR), and interfaces `Fa0/3 - Fa0/4` to VLAN 20 (IT).
Core Uplink Trunking: Configured interface `FastEthernet0/24` as a dedicated trunk link using `switchport mode trunk` to allow VLAN 10 and 20 traffic to cross up to the router.

4. Traffic Filtering & Firewall ACLs
Implemented traffic security rules directly on the sub-interfaces to manage inter-departmental data tracking:
`access-list 1`: Created a standard ACL to drop traffic coming from the `192.168.10.0/24` subnet.
`access-list 100`: Configured an extended network access list applied on the inbound interface to strictly drop IP traffic traveling from the HR network directly into the IT resource blocks.

Verification & Functional Testing
To prove that our network configurations and security rules are working, diagnostic tests were run directly from an internal workstation endpoint:

1.Intra-VLAN Connectivity Success: A ping to an address within the same department (`192.168.10.2`) successfully returns responses with `0% packet loss`.
2. Security Access Policy Success: A ping directed across departments toward an IT workstation (`192.168.20.2`) is immediately blocked by the router's active ACL policy, returning a `Destination host unreachable` message.
 Hardware & Protocols Used
Simulation Environment: Cisco Packet Tracer 1.0
Core Appliances: Cisco 2911 Router, Cisco Catalyst 2960 Switch
Protocols & Standards: IEEE 802.1Q Trunking, Inter-VLAN Routing (ROAS), Cisco IOS DHCP Server, Extended Access Control Lists (ACLs)


<img width="817" height="548" alt="runningconf" src="https://github.com/user-attachments/assets/6d9d5136-5853-4bb1-9b54-fcd3a7cd25c3" />
<img width="817" height="548" alt="runningconf" src="https://github.com/user-attachments/assets/9cd65b2e-96ff-4604-82e9-0a0571300391" />
<img width="662" height="382" alt="routr" src="https://github.com/user-attachments/assets/84121629-b992-4a29-9c80-21f20a03aad6" />

<img width="515" height="539" alt="SWS" src="https://github.com/user-attachments/assets/247a90b8-e363-458b-9a1c-1043a574e695" />
<img width="659" height="390" alt="switch" src="https://github.com/user-attachments/assets/0a00272b-2053-436f-91ca-837938530866" />


<img width="491" height="517" alt="ping" src="https://github.com/user-attachments/assets/69f39565-c076-4601-b933-b2f86d606efc" />
<img width="603" height="580" alt="ip intrfac" src="https://github.com/user-attachments/assets/216fa7a9-3e48-424f-8ac3-6b163de48b0e" />
<img width="652" height="400" alt="intrsacs brif" src="https://github.com/user-attachments/assets/cda68aee-a10b-4c21-82df-28901fdaa56c" />
<img width="858" height="376" alt="Capture" src="https://github.com/user-attachments/assets/d1c80762-f832-424d-be9f-912e729d42c0" />
