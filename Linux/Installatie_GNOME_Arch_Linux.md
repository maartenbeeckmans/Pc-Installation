# GNOME installation Arch Linux

## Update all packages

```bash
    sudo pacman -Syu
    sudo reboot
```

## Installation

```bash
    # install xorg
    sudo pacman -S xorg xorg-server
    # install gnome DE
    sudo pacman -S gnome
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
    # Installation
    sudo pacman -S lxdm
    # Stop any other running DMs
    sudo systemctl stop gdm.service
    sudo systemctl disable gdm.service
    # Start and enable lxdm service at boot
    sudo systemctl start lxdm.service
    sudo systemctl enable lxdm.service
```

## Reboot system

```bash
    sudo reboot
```