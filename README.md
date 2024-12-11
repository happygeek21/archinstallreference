# My Personal Arch Install Commands

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
