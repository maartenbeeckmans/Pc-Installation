# GNOME install Arch Linux

## Update all packages

```bash
    sudo pacman -Syu
```

## Installation

```bash
    sudo pacman -S xorg xorg-server     # install xorg
    sudo pacman -S gnome                # install gnome DE
```

## Enable gdm service

```bash
    sudo systemctl start gdm.service
    sudo systemctl enable gdm.service
```

### Other display managers

* **LightDM** - Cross-desktop display manager, can use various front-ends in any toolkit
* **LXDM** - LXDE display manager, can be used independent of the LXDE desktop environment
* **MDM** - MDM display manager, used in Linux Mint, a fork of GDM 2
* **SDDM** - QLM-based display manager and successor to KDE4,s kdm; recommended for Plasma 5 en LXQT
* **XDM** - X display manager with support for XDMCP, host chooser

For example, if you want to install LXDM for GNOME DE, run:

```bash
    sudo pacman -S lxdm                     # Installation
    sudo systemctl stop gdm.service         # Stop any other running DMs
    sudo systemctl disable gdm.service
    sudo systemctl start lxdm.service       # Start and enable lxdm service at boot
    sudo systemctl enable lxdm.service
```

## Reboot system

<link rel="stylesheet" href="../style.css">