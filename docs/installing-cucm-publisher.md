# Installing CUCM Publisher

## The CUCM Image

I initially attempted to obtain CUCM through my employer, but wasn't able to get access to the installation media. Since this lab was built for educational purposes, finding installation media became one of the biggest obstacles before I could even begin learning the platform. I was able to eventually gain access to the image through my own means. 

## Other Images

During the course of building my lab, I found that the networking community has created several excellent resources for EVE-NG users. One particularly helpful repository was [Cisco Images for GNS3 and EVE-NG](https://github.com/hegdepavankumar/Cisco-Images-for-GNS3-and-EVE-NG), which catalogs a wide variety of virtual appliances and image information for use in lab environments. While users are responsible for obtaining software in accordance with vendor licensing terms, resources like this were invaluable for understanding what virtual appliances were available and how they fit into an EVE-NG deployment.

## Why I Deployed CUCM Directly on ESXi

Although CUCM can be placed inside an EVE-NG environment, I chose to deploy the Publisher and Subscriber as dedicated virtual machines directly on VMware ESXi.

CUCM is an application server rather than a device that I needed EVE-NG to emulate. Running it directly on ESXi allowed me to manage its CPU, memory, storage, snapshots, and virtual network connections independently from the EVE-NG virtual machine. It also more closely represented how CUCM would normally be deployed in a production virtualized environment.

To separate the Collaboration environment from my home network, I created an ESXi port group named `PG-Voice` and assigned it VLAN ID 20. The following virtual machines were connected to this port group:

- CUCM Publisher
- CUCM Subscriber
- Windows Server running Active Directory and DNS
- One of the EVE-NG virtual network adapters

Inside EVE-NG, that network adapter was presented to the lab through a Cloud node. I connected the Cloud node to `GigabitEthernet2` on the Cisco Catalyst 8000v and configured the interface with `10.3.3.100/24`.

This made the C8000v the default gateway for devices on the `10.3.3.0/24` Collaboration network.

## Benefits of This Design

Deploying CUCM directly on ESXi provided several advantages:

- Better resource allocation
- Easier snapshot management
- A topology that more closely resembles production deployments
- Separation between the Collaboration network and the home LAN

## Architecture

Home network:           192.168.1.0/24
C8000v Gi1:             192.168.1.200/24

Collaboration network:  10.3.3.0/24
C8000v Gi2:             10.3.3.100/24
ESXi port group:        PG-Voice
VLAN:                   20

The C8000v acted as the Layer 3 gateway between my home network (192.168.1.0/24) and the Collaboration network (10.3.3.0/24).

The resulting architecture looked like this:

```
                   Home Network
                 192.168.1.0/24
                        │
               Verizon FiOS Router
                  192.168.1.1
                        │
                Cisco C8000v (EVE-NG)
        ┌───────────────┴───────────────┐
        │                               │
GigabitEthernet1                 GigabitEthernet2
192.168.1.200                    10.3.3.100
(Home LAN)                    (Collaboration LAN)
                                        │
                         VMware ESXi Port Group
                                        │
        ┌───────────────────────────────┼──────────────────────────────┐
        │                               │                              │
 CUCM Publisher                  CUCM Subscriber              Windows Server
 10.3.3.x                         10.3.3.x                    10.3.3.x
```

This design gave me a realistic multi-subnet lab that required proper routing, DNS, DHCP, and later Active Directory integration rather than placing every device on a single flat network.

## Smart Licensing

When I first built the lab, I assumed I would need to disable Cisco Smart Licensing to continue using CUCM beyond the evaluation period.

After spending time researching the topic and experimenting with the licensing configuration, I eventually discovered that the built-in evaluation period was sufficient for my lab environment. In practice, I found that the evaluation timer never became an obstacle during my studies, making the additional licensing changes unnecessary.
