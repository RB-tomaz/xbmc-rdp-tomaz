xbmc-rdp-tomaz
==============

xbmc-rdp for RaspberryPi. Compiled flowing http://www.raspbian.org/RaspbianXBMC and added some of my ideas implemented with google help :)


http://www.raspberrypi.org/phpBB3/viewtopic.php?f=66&t=12455

Hi,

almost week ago I tried to compile XBMC and I was surprised, first tray and I received:

XBMC built successfully


Here is prepared package (my first, just googling the way how to do it:) for XMBC for Raspberry pi users (normal users)!Almost no configuration needed :) 

Motivated by RasPiThon group, I decided to make XMBC.deb, for "normal" users. If they can code game like this in 48h I can do XBMC.deb in 120h hehe, not far from true.

And I know my English sucks and you cannot wait to try XBMC, you can go directly to line: "4.)" :)

Best installing method would be of course "apt-get install xbmc", but till then...

I will start from beginning:

1.) Write latest image: 2012-07-15-wheezy-raspbian to your SD card.

2.) Start Raspberry pi and configure basic stuff (keyboard, re-size partition, start ssh,...) Raspi-conig which starts on boot helps a lot :)

3.) Update your Raspberry

    sudo apt-get update
    sudo apt-get upgrade  

**4.) Install compiled xbmc-rdp-tomaz.armhf.deb (and all dependencies needed):**

https://github.com/downloads/RB-tomaz/xbmc-rdp-tomaz/xbmc-rdp-tomaz.armhf.deb

    wget https://github.com/downloads/RB-tomaz/xbmc-rdp-tomaz/xbmc-rdp-tomaz.armhf.deb
    sudo dpkg -i xbmc-rdp-tomaz.armhf.deb; sudo apt-get -y -f install  

Answer question about autostart of XBMC*

*I have to explain little more:

- If you select Yes XMBC will start when you power your Raspberry Pi, with one exception:
You will be asked to press any key, if you DON'T want XBMC to start! You will have 5 sec, otherwise it will continue and start XMBC. If you press any key, you will get prompt and you can still manually start xbmc, startx or do whatever you like.
So you have total control :)
This will also disable autostart of X, if you have enabled it with raspi-config in 2.)
- If you select No your /etc/inittab will not be changed!
You will have to login and start XBMC with command "xbmc".

Answer question about mounting /dev/sda1 and /dev/sdb1 USB/HDD disks when starting XBMC

*I have to explain little more:

- If you select Yes XBMC will try to mount /dev/sda1 to /mnt/xbmc1 and /dev/sdb1 to /mnt/sdb2
- If you select No XBMC will not try to mount

5.) Reboot

6.) Manually start xbmc (if not started automatically): I think this version is running OK without root access.

    xbmc

7.) You can safely delete xbmc-rdp-tomaz.armhf.deb, if you want.

8.) Please report, if it is working for you. Or more important, if it is not working :)

Probably I will not know how to help you, because I've juste follow this page to compile XBMC and found all stuff on google and on raspi-conig

I did learn how to create basic .deb and how to install (with all dependencies) and uninstall .deb. Not sure, if I have put dependencies correctly inside .deb, we will see when it will stop installing :) Today I have tested whole procedure with new SD card and it worked for me.

The last thing is of course, how you uninstall/remove XBMC. This part can be improved more... You can remove it with one line (XBMC, all dependencies, specific settings and "pi" user settings in folder ~/.xbmc):

To remove XBMC package:

    sudo dpkg -r xbmc-rdp-tomaz;sudo apt-get -y autoremove

It will ask you, if you want to start X on boot. This is because I don't know, what you had prior to installng this package. This could be TODO: to save settings of autostart X before installing package. Easy?

I just remember when uninstalling "disable_overscan=1" will stay unchanged. This you can be easily set with "sudo raspi-config".
Of course, could also be saved before installing package (preinst), another TODO for someone :)

===============================================
================ADVANCED USERS:================
===============================================

