<h1>Updates</h1>

We currently have a cloud server where your IsardVDI can connect and update contents.

[TOC]

# Types of updates

## Domains

Domains in updates are Operating Systems already installed and optimized, ready to be used. Default user is **isard** and password **pirineus**

## Media

Links to ISO downloads.



## Virt installs

This allows to download full set of hardware definitions for new OS that you will install with your uploaded ISOS in media.

# Recommended updates

There are updates that will bring features for some actions, mainly if your are using Win guests and host clients:

- **Media**:
  - Windows Virtio drivers x64: This will allow to add this ISO to new created Win desktops. In this ISO there are all the drivers needed for windows performance optimization. Recommended to install at least Virtio for hard disk and for network.
  - Virt-viewer: This is the client optimized for desktop viewers using the Spice protocol that should be used in Windows
  - Virt-viewer usb drivers: This is the client transparent plug-in for Win that will allow to transparently plug any USB device from your client computer to the guest, as if it was connected there.