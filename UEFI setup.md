After a little more persistence I have come up a step by step workable solution for migrating from BIOS-Legacy to EFI bboting so I'll answer my own question now.

This applies only to booting multiple copies of unbuntu (or some flavour) and assumes you are starting from scratch with a new or repurposed drive and that your motherboard is fairly new (mine is 2014 vintage).  If you are moving your current bios/mbr install rather than doing a new install I suggest making a partition image with fsarchiver using qt-fsarchiver.  This assumes your current install of linux is UEFI capable.  The latests releases of ubunutu (and flavors) are UEFI capable.

With my step by step I now have a mirrored and testing copies of my primary os installed on the same machine all loadable from refind!

###Step by Step    

 - Create a persistent UEFI USB stick with some flavor of linux (see notes below)   

 -  Boot that stick as UEFI which involves entering your cmos settings and being sure your settings will boot UEFI.  You should see your USB listed there usually by manufacturer name.  It's best to just to turn off any legacy booting option(s) so you are sure.   When it boots the first time it will move most of OS files to the casper-rw partition and mount that as the filesystem.  If you put anything there before you'll have to look in the /media folder to find it.  

 - From the booted stick Using Gparted

     a. Make a new GPT partition table on what will be your primary drive (scrubs the drive!)

     b. Now make partition for ESP, 256mb or more, formatted Fat32, label it ESP.  Mark it bootable (it will be marked EFI bootable since it is a GPT drive now)

 -  Install rEFInd.  http://www.rodsbooks.com/refind/
There are three methods (deb package, script or manual)  I suggest downloading the latest bin.zip archive. Unpack that and run the install script from sudo.  It will find and mount your new ESP/EFI parition.  It does complain about not running the efibootmgr.  ESP/EFI will be still mounted at boot/efi.  So to fix this all I did was copy the drivers_x64 directory (mine is 64bit machine) found inside zip archive in the refind directory (same relative location).

 -  Now remove your stick and try rebooting.  Enter the Cmos and see if refind is now one of the available UEFI devices to boot.  I made it 2nd after my stick.  That way when the stick is in I boot from the stick otherwise from refind on the harddrive ESP.

 -  Now you can install an (efi) os on another partition like maybe Mint on the stick you just made.   In the /boot directory of this install might contain a refind_linux.conf file which can be customized.  I choose just to rename (delete it).  It's not necessarily needed and it can cause boot problems if misconfigured.  I recommend you choose clear partition labels as refind picks up on those  and uses them in the menu.  It will avoid having to customize the main refind.conf file which is found in the EFI/refind directory.  I chose to only uncomment/change these lines.  See Rod's excellent documentation for all things rEFIfind.    

* scan_delay 5 
* timeout 10 
* default_selection "your partition label"

###NOTES:

**Making a USB Stick**

* Use a 4gb min stick
   
* Use gparted to set a GPT table on the stick, make a 2gb partition and format it fat32, label it "boot", set the boot flag (it will be EFI) 
   
* Partition/format the rest ext4 with a label of "casper-rw".   
   
* Mount an efi capable iso (I used linux mint 17.1) and the "boot" partition and copy the contents of the iso.    
   
* edit boot/grub/grub.cfg on the USB stick, edit the first boot stanza and add the option "persistent" like this:    

`
linux	/casper/vmlinuz file=/cdrom/preseed/linuxmint.seed boot=casper   iso-scan/filename=${iso_path} quiet splash persistent --
`
http://shallowsky.com/blog/linux/install/ubuntu-persistent-live-cd.html

-------

While you have that mounted I would probably cut and paste this text along with the url of this post into a text file and save it there as well.

-------

One irritating thing about Mint is that if not on a laptop it will still load the laptop display and thus you can't "see" anything.   To Fix this right click a terminal open or use cntrl-alt-t.  Then run "cinnamon-settings" go to display settings and turn off the laptop display and make your primary display the default. 

-------

Later, once the USB is booted I also install qtfsarchiver so I can restore my fsarchiver archives with a GUI. 

If restoring a fsarchiver archive to a partition you will need to ask for a new uuid after restore (in gparted) and to edit the fstab accordingly as well as delete/fix any refind_linux.conf in the /boot directory.


 --
 Fixing lost motherboard NVRAM setting (firmware no longer lists REFIND as bootable choice)

1. mount efi(esp) partiton at /media/david/EFI/boot/efi and make a copy of efi/esp partition
2.  Download the latest refind bin.zip file and extract to desktop
3.  CD into extracted directory 

sudo ./install.sh --root /media/david/EFI/boot/efi

If it complains about not running efibootmgr properly you probably need to install it. I did. Then just rerun install script.

sudo apt-get install efibootmgr

-------------------------------------

--usedefault device-file    You can install rEFInd to a disk using the default/fallback filename of EFI/BOOT/bootx64.efi (and EFI/BOOT/bootia32.efi, if the 32-bit build is available) using this option. The device-file should be an unmounted ESP, or at least a FAT partition, as in --usedefault /dev/sdc1. Your computer's NVRAM entries will not be modified when installing in this way. The intent is that you can create a bootable USB flash drive or install rEFInd on a computer that tends to "forget" its NVRAM settings with this option. This option is mutually exclusive with --notesp and --root.

-------------

If restoring to another partition a copy which you want to boot

After restoring 4 tasks

*  FIRST edit the partition label (as it will be the same as the backup up partition) and might be mounted over the root /
*  Then use gparted to give the restored partiton (unmount if mounted) a new uuid so it doesn't conflict with the exiting one from which the backup was taken.
*  edit or delete refind_linux.conf file in the /boot directory of the restored partition.  If you decide to edit the uuid in the entries must match the new UUID.

*  edit the etc/fstab file and change the / root partition to the new label or uuid

after booting into new partition
* change wallpaper
* change icon indicating what version you are looking at
* 
