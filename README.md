# mame-attractmode-cab

## Add a new game to Attract Mode

- Get `catver.init` from https://www.progettosnaps.net/catver/
- scp `catver.ini` to `/home/rastan/mame/catver.ini`
- ssh in to the game as user `rastan`
- run `attract --build-romlist mame --full`
- the above command will generate a new mame.txt, find the game you want in the new file and add the corresponding line to `.attract/romlists/mame.txt`
  for example, to add the 25th anniversary PacMan:
  ```
  25pacman;25pacman;mame;;;;MultiGame / Compilation;;;;;;;;;;;;;;
  ```

## OS Install

[OS_install](./OS_install.md)

## Configure WiFi

## Configure Attract mode and MAME

## OLD

Update: December 2023

Install Ubuntu **server** 22.04 LTS

Accept defaults, except for the ssh checkbox, make sure that this is selected

Pay attention when setting up the disk, I ended up with a 100GB logical volume and had to extend it. This is how I extended it to 300GB:

```bash
sudo lvextend -L 300GB /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

```bash
sudo apt update && sudo apt upgrade
```

```bash
sudo apt-get install libsdl2-dev
```

```bash
sudo apt install libsfml-dev \
                 libavutil-dev \
                 ffmpeg \
                 libtiff-dev \
                 libfreetype6-dev \
                 libexpat1-dev \
                 libavcodec-dev \
                 libavformat-dev \
                 pkg-config \
                 libswscale-dev \
                 libalut-dev \
                 libglfw3-dev \
                 libgl1-mesa-dev \
                 libglu1-mesa-dev \
                 libfontconfig1-dev \
                 qtbase5-dev \
                 libsdl2-ttf-dev \
                 libxrandr-dev \
                 libjpeg-dev \
                 libarchive-dev \
                 libswresample-dev \
                 libopenal-dev \
                 xinit \
                 openbox \
                 menu \
                 x11-xserver-utils \
                 python3-xdg \
                 fonts-recommended \
                 xorg-fonts-misc \
                 git
```

```bash
git clone https://github.com/mamedev/mame.git
```

```bash
cd mame
make -j $(cat /proc/cpuinfo | grep -c processor)
```

```bash
cd ~/
scp -r droscigno@mini.local:/Volumes/Annex/arcade/mame .
```
==================================================================================

OLD

## Install Ubuntu Server
I used 16.04 ubuntu SERVER iso:
`http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso`
18.04 has a different version of freetype and causes compile errors.

### Installation choices
During the installation you will be asked what packages you want.  The `standard system utilities` will be pre-selected, keep that one and add `openssh server` if you like to be able to connect remotely.

### Let the install finish and reboot

### Setup sound

See this page for the sound bits: http://forum.arcadecontrols.com/index.php/topic,156122.0.html
Stop after adjusting sound with `alsamixer` (do not install openbox)

Note: Substitute your username for `rastan` (my cab is an old Rastan cab):
```
sudo apt-get install alsa-utils
sudo usermod -a -G audio rastan
sudo alsamixer
```
### Reboot
Reboot after installing and configuring audio or MAME will hang when you try to exit a game.

### Add tools and libraries

At this point this is a commandline (no Xwindows) system.  Install the basics for X, compiling, libraries, and pre-requisites for MAME and Attract Mode.
```
sudo apt-get update && sudo apt-get upgrade

sudo apt-get -y install git libsfml-dev libopenal-dev \
libavformat-dev libfontconfig1-dev libfreetype6-dev libswscale-dev \
libavresample-dev libarchive-dev libjpeg-dev libglu1-mesa-dev \
xorg xterm build-essential
```

### Update the compiler
MAME .217 requires GCC 7.x:

Don't go wild copying and pasting all of the commands at once, run them line by line as some of the commands are interactive.

```
sudo apt-get install -y software-properties-common
```

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```

```
sudo apt update
```

```
sudo apt install g++-7 -y
```

```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 60 \
                         --slave /usr/bin/g++ g++ /usr/bin/g++-7 
```

```
sudo update-alternatives --config gcc
```

```
gcc --version
```

```
g++ --version
```

## Install MAME

```
sudo apt-get install git build-essential python libsdl2-dev libsdl2-ttf-dev libfontconfig-dev qt5-default
```