Some people don't like any changes made to the system, I totally understand them! That's the reason I'm actually preparing this package, because I don't like to have whole image custom made by one person with keyboard settings, local settings set and so on. But hay If I'm talking about one person, probably some good package out there on apt-get was prepared by just one person, who knows :)

Thanks to silas and his image I started to believe, it is possible to have XBMC on Pi! Don't kill me silas for making this .deb package ;)

So here is packet, with "untuched" XBMC binary, just packed in .deb:

https://github.com/downloads/RB-tomaz/xbmc-rdp/xbmc-rdp.armhf.deb

    wget https://github.com/downloads/RB-tomaz/xbmc-rdp/xbmc-rdp.armhf.deb
    sudo dpkg -i xbmc-rdp.armhf.deb; sudo apt-get -y -f install
   

and uninstalling:

     sudo dpkg -r xbmc-rdp;sudo apt-get -y autoremove
   

It will install it as when you run "make install", and it will install all dependencies needed. Every other settings are up to you. You have to start it like: "clear ; /usr/lib/xbmc/xbmc.bin". Don't forget about memory. (sudo cp /boot/arm128_start.elf /boot/start.elf)
For shutdown solution I suggest you make file: /var/lib/polkit-1/localauthority/50-local.d/20-xbmclive.pkla and inside:

    [Actions for xbmc user]
    Identity=unix-user:pi
    Action=org.freedesktop.upower.*;org.freedesktop.consolekit.system.*;org.freedesktop.udisks.*
    ResultActive=yes
    ResultAny=auth_admin
    ResultInactive=yes

If you want to mount something (I remember I saw someone asking for NFS) before XMBC starts? 
You can edit file /usr/bin/xbmc and mount whatever you like before line "clear ; /usr/lib/xbmc/xbmc.bin"

===============================================
=================END? Not yet?=================
===============================================
-------------------------------
For more cureous people, below:
-------------------------------
With postinst script in .deb I tried to fast fix some stuff people notice in this topic: 
Instructions for building XBMC for Raspbian Thanks to Obsys, mpthompson, silas, Hexxeh (on IRC) and many many others, very long thread, I didn't fix all problems/ideas, but just few.

Uninstall will (for xbmc-rdp-tomaz.armhf.deb, which have set some specific settings):

1.) Remove XBMC and,

2.) copy /boot/arm224_start.elf to /boot/start.elf

3.) Delete autostart of XBMC, if exists

4.) delete autologin of user pi, if exists

5.) Ask if you want to boot directly to X desktop.

6.) Disable mounting of USB/HDD (/dev/sda1 and /dev/sdb1)

7.) Delete user settings in ~/.xbmc folder

------------------------------------
For even more cureous people, below:
------------------------------------
I was thinking, if I prepare .deb file with dependencies it will install them directly, I was mistaken, so that's why there is workaround/solution with "...; sudo apt-get -y -f install"

You can see basic information about installed package:

    sudo dpkg -l xbmc-rdp-tomaz

You can see all files in installed package like this:

    sudo dpkg -L xbmc-rdp-tomaz

If you don't like autostart of XMBC open file .bashrc and delete last 3 lines:

    case "`tty`" in # XBMC - start only for tty1
     *tty1) xbmc-ask;; # XBMC - start XBMC
    esac # XBMC - end of case

If you don't like autologin open file /etc/inittab and uncomment line # XBMC_NORMALLOGIN and comment line # XBMC_AUTOLOGIN like this:

    1:2345:respawn:/sbin/getty --noclear 38400 tty1 # XBMC_NORMALLOGIN
    \#1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>\&1 # XBMC_AUTOLOGIN

If you don't like XMBC to mount your devices simple delite two files:

    rm ~/.automountUSBforXBMC
    rm ~/.autoumountUSBforXBMC

If you would like XBMC to mount your devices simple create (same) two files:

    touch ~/.automountUSBforXBMC
    touch ~/.autoumountUSBforXBMC

If you don't want XBMC to unmount your USB/HDD devices just delete file 

    rm ~/.autoumountUSBforXBMC

If you want more or less seconds for timeout on "press any key to" cancel start of XBMC, you can simply edit
file xbmc-ask and change line "timeout" to whatever you prefer.

    sudo vi /usr/bin/xbmc-ask
    timeout=5

