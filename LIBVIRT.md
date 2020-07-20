# Worklog for bringing this online with Libvirt


## Prerequisites

- A working KVM setup. Although the [virt-manager](https://packages.ubuntu.com/bionic-updates/virt-manager) UI isn't actually required, installing it makes things easier to troubleshoot. It also includes all necessary dependencies. There's an [Ubuntu Wiki](https://help.ubuntu.com/community/KVM) with more details.
- [Packer](https://www.packer.io/downloads) - using 1.6.0 for testing, put it in `/usr/local/bin`
- [Vagrant](https://vagrantup.com) - using 2.2.9, put it in `/usr/local/bin`
  - [Libvirt plugin](https://github.com/vagrant-libvirt/vagrant-libvirt#installation)
- A VNC viewer for troubleshooting the VM


## Building

```
$ packer build --only=qemu ubuntu1804.json

# Or if you're debugging

$ PACKER_LOG=1 packer build --only=qemu ubuntu1804.json
```


## Troubleshooting

If things appear to be stuck, look at the logs for the VNC port. Connect with your VNC viewer.

```
==> qemu: Starting VM, booting from CD-ROM
    qemu: The VM will be run headless, without a GUI. If you want to
    qemu: view the screen of the VM, connect via VNC without a password to
    qemu: vnc://127.0.0.1:5937
```