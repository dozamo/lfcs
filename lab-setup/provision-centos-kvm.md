---
layout: default
title: Provisioning CentOS Stream 8
# ¡YA NO NECESITAMOS LA LÍNEA 'parent'! Jekyll lo deduce por la carpeta.
### parent: Lab Environment Setup
nav_order: 1
#permalink: /lab-setup/centos/ # Opcional, para una URL más bonita
---

# Provisioning a CentOS Stream 8 VM on KVM (CLI)

This guide covers the creation of a CentOS 8 Stream VM using `virt-install` and KVM storage pools.

## 1. Initial Specifications

This VM will be the first RHEL-based guest domain for our LFCS lab environment.

- **Name:** `lfcs-centos-n01`
- **RAM:** 2048 MB
- **Disk Path:** `/var/lib/libvirt/images/vms_pool/lfcs-centos-n01.qcow2` (managed by the pool)
- **Disk Size:** 16 GB
- **vCPUs:** 2
- **Network Bridge:** `br0`

## 2. Prerequisites

- A running KVM host.
- A "dir" type storage pool named `vms_pool` is available and active.
  ```bash
  $ sudo virsh pool-info vms_pool
  Name:           vms_pool
  UUID:           e97f1b55-5f63-494f-bbd6-a470f5425115
  State:          running
  Persistent:     yes
  Autostart:      yes
  Capacity:       2.32 TiB
  Allocation:     1.65 TiB
  Available:      702.66 GiB
  ```

The CentOS Stream 8 ISO is downloaded to `/var/extra/kvm/isos/CentOS-Stream-8-20240603.0-x86_64-dvd1.iso`.

## 3. Creating the Virtual Disk via Storage Pool

Using a storage pool is the recommended method for managing virtual disks.
```bash
sudo virsh vol-create-as vms_pool lfcs-centos-n01.qcow2 16G --format qcow2
```

Verify its creation:
```bash
sudo virsh vol-list vms_pool
```

## 4. The virt-install Command

We will use the `--cdrom` option for the installation media.
```bash
sudo virt-install --name lfcs-centos-n01 \
   --ram 2048 --vcpus 2 \
   --disk vol=vms_pool/lfcs-centos-n01.qcow2 \
   --network bridge=br0 \
   --graphics vnc,listen=127.0.0.1,port=5922 \
   --cdrom /var/extra/kvm/isos/CentOS-Stream-8-20240603.0-x86_64-dvd1.iso \
   --os-variant centos-stream8 \
   --boot cdrom,menu=on
```

_Note:_ After running this command, connect to the VM using a VNC client or `virt-viewer` to complete the manual OS installation. To connect via `virt-viewer`:
```bash
virt-viewer --connect qemu:///system --wait lfcs-centos-n01
```

## 5. Benefits of Using a Storage Pool

- _Integrated Management:_ The disk is registered within the KVM pool.
- _Better Visibility:_ Easily list all volumes with `virsh vol-list <pool_name>`.
- _Portability:_ If you export/move the pool, all its volumes are included.
- _Security:_ Permissions are managed by libvirt.
