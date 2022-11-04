## Project 1 Markdown

Create new VM
Set Guest Operating system to Linux
Change Dropdown to Other Linux 5.x kernel 64-bit
Set Max disk size to 20GB
Before starting VM, go into the folder and find the .vmx file
Edit it and add ```firmware="efi``` to the second line
Boot into the machine

Verify Boot Mode: 
```ls /sys/firmware/efi/efivars```

Connect to the Internet
```ip link```
Check Internet Connection 
```ping archlinux.org```
Check Clock
```timedatectl status```

Create the Partitions
    ```fdisk /dev/sda```
In fdisk, create two partitions with these inputs:
    n
    Defaults for all except last sector (input = +5G)

    n
    Defaults for rest
Enter w to Save

Format the Partitions
```mkfs.fat -F 32 /dev/sda1```
```mkfs.ext4 /dev/sda2```

Mount the Partitions
```mount --mkdir /dev/sda1 /mnt/boot```
```mount /dev/sda2 /mnt```

Install Packages
```pacstrap -K /mnt base linux linux-firmware```

Create Fstab file 
```genfstab -U /mnt >> /mnt/etc/fstab```

Change Time Zone
```ln -sf /usr/share/zoneinfo/US/Central /etc/localtime```

``nano /etc/locale.gen``
Ctrl+W
Input ``en_US.UTF-8 UTF-8``
Uncomment it
Save
Exit
Verify with ``locale-gen``

``nano /etc/locale.conf``
Change ``LANG`` to ``LANG=en_US.UTF-8``

Create the Hostname file
``touch /etc/hostname``

Change root
``arch-chroot /mnt``pac

Change Password
``passwd``
Enter new password

**Boot Loader**

Change root
``arch-chroot /mnt``
Download GRUB
``pacman -Sy grub``
``pacman -Sy efibootmgr``

Mount to efi
``mount --mkdir /dev/sda1 /esp``

Install
``grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB``
Configure
``grub-mkconfig -o /boot/grub/grub.cfg``

Make Users
``useradd lexi``
``passwd lexi``
``groupadd sudo``
``usermod -aG sudo lexi``

``useradd codi``
``passwd codi``
``usermod -aG sudo codi``

Double Check while inside arch-chroot /mnt
``nano /etc/locale.gen``
Ctrl+W
Input ``en_US.UTF-8 UTF-8``
Uncomment it
Save
Exit
Verify with ``locale-gen``

``nano /etc/locale.conf``
Change ``LANG`` to ``LANG=en_US.UTF-8``

exit

Verify Sudo is working
``visudo``
Find "Uncomment to allow members of the group sudo to execute any command"
Uncomment
Ctrl+wq!

Change root
``arch-chroot /mnt``

Download LXDM
``pacman -Sy LXDM``
``pacman -Sy LXDE``

exit
Make Users
``useradd lexi``
``passwd lexi``
``groupadd sudo``
``usermod -aG sudo lexi``

``useradd codi``
``passwd codi``
``usermod -aG sudo codi``

Add Color
``cp /etc/pacman.conf /etc/pacman.conf.backup``
``nano /etc/pacman.conf``
Uncomment Color
Save and exit
``sed -i 's/#Color/Color/g' /etc/pacman.conf``
``pacman -Suy``

``cp /etc/nanorc /etc/nanorc.backup``
``nano /etc/nanorc``
Add ``include "/usr/share/nano/*.nanorc"`` to the end
Save and exit


Enable LXDE
``systemctl enable lxdm.service``
``nano /etc/lxdm/lxdm.conf``
Uncomment ``session=/usr/bin/startlxde``
Reboot
``exit``
``umount -R /mnt``
``reboot``

Create Alias'
``nano ~/.bashrc``
Input alias's with the syntax: ``alias (name)=command``








