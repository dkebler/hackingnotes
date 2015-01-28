# Virtual Machine Notes

## VMware


Issue with missing machines in list in VMware Player on linux host.
https://communities.vmware.com/thread/305983
10. Re: Guest Machines Do Not Show in VMPlayer Library

An easy enough workaround. Just create a symbolic link:
```
ln -s /home/user/.local/share/recently-used.xbel /home/user/.recently-used.xbel
```
That seemed to fix the problem for me.  Thanks for the previous posters who noticed which files were being used!

### 3d Acceleration in linux host

Hi there

If you are running Windows Virtual Machines on OPENSUSE or LINUX Mint (I haven't tried other distros) and are using INTEL (not the Nvidia graphics) you might be struggling to enable 3D and hardware acceleration on Windows Guests. This post applies to those running VMWARE workstation or VMPLAYER --it might work with Virtual Box - haven't tried.

What you need to do are the following

Install these packages

1) Driconf (OPENSUSE 13.1 ignore - not necessary)

2) install libtxc-dxtn (might be dxtn0 etc if its been upgraded). This package enables S3TC which is required to get the hardware acceleration and 3D available for the VM's

3) add these lines to the Virtual Machines configuration file (the .vmx file)

mks.enable3d="TRUE"
mks.gl.allowBlacklistedDrivers="TRUE"


re-start VMware workstation / vmplayer

enjoy proper DVD / games etc .

(Note in the VMware editor also check the enable 3D box in the virtual machine settings !!!).

BTW this will also correct problems in UNITY mode for Windows 8.1 Guests where the application menus are transparent !!. Note here I mean UNITY mode for VM's as in Desktop integration -- NOT UNITY as used in UBUNTU).
http://www.linuxquestions.org/questions/linux-virtualization-and-cloud-90/enabling-3d-and-hardware-acceleration-in-guest-vms-on-linux-host-4175503780/

## Making Copies 
(linux machines)

change the MAC address of the virtual network card in VM player
(under advanced)
change a linux machine name after loading to avoid confusion
```sudo rsub /etc/hostname
and
   sudo rsub /etc/hosts

make single entry in first <hostname> file match that in hosts file 127.0.1.1 <hostname>
```
Check the ip address and copy/modify an entry of the original host entries in the config file under .ssh directory