```
mkdir mame-src
cd mame-src
wget https://github.com/mamedev/mame/archive/mame0217.tar.gz
```

untar

```
make -j3
```

```
sudo mkdir -p /usr/local/games/mame && sudo cp -r mame64 plugins /usr/local/games/mame/
```

### Test with a ROM from mamedev.org

```
wget https://www.mamedev.org/roms/supertnk/supertnk.zip # from mamedev.org
mkdir -p ~/mame/roms
mv supertnk.zip ~/mame/roms
```
#### Note: You must run this next command from the console, not from a remote (ssh) connection:

Command from https://linuxconfig.org/how-to-run-x-applications-without-a-desktop-or-a-wm

```
xinit /usr/local/games/mame/mame64 -rp $HOME/mame/roms supertnk $* -- :0 vt$XDG_VTNR
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
```
### Test Attract Mode
Choose language and let it add emulators that are found (MAME)

```
xinit /usr/local/bin/attract $* -- :0 vt$XDG_VTNR
```

ESC quits

## Setup auto login

Note: You can read more about this at https://askubuntu.com/questions/659267/how-do-i-override-or-configure-systemd-services

Edit the `getty` process for `tty1`
```
sudo systemctl edit getty@tty1
```

Paste in these lines (substitute your username for `rastan` (my cab is an old Rastan cab):
```
[Service]
ExecStart=
ExecStart=-/sbin/agetty -a rastan --noclear %I $TERM
```

## Test auto login
Reboot.  You should be automatically logged in.

## Configure Attract-Mode to auto start
By adding the `xinit` command used earlier to start Attract-Mode to your `.profile` you can start Attract-Mode at boot.  Edit your .profile and add these lines to the bottom:
```
# start attract if we are logged in on tty1:
case "`tty`" in
    /dev/tty1)
                xinit /usr/local/bin/attract $* -- :0 vt$XDG_VTNR 
                ;;
esac
```


## Test Attract-Mode auto start
Reboot.  You should be automatically logged in, and Attract-Mode should start.  Play Super Tank, and then exit Super Tank and Attract-Mode (`ESC`, `ESC`).

## Have Attract-Mode auto-restart
If you want Attract-Mode to restart when a player exits Attract-Mode, then modify the `.profile` adding `&& exit` (the getty process will log you back in, which will restart Attract-Mode)

```
# start attract if we are logged in on tty1:
case "`tty`" in
    /dev/tty1)
                xinit /usr/local/bin/attract $* -- :0 vt$XDG_VTNR && exit
                ;;
esac
```

# Artwork

See http://forum.attractmode.org/index.php?topic=344.0 to acquire artwork 

I think that these are the default directories in which the themes, or layouts, look for the artwork:

```
mkdir -p $HOME/mame/flyer  $HOME/mame/marquee  $HOME/mame/roms  $HOME/mame/snap  $HOME/mame/wheel
```

Place the artwork in the above directories.  If Attract-Mode is running while you do this you will see the artwork in Attract-Mode without restarting.

# Audio
I had no audio output at the headphone jack.  To find the default soundcard (I have two) I ran `aplay -l`:
```
rastan@arcade:/etc$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: HDMI [HDA Intel HDMI], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: PCH [HDA Intel PCH], device 0: ALC3220 Analog [ALC3220 Analog]
  Subdevices: 0/1
  Subdevice #0: subdevice #0
```

`Card 1` is the one I want, so I set the default by adding the file `/etc/asound.cnf` with this content:
```
defaults.pcm.card 1
defaults.ctl.card 1
```

I then rebooted.

# Romlists
I only have a few games.  The ones I have are in the [`.attract/romlists/mame.txt`](https://github.com/DanRoscigno/mame-attractmode-cab/blob/master/romlists/mame.txt).  I have had no luck with generating the files with filters etc, it is easier for me to just grab a full list and edit it down to the games I have.

# Layouts
I use [Yaron's At The Arcade](http://forum.attractmode.org/index.php?topic=3016.0) theme.  I unzip it into `.attract/layouts/` and then edit `.attract/attract.cfg` and set the layout there:
```
# Generated by Attract-Mode v2.6.0
#
display	mame
	layout               At-The-Arcade
	romlist              mame
	in_cycle             yes
	in_menu              yes
```
