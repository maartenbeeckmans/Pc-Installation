# Arch Linux installation (EFI)

## Step 0: Preparation

* Download latest image from [Linux arch](https://www.archlinux.org)

### Test internet connection

```bash
    ip a
    ping -c5 google.be
```

When not using dhcp

```bash
    ifconfig eno167777736 192.168.x.x netmask 255.255.255.0
    route add default gw 192.168.1.1
    echo "nameserver 8.8.8.8" >> /etc/resolv.conf
```

## Step 1: partition hard disks

List all hard disks in the machine

```bash
    cat /proc/partitons
    ls /dev/sd*
    ls /dev | grep '^[s|v|x][v|d]'$*
    fdisk -l
```

### Partition table

| Type                  | Mounting point    | Size              | Formatation   |
| :--                   | :--               | :--               | :--           |
| EFI System partition  | `/dev/sda1`       | 512M              | FAT32         |
| Swap partition        | `/dev/sda2`       | 2xRAM             | Swap On       |
| Root partition        | `/dev/sda3`       | at least 20 GB    | ext4          |

```bash
    cfdisk /dev/sda         # Semi graphical user interface
    # Check after installation
    fdisk -l
```

### Format partitions

```bash
    mkfs.fat -F32 /dev/sda1 # Format efi partition
    mkfs.ext4 /dev/sda3     # Format root partiton
    mkswap /dev/sda2        # Format swap partition
```

## Step 2: install Arch Linux

### Mount the partitions

```bash
    mount /dev/sda3 /mnt    # Mount root partition
    ls /mnt                 # Check - only lost+found here
    swapon /dev/sda2        # Mount swap partition
```

### Put closest mirror on top of the mirrorlist

```bash
    File path: /etc/pacman.d/mirrorlist
```

### Enable Arch Multilib (optional - 32bit operating system)

```bash
    # Uncomment this in /etc/pacman.conf
    [multilib]
    Include = /etc/pacman.d/mirrorlist
```

### Start Linux Arch installation

Takes 5 - 20 mins to complete installation, depending on the internet connection

```bash
    pacstrap /mnt base base-devel
```

## Generate fstab file

The *fstab (5)* file can be used to define how disk partitions, various other block devices, or remote filesystems should be mounted into the filesystem.

```bash
    # Generate
    genstab -U -p /mnt >> /mnt/etc/fstab
    # Inspect file
    cat /mnt/etc/fstab
```

## Step 3: Arch Linux system configuration

### Change root and add hostname

```bash
    # Change root to new disk
    arch-chroot /mnt
    # Change hostname
    echo "hostname" > /etc/hostname
```

### Configure system language

```bash
    # Uncomment encoding languages
    File path: /etc/locale.gen
    # Example en-us:
        # en_US.UTF-8 UTF-8
        # en_US ISO-8859-1
    # Generate system language layout
    locale-gen
    echo LANG=en_US.UTF-8 > /etc/locale.conf
    export LANG=en_US.UTF-8
```

### Configure system time zone

```bash
    # Set Time zone
    ln -sf /usr/share/zoneinfo/Europe/Brussels /etc/localtime
    # Set hardware clock to UTC
    hwclock --systohc --utc
```

### Update system

```bash
    pacman -Syu
```

### User configuration

```bash
    # set password user account
    passwd
    # add user
    useradd =mg users -G wheel,storage,power -s /bin/bash user_name
    # set password user_name
    passwd user_name
    # install sudo package + give user sudo rights
    pacman -S sudo
    visudo
        # uncomment this line
        %wheel ALL=(ALL) ALL
```

### Install boot loader (GRUB)

```bash
    pacman -S grub efibootmgr dosfstools os-prober mtools
    mkdir /boot/EFI
    mount /dev/sda1 /boot/EFI
    grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
    # Create grub configuration file
    grub-mkconfig -o /boot/grub/grub.cfg
```

### Finish installation

```bash
    exit
    umount -a
    telinit 6
```

## Step 4: first boot

```bash
    # enable dhcp
    sudo systemctl start dhcpcd
    sudo systemctl enable dhcpd
    ip a
    ping -c5 google.be
```

You can now install user interface

[Official Arch installation guide](https://wiki.archlinux.org/index.php/Installation_guide#Time_zone)
source: [tecmint](https://www.tecmint.com/arch-linux-installation-and-configuration-guide/)