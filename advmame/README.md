### From an old email
```
Here is how I setup Fedora 7 to boot directly into Wah!Cade:

0) Setup Fedora to boot without GDM or KDM (in fact, I do not have
Gnome or KDE installed)
1) Ran 'startx' as user mame to get X running
2) Edited /etc/X11/xinit/Xclients to start Wah!Cade and nothing else:

If you look in this file you might see the section where twm is
started as a last resort. I edited this section commenting out the
clock, Xterm and twm, and adding Wah!Cade:

# Argh! Nothing good is installed. Fall back to twm
{
# gosh, neither fvwm95 nor fvwm2 is available;
# fall back to failsafe settings
[ -x /usr/bin/xsetroot ] && /usr/bin/xsetroot -solid '#222E45'

#if [ -x /usr/bin/xclock ] ; then
#/usr/bin/xclock -geometry 100x100-5+5 &
#elif [ -x /usr/bin/xclock ] ; then
#/usr/bin/xclock -geometry 100x100-5+5 &
#fi
#if [ -x /usr/bin/xterm ] ; then
#/usr/bin/xterm -geometry 80x50-50+150 &
#fi

if [ -x /usr/games/wahcade ] ; then
/usr/games/wahcade --full-screen
fi

#if [ -x /usr/bin/firefox -a -f /usr/share/doc/HTML/index.html ]; then
#/usr/bin/firefox /usr/share/doc/HTML/index.html &
#fi
#if [ -x /usr/bin/twm ] ; then
#exec /usr/bin/twm
#fi
}


3) Exited X by hitting CTRL-ALT-Backspace

4) Started X again by typing 'startx' This time I get just WahCade,
with no window manager. To get back to the command prompt I hit escape
to exit WahCade and X shuts down

5) Setup Fedora to log user mame in automatically on tty1 at boot by
editing /etc/inittab and replacing this line:

1:2345:respawn:/sbin/mingetty tty1

with this one:

1:3:respawn:/sbin/mingetty --autologin mame tty1

7) Test the inittab change by rebooting, sure enough mame got logged
in automatically

8) Setup mame's ~/.bash_profile to start X at login if the login is on
tty1 by adding this to ~/.bash_profile:

# start X if we are logged in on tty1:
case "`tty`" in
/dev/tty1)
startx && exit
;;
esac

9) Tested to see that X starts automagically by logging out from tty1,
there is no need to reboot as the 'respawn' instruction in inittab
automatically logs mame back in whenever that user logs out from tty1.
This has the side effect of restarting WahCade if the player hits
escape and exits completely

Let me know if this does not work on Ubuntu, I can load Ubuntu and
work out the details if needed.
```
