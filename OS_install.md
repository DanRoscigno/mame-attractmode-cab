# OS Inatall

I am using Arch Linux.

## Install process

My old Dell OptiPlex 3200 boots uses secure boot by default, based on my reading of the Arch wiki I need to be out
of secure boot to install Arch. I disabled secure boot, and so I had to follow the instructions for a Legacy BIOS install.

Some are installed during the Arch install, and some are installed
after booting into the new system.

## All of the packages installed on my system

This is the output of:
```bash
expac "%-20n\t\# %10d\\" $(comm -23 <(pacman -Qqen | sort) <({ pacman -Qqg xorg; expac -l '\n' '%E' base; } | sort -u))
```

More info on using expac and other commands to list packages is on the [pacman Tips and Tricks](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks), which is where I got the above command.

```
alsa-utils          	# Advanced Linux Sound Architecture - Utilities
base                	# Minimal package set to define a basic Arch Linux installation
base-devel          	# Basic tools to build Arch Linux packages
dhclient            	# A standalone DHCP client from the dhcp package
dhcpcd              	# DHCP/ IPv4LL/ IPv6RA/ DHCPv6 client
expac               	# alpm data (pacman database) extraction utility
fakeroot            	# Tool for simulating superuser privileges
git                 	# the fast distributed version control system
grub                	# GNU GRand Unified Bootloader (2)
iwd                 	# Internet Wireless Daemon
linux               	# The Linux kernel and modules
linux-firmware      	# Firmware files for Linux
mame                	# Port of the popular Multiple Arcade Machine Emulator using SDL with OpenGL support
neovim              	# Fork of Vim aiming to improve user experience, plugins, and GUIs
netctl              	# Profile based systemd network management
openbox             	# Highly configurable and lightweight X11 window manager
openssh             	# SSH protocol implementation for remote login, command execution and file transfer
pipewire-alsa       	# Low-latency audio/video router and processor - ALSA configuration
rsync               	# A fast and versatile file copying tool for remote and local files
screen              	# Full-screen window manager that multiplexes a physical terminal
sudo                	# Give certain users the ability to run some commands as root
vim                 	# Vi Improved, a highly configurable, improved version of the vi text editor
wpa_supplicant      	# A utility providing key negotiation for WPA wireless networks
xautolock           	# An automatic X screen-locker/screen-saver
xorg-fonts-misc     	# X.org misc fonts
xorg-fonts-type1    	# X.org Type1 fonts
xorg-xinit          	# X.Org initialisation program
xterm               	# X Terminal Emulator
```

During the Arch install you could run:

```bash
pacman -S \
alsa-utils \
base \
base-devel \
dhclient \
dhcpcd \
expac \
fakeroot \
git \
grub \
iwd \
linux \
linux-firmware \
mame \
neovim \
netctl \
openbox \
openssh \
pipewire-alsa \
rsync \
screen \
sudo \
vim \
wpa_supplicant \
xautolock \
xorg-fonts-misc \
xorg-fonts-type1 \
xorg-xinit \
xterm
```

## Installed after booting the new system

Attract mode for MAME can bu installed from the community maintained packages (from source):

```bash
mkdir GitHub
Build and install attract mode
cd GitHub/
git clone https://aur.archlinux.org/attract.git
cd attract/
makepkg -sirc
attract --help
```
