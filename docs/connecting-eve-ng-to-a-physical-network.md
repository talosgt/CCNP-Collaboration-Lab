# Connecting EVE-NG to a physical network

## Goal

Connect a Cisco C8000v running inside EVE-NG to my physical home network.

## Initial Goal

```
Internet
    │
Verizon Router
192.168.1.1
    │
Home LAN
    │
Intel NUC (ESXi)
    │
EVE-NG
    │
Cisco C8000v
Gi1: 192.168.1.200
```

## Process

After deploying EVE-NG and importing a Cisco Catalyst 8000v image, I configured `GigabitEthernet1` on the router with the address `192.168.1.200/24`.

To connect the virtual router to my physical home network, I added a Cloud node to the EVE-NG topology. An EVE-NG Cloud node represents one of the network interfaces assigned to the EVE-NG virtual machine; it does not directly represent the physical router itself.

The EVE-NG virtual machine used VMware VMXNET3 network adapters, which appeared inside the EVE-NG operating system as Linux interfaces such as `eth0` and `eth1`. I mapped `Cloud1` to `eth1`, which was connected through an ESXi port group to my physical home LAN.

I then connected `GigabitEthernet1` on the Catalyst 8000v to `Cloud1`. This connected GigabitEthernet1 on the C8000v to my physical home network `192.168.1.0/24`, where my Verizon FiOS router was configured with the gateway address `192.168.1.1`.

The intended path was:

```text
Catalyst 8000v Gi1
192.168.1.200/24
        │
      Cloud1
        │
       eth1
        │
ESXi port group
        │
Physical home network
        │
Verizon router
192.168.1.1
```

Initially, I connected the C8000v to Cloud1, expecting it to communicate directly with my Verizon FiOS home network (192.168.1.0/24). However, the router was unable to reach the network or communicate with the Verizon router at 192.168.1.1.

After several rounds of troubleshooting, it became clear that the Cisco router configuration was not the source of the problem. Instead, the issue involved how the EVE-NG Cloud node interfaced with the VMware virtual networking configuration. Once the correct virtual interface and networking settings were identified, the C8000v was able to communicate successfully with my home network.

## Troubleshooting

When I examined the available Cloud interfaces inside EVE-NG, I found multiple VMware virtual adapters (VMXNET interfaces). Determining which interface corresponded to the physical network required verifying the virtual machine's network adapter assignments in ESXi and matching them to the interfaces detected by EVE-NG.

Once the correct VMXNET interface was mapped to Cloud1, the Cisco C8000v was finally able to communicate with my home network.

## ESXi Networking

Although the Cloud node was now connected to the correct virtual network adapter, traffic still did not pass correctly between the virtual Cisco router and the physical network. The issue turned out to be the ESXi virtual switch security policy. Enabling Promiscuous Mode, MAC Address Changes, and Forged Transmits allowed the virtual router to forward traffic properly and communicate with the physical network.

## Takeaways

Although the final solution involved only a few configuration changes, this was one of the first times I had to understand how VMware ESXi networking, EVE-NG Cloud nodes, and Cisco virtual routers all interacted. The issue wasn't caused by the router configuration itself, but by how the virtual networking was presented to the virtual machine.

This experience reinforced an important lesson: when troubleshooting virtual labs, don't assume the problem is inside the Cisco configuration. The virtualization layer is just as important to understand.

⬅️ **Previous:** [Rebuilding the Lab Environment](rebuilding-the-lab-environment.md)

➡️ **Next:** [Installing CUCM Publisher](installing-cucm-publisher.md)
