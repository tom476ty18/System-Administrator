# Arch Linux VM Installation Documentation:

# Introduction:
This documentation outlines the steps taken to install Arch Linux on a virtual machine (VM), along with additional modifications specified in the assignment. This is not a reproduction of the Arch Wiki but a personalized guide to recreate this customized Arch Linux VM.

# 1. Setting Up the VM Environment
Virtualization Software: VMware/VirtualBox (my choice)
Configure the VM with recommended resources: 2 GB RAM, 20 GB Disk, 2 CPU cores.
Command: No specific commands here, but make sure your VM settings align with these requirements for a smoother installation.

# 2. Booting the Arch ISO
Boot Mode: Ensure the VM boots in UEFI mode.
Command: ls /sys/firmware/efi/efivars
This checks if the system has booted in UEFI mode. If no files are shown, the VM may be in legacy mode.

# 3. Preparing the Disk
Partitioning:
Use cfdisk or fdisk to create partitions. Recommended layout:
/boot (512MB, FAT32)
Root partition / (remaining space)

Commands:
cfdisk /dev/sda
mkfs.fat -F32 /dev/sda1   # Boot partition
mkfs.ext4 /dev/sda2       # Root partition

# 4. Mounting Partitions
Mount the root and boot partitions.

Commands:
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot


# 5. Installing Essential Packages
Base Installation:

Command:
pacstrap /mnt base linux linux-firmware
Installs the core packages needed to run Arch Linux.


# 6. Configuring the System
Generate Filesystem Table:

Command:
genfstab -U /mnt >> /mnt/etc/fstab

Chroot into the installation:

Command:
arch-chroot /mnt


# 7. Localization and Time Zone
Set the Time Zone:

Command:
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc

Localization:

Edit /etc/locale.gen and uncomment your preferred locale, e.g., en_US.UTF-8 UTF-8.

Commands:
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf


# 8. Network Configuration
Hostname and Hosts File:

Commands:
echo "myhostname" > /etc/hostname

Add entries to /etc/hosts:
127.0.0.1   localhost
::1         localhost
127.0.1.1   myhostname.localdomain myhostname


# 9. Install and Configure Bootloader
GRUB Installation:

Commands:
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg


# 10. Additional Configurations
User Accounts:

Create accounts with sudo permissions.
Commands:
useradd -m -G wheel justin
passwd justin
echo "justin ALL=(ALL) ALL" >> /etc/sudoers
useradd -m -G wheel codi
passwd codi
echo "codi ALL=(ALL) ALL" >> /etc/sudoers

Set GraceHopper1906 as the password for instructor accounts and configure password change on login:
passwd -e justin
passwd -e codi


Desktop Environment (DE):

Install LXQT or another DE of your choice.
Commands:
pacman -S xorg-server lxqt sddm
systemctl enable sddm

Change Default Shell:

Install and set zsh as the default shell for the users.
Commands:
pacman -S zsh
chsh -s /usr/bin/zsh justin
chsh -s /usr/bin/zsh codi

SSH Installation:

Commands:
pacman -S openssh
systemctl enable sshd
systemctl start sshd

Terminal Color Coding:

Modify the ~/.bashrc or ~/.zshrc file to enable color coding. For example:
Commands:
echo 'PS1="\[\e[0;32m\]\u@\h:\w$\[\e[0m\] "' >> ~/.bashrc
source ~/.bashrc

Booting to GUI:

Ensure that the system boots into the DE.
Command:
systemctl set-default graphical.target

# 11. Troubleshooting and Issues
Document any errors or adjustments you made, such as:
Partitioning Errors: Incorrect mount points.
Network Configuration: DNS issues resolved by editing /etc/resolv.conf.
GRUB Installation: If GRUB installation fails, check the EFI partition size and reattempt.

# Conclusion:
This documentation should guide the recreation of the Arch Linux VM installation. Via my process of installing Arch Linux in VirtualBox. If required for clarification. Remember to consult the Arch Wiki for deeper insights or troubleshooting tips.

# References:
Arch Linux Wiki - Installation Guide

Markdown Guide:
This setup ensures you have a well-documented process and should streamline installation efforts on your Arch Linux VM. Once you've formatted this in Markdown, it will be ready for GitHub Pages.


