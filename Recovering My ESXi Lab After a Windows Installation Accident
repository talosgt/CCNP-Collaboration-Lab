# Recovering My ESXi Lab After a Windows Installation Accident

During the initial build of my Intel NUC lab, I accidentally rendered ESXi unbootable while installing Windows 11 onto a separate drive. Fortunately, the VMFS datastore remained intact, so after reinstalling ESXi I was able to reconnect to the datastore and recover all of my virtual machines. This was especially valuable because VMware's licensing and download process had changed since I originally built the lab, making a full rebuild more difficult than when I first started.

I followed VMware's licensing and download process that was available at the time. Because those policies have since changed, anyone reproducing this lab should consult the current VMware/Broadcom documentation for the appropriate licensing and installation method.

## Intel NUC Build - My Biggest Mistake (So Far)

Goal

Build a dedicated Intel NUC server for my Cisco Collaboration lab using ESXi.

## Hardware

- Intel NUC11TNHi7
- 64 GB RAM
- 1 TB NVMe SSD (ESXi datastore)
- 2.5" SATA HDD (Windows 11)

## What Happened

I wanted to install Windows 11 onto the 2.5" SATA drive.

I assumed Windows Setup would only modify that drive.

Instead...

Windows rewrote the bootloader and ESXi would no longer boot.

I thought I had destroyed my entire lab.

## What I Learned

The installer doesn't necessarily place boot files on the disk you select for Windows.

It often chooses the existing EFI System Partition on another drive.

In my case, Windows modified the boot configuration on the NVMe that contained ESXi.

## The Good News

Although ESXi no longer booted...

The virtual machines were still on the datastore. The VM files hadn't been erased. Only the boot environment had changed.

## Recovery

I reinstalled ESXi. After reconnecting to the datastore. All of my VMs reappeared.

That included:

- CUCM Publisher
- CUCM Subscriber
- EVE-NG
- Everything else

## Lesson Learned

Before installing another operating system:

- Disconnect any drives you don't want touched.
- Or disable them in BIOS.
- Always have a backup of the ESXi configuration.
- VM data can survive even if ESXi itself does not.
