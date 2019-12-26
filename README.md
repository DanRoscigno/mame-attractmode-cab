# mame-attractmode-cab

## Install Ubuntu Server
I used 16.04 ubuntu SERVER iso:
`http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso`
18.04 has a different version of freetype and causes compile errors.

### Installation choices
During the installation you will be asked what packages you want.  The `standard system utilities` will be pre-selected, keep that one and add `openssh server` if you like to be able to connect remotely.

### Let the install finish and reboot

### Add tools and libraries

At this point this is a commandline (no Xwindows) system.  Install the basics for X, compiling, libraries, and pre-requisites for MAME and Attract Mode.
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
#### Note: You must run this next command from the console, not from a remote (ssh) connection:

Command from https://linuxconfig.org/how-to-run-x-applications-without-a-desktop-or-a-wm

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

## 
