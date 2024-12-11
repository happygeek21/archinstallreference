# My Personal Arch Install Commands

## Disclaimer
This guide is tailored to my personal Arch Linux installation process. Use it at your own risk. Always ensure that you have a backup of your data and verify commands as they may vary depending on your system's hardware and configuration.

---

## 1. Setting Up Disk and Partitions
- `lsblk`
- `cfdisk /dev/<diskname>`

## 2. Formatting the Partitions
- `mkfs.<filesystem type> /dev/<partition name>`
- `mkfs.fat -F 32 /dev/<partition name>`
- `mkswap /dev/<swap partition name>`

## 3. Mounting the Partitions
```bash
mount /dev/<rootpartition> /mnt
swapon /dev/<swap partition name>
mkdir -p /mnt/boot/efi
mount /dev/<efi partition> /mnt/boot/efi
```
> **Note:** Use `lsblk` to verify mount points.

## 4. Install Essential Packages
```bash
pacstrap -K /mnt base linux linux-firmware linux-headers sof-firmware base-devel amd-ucode nano networkmanager grub efibootmgr
genfstab -U /mnt >> /mnt/etc/fstab
```

## 5. Setting Up the Root Partition
```bash
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
nano /etc/locale.gen
locale-gen
nano /etc/locale.conf
# Add the following line:
LANG=en_IN.UTF-8
nano /etc/hostname
mkinitcpio -P  # (optional)
```

## 6. Adding a User to the Wheel Group
```bash
passwd  # Set root password
useradd -m -G wheel -s /bin/bash <username>
EDITOR=nano visudo
# Uncomment the line to allow wheel group permissions.
```

## 7. Installing the GRUB Bootloader
```bash
grub-install /dev/<disk name>
grub-mkconfig -o /boot/grub/grub.cfg
systemctl enable networkmanager
```

## 8. Unmounting the Partitions and Reboot
```bash
exit
umount -a
reboot
```

## 9. Post Install
```bash
sudo pacman -S plasma sddm firefox kate konsole
sudo systemctl enable --now sddm
```

---

## License
This guide is licensed under the GNU General Public License v3.0. You are free to use, modify, and distribute it under the terms of this license.

For more details, see the [GNU General Public License](https://www.gnu.org/licenses/gpl-3.0.en.html).
