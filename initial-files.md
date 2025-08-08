---
title: "Welcome to LFCS Notes"
#permalink: /
---
# My Personal LFCS Study Guide

Welcome! This site serves as my personal, evolving knowledge base for the **[Linux Foundation Certified Sysadmin (LFCS)](https://training.linuxfoundation.org/certification/linux-foundation-certified-sysadmin-lfcs/#exams)** certification. It's a collection of notes, commands, and key concepts organized to aid in my studies and, hopefully, to help others on a similar path.

The content is a work in progress, compiled from various resources and my own hands-on practice.

---

## 🚀 Exam Domains Structure

To align directly with the certification requirements, this guide is structured to mirror the official **LFCS Domains & Competencies** as published by The Linux Foundation. You can use the sidebar to navigate through each core area.

The main domains covered are:

* **Operations Deployment** (25%)
* **Networking** (25%)
* **Storage** (20%)
* **Essential Commands** (20%)
* **Users and Groups** (10%)

Each section will contain detailed notes, practical examples, and configuration snippets relevant to that domain.

## 📚 Additional Resources & Notes

Beyond the official domains, this site also includes supplementary sections that I find useful:

*   **LFS201 Course Notes:** Personal notes and key takeaways from the Linux Foundation's "Essentials of Linux System Administration (LFS201)" course. This section consolidates crucial concepts from the training material.

*   **Meta: Site Deployment:** A special section detailing how this site itself is built and deployed. It serves as a practical example of using **GitHub Pages with its native Jekyll engine** and the `just-the-docs` theme, requiring zero CI/CD pipeline configuration. A great real-world use case!

---

Use the navigation on the left to dive into the topics. Good luck with your studies! 🐧
---
title: "Provisioning CentOS Stream 8"
#description: "A step-by-step guide to installing a CentOS Stream 8 VM using virt-install and KVM storage pools."
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
---
title: "Provisioning Ubuntu 22.04 LTS"
#description: "Guide for an automated, text-based installation of an Ubuntu 22.04 LTS server for our lab."
---

# Provisioning an Ubuntu 22.04 LTS VM on KVM (CLI)

This guide covers an automated, text-based installation of Ubuntu Server 22.04 using `virt-install` and KVM storage pools.

## 1. Initial Specifications

This VM will be the first Debian-based guest domain for our LFCS lab environment.

- **Name:** `lfcs-ubuntu-n01`
- **RAM:** 2048 MB
- **Disk Path:** `/var/lib/libvirt/images/vms_pool/lfcs-ubuntu-n01.qcow2` (managed by the pool)
- **Disk Size:** 16 GB
- **vCPUs:** 2
- **Network Bridge:** `br0`

## 2. Prerequisites

- A running KVM host.
- An active storage pool named `vms_pool`.
- The Ubuntu Server 22.04 ISO is downloaded to `/var/extra/kvm/isos/ubuntu-22.04.5-live-server-amd64.iso`.

## 3. Creating the Virtual Disk via Storage Pool

This ensures proper management of the virtual disk by libvirt.

```bash
sudo virsh vol-create-as vms_pool lfcs-ubuntu-n01.qcow2 16G --format qcow2
```
    
Verify its creation:
```bash
sudo virsh vol-list vms_pool
```

## 4. The virt-install Command for Automated Install

This command uses `--location` to extract the kernel and initrd, enabling a console-based, automated installation.

```bash
sudo virt-install --name lfcs-ubuntu-n01 \
   --ram 2048 \
   --vcpus 2 \
   --disk vol=vms_pool/lfcs-ubuntu-n01.qcow2 \
   --os-variant ubuntu22.04 \
   --network bridge=br0 \
   --graphics none \
   --console pty,target_type=serial \
   --location /var/extra/kvm/isos/ubuntu-22.04.5-live-server-amd64.iso,kernel=casper/vmlinuz,initrd=casper/initrd \
   --extra-args 'console=ttyS0,115200n8 autoinstall'
```

_Note:_ The `--extra-args '... autoinstall'` part triggers Ubuntu's automated installation process. You may need a cloud-init user-data file for a fully unattended setup. For a standard text-based install, you can follow the prompts directly in the console after connecting.

To connect to the serial console:
```bash
sudo virsh console lfcs-ubuntu-n01
```

## 5. Benefits of Using a Storage Pool
- Integrated Management: The disk is registered within the KVM pool.
- Better Visibility: Easily list all volumes with virsh `vol-list <pool_name>`.
- Portability: If you export/move the pool, all its volumes are included.
- Security: Permissions are managed by libvirt.

---
title: Useful Resources
permalink: /resources/
---

# Resource Hub

Here is a curated collection of links, tools, and sites that are extremely useful for studying Linux Systems Administration and preparing for the LFCS exam.

## Community Study Guides
*A great way to get different perspectives and find inspiration.*
* [daveopspublic > lfcs-study-notes](https://gitlab.com/daveopspublic/lfcs-study-notes)
* [Giuliano's LFCS Exam Personal Notes](https://giulianopz.github.io/lfcs/)

## Official Documentation & Related Certs
*Links to official documentation from recognized organizations.*
* [Linux Professional Institute (LPI) - LPIC-1](https://www.lpi.org/our-certifications/lpic-1-overview/)

<!--
## Cheat Sheets
*Quick references for day-to-day commands.*
*   [Linux Command Line Cheat Sheet](https://...link...)
*   [Vim Cheat Sheet](https://...link...)
-->

<!--
## Labs & Hands-On Practice
*Platforms to practice your skills in a real environment.*
*   [Katacoda (Now on O'Reilly)](https://...link...)
*   [KodeKloud](https://...link...)
-->

## Online Communities
*Places to ask questions and learn from others.*

<!-- * [Linux Foundation Forum]() -->

* [Reddit - r/linuxadmin](https://www.reddit.com/r/linuxadmin/)
---
layout: default
title: Lab Environment Setup
nav_order: 2
has_children: true
permalink: /lab-setup/  # Le damos una URL limpia
---

# Lab Environment Setup

This section details the process of setting up the virtual lab environment used for studying and practicing for the LFCS exam. The primary hypervisor used is KVM on a Linux host.

Here you will find step-by-step guides for provisioning the virtual machines that will serve as our practice targets.
