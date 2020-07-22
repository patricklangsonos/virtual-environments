# Worklog for bringing this online with Libvirt


## Prerequisites

- A working KVM setup. Although the [virt-manager](https://packages.ubuntu.com/bionic-updates/virt-manager) UI isn't actually required, installing it makes things easier to troubleshoot. It also includes all necessary dependencies. There's an [Ubuntu Wiki](https://help.ubuntu.com/community/KVM) with more details.
- [Packer](https://www.packer.io/downloads) - using 1.6.0 for testing, put it in `/usr/local/bin`
- [Vagrant](https://vagrantup.com) - using 2.2.9, installed using the Debian package
  - [Libvirt plugin](https://github.com/vagrant-libvirt/vagrant-libvirt#installation)
    - tip: `CONFIGURE_ARGS="with-libvirt-include=/usr/include/libvirt with-libvirt-lib=/usr/lib" vagrant plugin install vagrant-libvirt`
- A VNC viewer for troubleshooting the VM


## Building

```
$ packer build --only=qemu --var 'github_feed_token=...' ubuntu1804.json

# Or if you're debugging

$ PACKER_LOG=1 packer build --only=qemu --var 'github_feed_token=...' ubuntu1804.json
```


## Troubleshooting

If things appear to be stuck, look at the logs for the VNC port. Connect with your VNC viewer.

```
==> qemu: Starting VM, booting from CD-ROM
    qemu: The VM will be run headless, without a GUI. If you want to
    qemu: view the screen of the VM, connect via VNC without a password to
    qemu: vnc://127.0.0.1:5937
```


## Using this with Vagrant + libvirt

If you want to use this with Vagrant, you can package it up as a box. However, I'm currently running into a few issues with it:

- SSH is not set up for Vagrant's default public key https://superuser.com/a/591651
- Interface names change (ex: from ens3 to ens5), so netplan doesn't bring the right interface or interfaces up

Get [create_box.sh](https://raw.githubusercontent.com/vagrant-libvirt/vagrant-libvirt/master/tools/create_box.sh) from the libvirt repo.

cd to the output-qemu folder and run:

```
curl -Lo create_box.sh https://raw.githubusercontent.com/vagrant-libvirt/vagrant-libvirt/master/tools/create_box.sh
chmod +x ./create_box.sh
./create_box.sh  ubuntu1804
vagrant box add ubuntu1804.box --name github-runner-ubuntu1804
```

Now create the VM

```
mkdir ~/github-runner
cd ~/github-runner
vagrant init --box=github-runner-ubuntu1804
# edit your Vagrantfile as you see fit
vagrant up
```


## References

- Boot command and preseed.cfg from https://github.com/geerlingguy/packer-boxes/blob/master/ubuntu1804/box-config.json
- Worked around sudo prompt issue based on https://stackoverflow.com/questions/31788902/packer-build-fails-due-to-tty-needed-for-sudo
- Packer QEmu builder docs https://www.packer.io/docs/builders/qemu


## Similar Works

- This offers a builder for Docker and Packer on AWS: https://github.com/terradatum/github-runner . It has some interesting tools to merge JSON files that may be helpful too.