------------------------------------
All files: on github, learned how to use it, while watching RasPiThon maraton. I watched almost whole 48h of RasPiThon maraton, hehe, link.
Versions:

- Primary (recommended): https://github.com/RB-tomaz/xbmc-rdp-tomaz.git
- Basic (just control file, nothing else changed): https://github.com/RB-tomaz/xbmc-rdp.git

------------------------------------

How to create .deb file yourself?

Very simple, you need to be in "arm224_start.elf". Now you should know what I'm talking about? Hit: sudo cp /boot/...

Google say the simplest way to build .deb from "make install" is CheckInstall, but Raspberry run out of memory. It uses all /tmp folder. I tried also "bzr" and same problem with /tmp. This bzr at leaset gave me error: "bzr -Derror dh-make -v test 1.0 ./test.tar.gz" but, couldn't figure it out. Then simply created ./DEBIAN/control file and that was it:) Simple, I say now; at the beginning it seemed impossible.

Building .deb from this (primary) git:

    sudo apt-get install git
    cd 
    git clone https://github.com/RB-tomaz/xbmc-rdp-tomaz.git
    rm -r xbmc-rdp-tomaz/.git #or mv ./xbmc-rdp-tomaz/.git ~
    dpkg -b ./xbmc-rdp-tomaz/ xbmc-rdp-tomaz.armhf.deb

If you receive error:
dpkg-deb (subprocess): data member: internal gzip write error: 'No space left on device'
dpkg-deb: error: subprocess <compress> from tar -cf returned error exit status 2
I got this error when checking procedure written here, because I forgot to set memory to 224. Solution I found is this:

    cd
    mkdir tmp
    export TMPDIR=/home/pi/tmp

and try again. Probably this would also work with CheckInstall and bzr!? I think setting variable TMPDIR is very good solution, especially for some future bigger projects:)

For your own "make": This I didn't test exactly as written. I'm writing this from my head so this might not actually work...

    mkdir ~/xbmc-deb
    cd ~/xbmc-rbp
    sudo make install [B]DESTDIR=/home/pi/xbmc-deb[/B]
    cd ~/xbmc-deb
    mkdir DEBIAN
    cd DEBIAN
    vi control # you have to prepare control file
    vi postinst # optional, if you want to do something after installing package
    vi postrm #optional, if you want to do something after removing package
    cd
    dpkg -b ./xbmc-deb/ xbmc-rdp.armhf.deb

P.S.: If you start xbmc and go to Video-Files and screen start blinking you didn't reboot, you need arm128_start.elf !

P.S.2: If you have problems with xbmc you should check power supply, I tried with 500mA and it was not engouh. With 700mA everything seems OK :)

P.S.3: If you like vi in colors I suggest you do:

    sudo apt-get install vim

and create file ~/.vimrc with lines:
:syntax on
:color torte # i like this profile more than default

P.S.4: I have added advancedsettings.xml for XMBC, thanks workshy:

    <advancedsettings>
     <gui>
      <algorithmdirtyregions>3</algorithmdirtyregions>
     <nofliptimeout>0</nofliptimeout>
     </gui>
    </advancedsettings>

P.S.5:If you have problems with composite video out when using XBMC (go to Settings->Video->Playback... don't remember now what I set ther but maybe will help you to know wher to look)

P.S.6: I think I just used more time writing this than making .deb :) Funny.

Someone could prepare better manual out of this ?

Last modification: I decided file name xbmc-rdp-tomaz.armhf.deb and xbmc-rdp.armhf.deb will be without version numbers, so it will be easier to update file on github, without modifying this post. I will give write access to person(s) who will prepare new .deb or modify control, postinst,... Especially correct my strange sentences...


And last question, anyone know how "apt-get install xbmc" would be possible?


I wish you all nice Raspberring. (RaspberryPiing hehe).


This message was send from Raspberry, ups not yet :)

Tomaz.

http://www.raspberrypi.org/phpBB3/viewtopic.php?f=66&t=12455

