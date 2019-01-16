# Dual boot

## Preparation

This guide is made for UEFI Operating Systems.

1. Disable Secure Boot in the UEFI
2. Diasble Fast Start-Up Windows 10

## Partitioning Linux

It is recommended to install Windows first. When you have finished the Windows setup, boot into the Linux installation environment where you can create and resize partitions for Linux without leaving the existing Windows partitions untouched. The Windows installation will create the *EFI system partition* which can be used by your Linuc boot loader.

### Partitions on a Windows System

| Name | Size | Containing? |
| :-- | :-- | :-- |
| Windows Recovery Environment | 499M | Contains files required to boot Windows (equivalent to `/`) |
| EFI System Partition | 100M | Formatted with FAT32 filesystem |
| Microsoft Reserved Partition | Generally 128M | |
| Microsoft basic data partiton | Differs | The Windows `C:` drive |
| Potentially system recovery and backup partitions and/or secondary Data Partition |

Check your patitionson your drive using the *Disk Management utility* in Windows. This will help you understand which partitions are essential to Windows, and which others you might repurpose.

***Warning:* the first 4 partitions in the table above list are essential, don't delete them!**

You can then proceed with partitioning, depending on your needs. Mind that there is no need to create an additional *EFI System Partition*, since it already exists.
The boot loader needs to support chainloading other EFI applications to do dual boot Windows/Linux.

***Tip:* rEFInd will autodetect Windows Boot Manager (`\EFI\Microsoft\Boot\bootmgfw.efi`) and show their boot menu automatically. For GRUB follow the [GRUB#Windows installed in UEFI/GPT mode](https://wiki.archlinux.org/index.php/GRUB#Windows_installed_in_UEFI/GPT_mode) on the Arch Linux wiki**

Install Arch Linux as in the guide.

Source: [arch wiki](https://wiki.archlinux.org/index.php/Dual_boot_with_Window)

<link rel="stylesheet" href="../style.css">