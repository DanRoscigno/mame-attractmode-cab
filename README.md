# mame-attractmode-cab

## 16.04 ubuntu SERVER iso:
`http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso`

### install standard system utilities and openssh server during install

## At this point this is a commandline (no Xwindows) system
```
sudo apt-get update && sudo apt-get -y install git libsfml-dev libopenal-dev \
libavformat-dev libfontconfig1-dev libfreetype6-dev libswscale-dev \
libavresample-dev libarchive-dev libjpeg-dev libglu1-mesa-dev \
xorg xterm build-essential
```
## MAME
```
sudo add-apt-repository ppa:c.falco/mame
sudo apt-get update
sudo apt-get install mame mame-doc mame-extra mame-tools
wget https://www.mamedev.org/roms/supertnk/supertnk.zip # from mamedev.org
mkdir -p ~/mame/roms
mv supertnk.zip ~/mame/roms
xinit /usr/games/mame supertnk $* -- :0 vt$XDG_VTNR
```

## Follow this:
https://github.com/mickelson/attract/wiki/Compiling-on-Ubuntu-Bionic-Beaver-%2818.04-LTS%29-amd64-desktop

```
git clone http://github.com/mickelson/attract attract
cd attract
make -j $(cat /proc/cpuinfo | grep -c processor)
sudo make install

xinit /usr/games/attract $* -- :0 vt$XDG_VTNR
```
