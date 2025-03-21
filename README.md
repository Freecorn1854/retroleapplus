# retroleap+ is a modication of the original retroleap project but to extend functionality to all linux apps (not just retroarch)

### unmodified readme with install instructions:

retroleap is a replacement firmware for Leapfrog devices (currently for LeapsterGS and LeapPad2), that combines retroarch, ARM-optimized emulator cores, and a buildroot root filesystem.

You can easily download and install retroleap on your leapsterGS system! To do this, follow the instructions below on your Linux machine.

-------------------------------------------------------------------------------

Installing:

1. Install the necessary dependencies:

On ubuntu/debian/etc: sudo apt install sg3-utils git python

2. Clone sshflash (the git checkout command is required for flashing Retroleap 2.0.0 Alpha 4 and below)

git clone https://github.com/mac2612/sshflash.git
cd sshflash
git checkout 9db71e4

3. Download the latest release .tar.gz file at:

https://github.com/mac2612/retroleap/releases

4. Extract the release file into the sshflash directory. You should now have the uImage, rootfs.ubifs, and surgeon_zImage files in your sshflash directory.

5. Plug your Leapster into a USB port on your PC. Turn it on while holding the L and R shoulder buttons, along with the hint button. 
You should see an image of a leapster connected to a PC on the leapster's screen. If your Leapster goes through the usual boot sequence (LeapFrog logo, boot chime) before the connected screen, you've done it wrong.

6. Run sshflash:

./remote_flash.sh

7. Wait for sshflash to do its thing.

8. If all goes well, your leapfrog device should reboot into GMenu2x!

9. These instructions have been tested on Ubuntu 18.04, Ubuntu 18.10, MX-21.3, and Pop!_OS 21.04. If you run into trouble, please open a bug!


Button Layout:
On the LeapsterGS:
-A/B/Pause/Question buttons - B/A/X/Y
-L/R shoulders - L/R
-Volume down/up - select/start
-Home button - modifier key for retroarch stuff
-Home + Question - Go back to retroarch menu when in an emulator
-Home + vol up / down - Volume up/down
-Home + L shoulder - Save emulator state
-Home + R shoulder - Load emulator state
-Power - Power off. Quick press for graceful power-off, long press for force-shutdown.

On the LeapPad2:
LeapPad2 has quite a limited button layout, so it may not be good for systems with a lot of buttons like SNES.
-Note that the D-pad is rotated to match the orientation of retroarch (i.e. left arrow is up, etc)
-Vol down/up - B/A
-Down arrow key - Select (doubles as the normal down arrow key).
-Home button - Start, and also retroarch modifier key
-Home button + Up arrow - go back to retroarch menu when in an emulator
-Home button + left arrow - Save emulator state
-Home button + right arrow - Load emulator state
-Home button + Volume up/down - Volume up/down


Default Cores:

atari800 - a tweaked atari800 emulator modified for leapster by JabberwockPL
catsfc - SNES emulator. Performance is alright depending on the game.
fceumm - NES emulator, good speed and fairly accurate.
gpsp - GBA emulator. Runs well; you need to put your GBA bios at /roms/gba_bios.bin on the device (see 'Adding ROMs')
imame4all - MAME, performance depends on the game. 
picodrive - Genesis emulator, runs well on the games I've tried.
pocketsnes - SNES emulator, runs some games well but slow on others.
quicknes - NES emulator, low accuracy but very fast.


Adding ROMs:

When you attach a booted leapster device running retroleap, it will show up as a network interface.
Retroleap will give your PC an IP on this interface in the 169.254.6.X space. The leapster is accessible on 169.254.6.1, using the private key supplied with sshflash.

To add ROMs, create the directory structure that you prefer using SSH - it should be within /roms in order to get full use of the flash memory in the device.

Then, use scp to move ROMs into the directory you just made.

For example:

ssh -i ~/sshflash/keys/id_rsa root@169.254.6.1 'mkdir /roms/nes'
scp -i ~/sshflash/keys/id_rsa /home/foobar/my_awesome_homebrew.nes root@169.254.6.1:/roms/nes

You can use df -h to see how much space you have available:

ssh -i ~/sshflash/keys/id_rsa root@169.254.6.1 'df -h /roms'


Emergency:
If you're stuck and your device is in a weird state, you can always go back to the stock leapster OS by booting your device in Surgeon mode (hold L + R shoulder buttons while powering up), and connecting to the Leapfrog Connect app. This will allow you to restore the stock leapster OS on the device.

Additionally, you can get serial access to the device either via the cartridge port, or via a header on the inside of the device.

Retroleap does not replace the stock bootloader, so unless something really nasty happens, you should be safe from bricking your device.


Doing your own build:

To do a build, clone the source tree with git.
Then do this:

make lfXXXX_defconfig (where XXXX is the board version you are building i.e. lf1000, lf2000 or lf3000)
make menuconfig (change whatever you want to change here - adding cores, etc.)
make
cp output/images/* ~/sshflash/ (equivalent for your system/location of SSHflash).


Wishlist/planned features:
-More cores to support additional systems.
-Overlay support for the tablet form-factor devices to make up for lack of buttons.
-Support for more leapster devices e.g. leapster explorer, didj(?), leappad, leappad3 and platinum.
-Overclocking support.
-Menuing other than the retroarch rgui menuing system.

If you'd like to get involved with this project, please get in touch! New contributors are always welcome :)


Shoutouts/thank yous:
-JabberwockPL (https://github.com/JabberwockPL) for being a trusted tester and collaborator, and doing cool stuff with atari800 cores, which will soon be integrated into retroleap!
-Special thanks for jrspruitt (https://github.com/jrspruitt) who wrote a bunch of the low-level code to talk to leapfrog's bootloader as part of OpenLFConnect
-The retroarch team and all the folks writing ARM-optimized cores that run so well on very dinky ARM systems (notaz especially!)
-LeapFrog Enterprises - for making such a cool and fun to hack device, being forthcoming with GPL requests, and generally being awesome. Sorry to see you guys get bought out by VTech, your products were infinitely better engineered and better manufactured. Hopefully through this projects, some of the old Leapster devices can live on past their original lifespan.
