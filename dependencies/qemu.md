# QEMU Setup Guideline
This is a step-by-step guide to setup QEMU, an open source machine emulator and virtualizer.

## Install QEMU
### Mac OS X Brew Install
```sh
brew install qemu
```
### Debian Package Install
```sh
apt-get install qemu
```
### RPM Package Install
```sh
yum install qemu-kvm
```
### Check QEMU installed version
```sh
qemu-system-x86_64 --version
```

## Build a Virtual Machine
Below is a sample shell script to build Ubuntu 18.04 VM on a Mac OS X.

Copy its content and save into `run.sh` file in your directory.
```sh
#!/usr/bin/env bash

set -eux

# Parameters.
id=ubuntu-18.04.4-desktop-amd64
disk_img="${id}.img.qcow2"
disk_img_snapshot="${id}.snapshot.qcow2"
iso="${id}.iso"

# Get image.
if [ ! -f "$iso" ]; then
  wget "http://releases.ubuntu.com/18.04/${iso}"
fi

# Go through installer manually.
if [ ! -f "$disk_img" ]; then
  qemu-img create -f qcow2 "$disk_img" 10G
  qemu-system-x86_64 \
    -cdrom "$iso" \
    -drive "file=${disk_img},format=qcow2" \
    -enable-kvm \
    -accel hvf \
    -m 2G \
    -smp 2 \
  ;
fi

# Snapshot the installation.
if [ ! -f "$disk_img_snapshot" ]; then
  qemu-img \
    create \
    -b "$disk_img" \
    -f qcow2 \
    "$disk_img_snapshot" \
  ;
fi

# Run the installed image.
qemu-system-x86_64 \
  -drive "file=${disk_img_snapshot},format=qcow2" \
  -enable-kvm \
  -accel hvf \
  -device e1000,netdev=net0 \
  -netdev user,id=net0,hostfwd=tcp::5555-:22 \
  -show-cursor \
  -usb \
  -device usb-tablet \
  -smp 8 \
  -m 2G \
  -vga virtio \
  "$@" \
```
Now run the script:
```sh
./run.sh
```
**Note**: you might need to tune the options in `qemu-system-x86_64` command depending on your host machine specification. For a list of standard options please [click here](https://www.qemu.org/docs/master/qemu-doc.html#Standard-options).

## References
* _Download QEMU_, QEMU Official Site, viewed 28 Feb 2020, <<https://www.qemu.org/download>>.
* _QEMU User Documentation_, QEMU Official Site, viewed 28 Feb 2020, <<https://www.qemu.org/docs/master/qemu-doc.html#pcsys_005fquickstart>>.
* _How to run Ubuntu desktop on QEMU?_, Ask Ubuntu, viewed 28 Feb 2020, <<https://askubuntu.com/questions/884534/how-to-run-ubuntu-desktop-on-qemu>>.