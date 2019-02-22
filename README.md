# Portable Network Boot Environment

**ezPXE** basically is an *Ansible* playbook which configures a local Vagrant VM or a Linux server in a home or corporate network with a `dnsmasq` DHCP proxy providing a `pxelinux` network boot environment, bootable from any PXE capable system.

Ever needed to do a quick installation of Windows 10 or Ubuntu but didn't want to go through the annoying process of creating a bootable USB drive? **ezPXE** provides a fully functional network boot environment with minimal configuration effort for exactly that use case!
Just have the Vagrant environment and some operation system ISO laying on your local laptop disk ready and run `vagrant up` to get started.


## Requirements
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) + Oracle VM VirtualBox Extension Pack
- [Vagrant](https://www.vagrantup.com/downloads.html) + `vagrant-persistant-storage` plugin


## Quick Start
To set up a local environment with *Vagrant* follow those steps:
1. Fill up the `images/` folder with the desired ISO files.
2. Create a copy of `boot.yml.example`:
   ```bash
   cp boot.yml.example boot.yml
   ```
3. Populate `boot.yml` with the desired boot environments.
4. Install the required `vagrant-persistent-storage` Vagrant plugin.
5. `vagrant up`

**Hint**: The first run of the Ansible playbook will take some time, because all the ISO files are being transfered into the VM and extracted. The VM's data disk is persistent and will survive `vagrant destroy` without loosing any data on in.


## Configuration
To provide a network boot environment, a new item needs to be created in `boot.yml` (for real world examples see `boot.yml.example`).

- **name**: a unique identifier (`[a-zA-Z0-9_]`) for each item.
- **title**: a descriptive name for that item (e.g.: `Windows 10`)
- **src**: wether the filename of an ISO image in the `images/` folder or an URL pointing to a `mini.iso` on a network/internet server.
- **template**:
  - `iso`: a locally available live system or installation ISO file capable of being booted from a raw ISO file (e.g. *VMware ESXi*).
  - `netboot`: a small ISO file directly from the internet (e.g.: `netboot.xyz`).
  - `live`: a *live-boot* capable Linux installation media, except Ubuntu.
  - `casper`: an Ubuntu *live-boot* installation media
  - `windows`: any Windows installation media.

When adding new boot environments into an already running **ezPXE** setup, just run `vagrant provision` and everything will be updated inside the environment.


## Windows Installation
When booting into a Windows installation media, wait until the setup screen appears and press `Shift+F10`. Enter `startnet.cmd` into the command line window to start the installation from the automatically provided network share. From there on it's like installing Windows from an USB drive.


## Limitations
At the moment **ezPXE** is only usable for Legacy BIOS, UEFI support will follow.
