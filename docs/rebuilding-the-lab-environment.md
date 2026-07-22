# Rebuilding the lab environment

Building a Cisco Collaboration lab at home wasn't just about passing the CCNP Collaboration exam. I wanted an environment where I could safely experiment, break things, troubleshoot them, and learn how enterprise voice networks actually work.

Throughout my career as a Systems Engineer, I had worked around Cisco infrastructure, but I rarely had the opportunity to deploy an entire collaboration environment from scratch. Building my own lab gave me complete control over every component—from the hypervisor all the way to a physical IP phone sitting on my desk.

This repository documents that journey.

Before downloading a single ISO, I wrote down what I wanted this lab to accomplish.

## My goals were:

- Learn VMware ESXi administration
-  Gain hands-on experience with Cisco Unified Communications Manager (CUCM)
- Build and integrate Microsoft Active Directory
- Configure Cisco IP phones rather than relying only on emulators
- Understand routing, DNS, DHCP, LDAP, certificates, and voice VLANs
- Create documentation that I could refer back to years later

## Hardware:

| Component             | Purpose                    |
| --------------------- | -------------------------- |
| Intel NUC11TNHi7      | ESXi host                  |
| 64 GB RAM             | Virtual machines           |
| 1 TB NVMe             | Datastore                  |
| Cisco Catalyst 2960-X | Physical switching         |
| Cisco 8841            | IP phones                  |
| iMac                  | Administration workstation |

## Lab Architecture

```text
Intel NUC11TNHi7
│
└── VMware ESXi 8
    │
    ├── EVE-NG
    │   └── Cisco C8000v
    │
    ├── Cisco Unified Communications Manager
    │   ├── Publisher
    │   └── Subscriber
    │
    └── Windows Server 2019
```

Cisco Packet Tracer is excellent for learning networking fundamentals, but it doesn't emulate an enterprise collaboration deployment. I wanted to work with the same software used in production environments, including CUCM, Active Directory, DNS, LDAP, and physical Cisco IP phones.

With the overall design planned, the first real challenge wasn't installing CUCM—it was recovering my ESXi environment after accidentally overwriting the boot drive during a Windows installation.

That experience ultimately shaped how I approached backups, documentation, and lab recovery.

➡️ **Next:** [Connecting EVE-NG to a Physical Network](connecting-eve-ng-to-a-physical-network.md)
