# Worklog for bringing this online with Libvirt


## Prerequisites

- A working KVM setup. Although the [virt-manager](https://packages.ubuntu.com/bionic-updates/virt-manager) UI isn't actually required, installing it makes things easier to troubleshoot. It also includes all necessary dependencies. There's an [Ubuntu Wiki](https://help.ubuntu.com/community/KVM) with more details.
- [Packer](https://www.packer.io/downloads) - using 1.6.0 for testing
- [Vagrant](https://vagrantup.com)
  - [Libvirt plugin](https://github.com/vagrant-libvirt/vagrant-libvirt#installation)



## Building

```
$ packer build --only=qemu ubuntu1804.json
```