---
title: VLAN
author: Benedict Ng
date: 2024-08-28 23:15:56
categories:
    - Uncategorized
---
## Native VLAN (PVID)

Native VLAN and PVID are essentially the same, they are both port default VLAN ID. The default VLAN ID for every port is 1 initially, and it can be managed accordingly. It was invented to accomodate the old devices that do not support VLAN.

## Acess Port

### Receive

- Untagged Packet

Tag it with native VLAN ID

- Tagged Packet

If the packet mathes the native VLAN ID, the port accepts it; otherwise the port discards it.

### Forward

Deprive the native VLAN ID and forward the packet as untagged

## Trunk Port

### Receive

- Untagged Packet

For untagged packet, when trunk port receives it, it will be tagged with native VLAN ID. If the native VLAN ID matches any entry of allowed VLAN list, the packet will be accepted; otherwise it will be discarded.

- Tagged Packet

The port compares the tagged ID with every entry of allowed VLAN list, if they match, the packet will be accepted; otherwise it will be discarded.

### Forward

- Packet VLAN ID matches the native VLAN ID and the VLAN ID is in the allowed VLAN list:

Deprive the VLAN ID and forward it as untagged.

- Packet VLAN ID mismatches the native VLAN ID and the VLAN ID is in the allowed VLAN list:

Do nothing, forward it with the original tag.

## Hybrid Port

### Receive

- Untagged Packet

For untagged packet, when trunk port receives it, it will be tagged with native VLAN ID. If the native VLAN ID matches any entry of allowed VLAN list, the packet will be accepted; otherwise it will be discarded.

- Tagged Packet

The port compares the tagged ID with every entry of allowed VLAN list, if they match, the packet will be accepted; otherwise it will be discarded.

### Forward

If the VLAN ID is in the allowed VLAN list, forward it. It is allowed to set if the packet is forwarded with tag or without tag by command.

## Mac-Based VLAN

Mac-based VLAN is a technique that associates end devices mac address with VLAN ID. It is used for the following scenario: people move around in the office and they don't always sit in fixed position, that means each different end device, such as PC, uses changing ports of switch. In this case, the mac address of each end device has to be bound with VLAN ID in order to identify which VLAN the PCs belong to.

To configure mac-based VLAN, the ports connected to the end devices has to be configured as hybrid port and set every VLAN need to pass to the PCs as untagged. This is because the end devices cannot process the tagged frame.
