# Virtual Machine Notes

## VMware

### Install

http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2053973

To install VMware Player on a Linux host:

Note: VMware Player for Linux is available as a .bundle download from the VMware Download Center. The Linux bundle installer starts a GUI wizard on most Linux distributions. On some Linux distributions, the bundle installer starts a command-line wizard instead of a GUI wizard.

    Log in to the Linux host with the user account that you plan to use with VMware Player.
    Open a terminal interface. For more information, see Opening a command or shell prompt (1003892).
    Change to root. For example:

    su root

Note: The command that you use depends on your Linux distribution and configuration.

Change directories to the directory that contains the VMware Player bundle installer file. The default location is the Download directory for the user which initiated the download from the VMware Download Center.
Run the appropriate Player installer file for the host system. For example:

    sh VMware-Player-e.x.p-xxxx.architecture.bundle --option

Where e.x.p-xxxx is the version and build numbers, architecture is i386 or x86_64, and option is a command line option.

The command line options available are:

        --gtk
        Opens the GUI-based VMware installer, which is the default option.

        --console
        Use the terminal for installation.

        --custom
        Use this option to customize the locations of the installation directories and set the hard limit for the number of open file descriptors.

        --regular
        Shows installation questions that have not been answered before or are required. This is the default option.

        --ignore-errors or -I
        Allows the installation to continue even if there is an error in one of the installer scripts. Because the section that has an error does not complete, the component might not be properly configured.

        --required
        Shows the license agreement only and then proceeds to install Player.

Accept the license agreement.

Note: If you are using the --console option or installing VMware Player on a Linux host that does not support the GUI wizard, press Enter to scroll through and read the license agreement or type q to skip to the yes/no prompt.

Follow the on-screen instructions or prompts to finish the installation.
Restart the Linux host.


After installation
On Windows host systems:

The installer creates a desktop shortcut, a quick launch shortcut, or a combination of these options in addition to a Start Menu item.
To start VMware Player on a Windows host system, select Start > Programs > VMware Player.

On Linux host systems:

VMware Player can be started from the command line on all Linux distributions.
On some Linux distributions, VMware Player can be started in the GUI from the System Tools menu under Applications.
To start VMware Player on a Linux host system from the command line, run the vmplayer command in a terminal window. For more information, see Opening a command or shell prompt (1003892). For example:

    /usr/bin/vmplayer &

---------------

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

