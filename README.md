# mame-attractmode-cab

## Install Ubuntu Server
I used 16.04 ubuntu SERVER iso:
`http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso`
18.04 has a different version of freetype and causes compile errors.

### Installation choices
During the installation you will be asked what packages you want.  The `standard system utilities` will be pre-selected, keep that one and add `openssh server` if you like to be able to connect remotely.

### Let the install finish and reboot

### Add tools and libraries

At this point this is a commandline (no Xwindows) system.  Install the basics for compiling, libraries, and pre-requisites for MAME and Attract Mode.
```
sudo apt-get update && sudo apt-get -y install git libsfml-dev libopenal-dev \
libavformat-dev libfontconfig1-dev libfreetype6-dev libswscale-dev \
libavresample-dev libarchive-dev libjpeg-dev libglu1-mesa-dev \
xorg xterm build-essential
```
## Install MAME
```
sudo add-apt-repository ppa:c.falco/mame
sudo apt-get update
sudo apt-get install mame mame-doc mame-extra mame-tools
```

### Test with a ROM from mamedev.org

```
wget https://www.mamedev.org/roms/supertnk/supertnk.zip # from mamedev.org
mkdir -p ~/mame/roms
mv supertnk.zip ~/mame/roms
```
#### Note: You must run this nooext command from the console, not from a remote (ssh) connection:
```
xinit /usr/games/mame supertnk $* -- :0 vt$XDG_VTNR
```
ESC will quit

## Install Attract Mode
Follow this:
https://github.com/mickelson/attract/wiki/Compiling-on-Ubuntu-Bionic-Beaver-%2818.04-LTS%29-amd64-desktop

```
git clone http://github.com/mickelson/attract attract
cd attract
make -j $(cat /proc/cpuinfo | grep -c processor)
sudo make install

xinit /usr/games/attract $* -- :0 vt$XDG_VTNR
```
