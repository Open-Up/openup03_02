# Managing KVM

I this hands on practice, you will create your first KVM virtual machine and make it run.

## Step 1 : Checking capabilities

To run KVM, you need a processor that supports hardware virtualization. Intel and AMD both have developed extensions for their processors, deemed respectively Intel VT-x (code name Vanderpool) and AMD-V (code name Pacifica). To see if your processor supports one of these, you can review the output from this command:

```
egrep -c '(vmx|svm)' /proc/cpuinfo
```

```
kvm-ok 
```

You also need to check you have a 64 bit processor : 

```
egrep -c ' lm ' /proc/cpuinfo
```

If 0 is printed, it means that your CPU is not 64-bit.

If 1 or higher, it is. Note: lm stands for Long Mode which equates to a 64-bit CPU.

Now see if your running kernel is 64-bit, just issue the following command:

```
uname -m
```

## Installing KVM

Necessary packages : 

```
qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
```

Then add yourself to the **libvirtd** and **kvm** user group.

You can check your installation using : 

```
virsh list --all
```

Check also that **ls -l /dev/kvm** belongs to the right group (libvirtd). If you need to change group of the device, either relogin (or reboot) or remove and re-install kernel module **kvm**.



## Command line ?

First, install a necessary package, and create a Linux bridge from the command line.

```
$ sudo apt-get install bridge-utils
$ sudo brctl addbr br0 
```

Then configure a bridge in /etc/network/interfaces : 

```
#auto eth0
#iface eth0 inet dhcp

auto br0
iface br0 inet dhcp
        bridge_ports eth0
        bridge_stp off
        bridge_fd 0
        bridge_maxwait 0
```

Restart the network and check your interfaces.

Use the **debian-server.iso** transmitted during lecture to boot a virtual machine.

Use br0 as source of network for your virtual machine.

Create a qcow2 disk for your machine. Then start it.

You can get a look to the bridge thanks to **brctl show**.

Perform the following checks : 
 - list the VM
 - CPU / memory information
 - check VM disks partitions
 - set memory to 512 MB

Now connect to your VM with a VNC client (eg : TigerVNC). Start interacting with your new virtual machine.

## Virt manager to ease VM creation 

Install **virt-manager**.



(You can follow this guide for user interface : https://www.howtoforge.com/tutorial/kvm-on-ubuntu-14.04/ )
