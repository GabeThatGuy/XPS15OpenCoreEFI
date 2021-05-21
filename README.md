# XPS 15 OpenCore EFI
An EFI to get (mostly) everything working on macOS Big Sur & OpenCore 0.6.2.
I'm going to keep this EFI up to date with new kexts, changes and improvements as new features and support come out, and for when (if) I manage to get the sleep functionality and SD Card reader working.

## Disclaimer
I am not responsible for any negative effects that are incurred onto your system after following this guide. Any steps you take are at your own risk. (That being said, reach out to me on the issues page and I'll do my best to help you out)

This guide is designed for those who have access to macOS. If you don't have a mac handy, you'll need to do a bunch of extra work and research for yourself.

## Compatible Systems
- Dell XPS 15 9500
    - i7-10750H
    - i7-10875H
    - i5 Models not tested, but should work just fine.
- Dell XPS 17 9700 not tested, this EFI should work with minor tweaking, but I'll release a new repo for that EFI.
- XPS 15 2-in-1 MAY be supported, although 2-in-1 functionality will be mainly useless unless you want to manually change the screen rotation in system preferences every time. 

## Things that don't work:
- Sleep functionality
- SD Card Reader
- Wi-Fi 6 support is in beta, but wifi 5 works just fine.
- Dedicated GPU: If your XPS is equipped with an NVIDIA Dedicated GPU, you won't be able to use it on macOS.
   Apple kind of hates NVIDIA and high sierra was the last macOS that had NVIDIA GPU support, and it was terribly implemented back then. For this reason, OpenCore disables the dedicated GPU on a hardware level at startup.
- Apple watch unlock and AirDrop: I'm currently working on getting these fixed, and I'll release the new EFI once I manage to get it working. Keep an eye on the releases page. 

- Battery Life: So, the battery works, battery health too. But there's a major downside. Your battery is only going to last about 5 hours at most, since macOS lacks compatibility with Dell's bios to properly control power management. Even with 0% cpu usage, the system still draws about 18 watts from the battery. If your XPS is a little older like mine is, you'll likely only get between 3 and 4 hours.
- Fingerprint sensor: As the XPS 15 lacks Apple's T2 chip, touch ID functionality doesn't work on macOS.
- Fan Control: Your fans will work in macOS, and keep the system cool. But, they keep the system too cool. The target temperature that's set from the factory is 45c, and so the fans will be at whatever value they need to be at to keep the system at 45c. I've tried MacsFanControl and iStatMenus to try and change the fan curve with no luck.

## Setting up the EFI
Okay, now that the scary legal stuff is out of the way, let's get into installing and using this EFI.

If you're new to hackintoshing, don't you worry. I'll keep this simple. Best thing you can do for yourself is have a look at Dortania's OpenCore guides. They're rediculously detailed and outline every step you need to get a hackintosh up and running.

The gist of what you need to do is:
1) Make a macOS Big Sur install disk. If you've got another mac you can use, just head to the app store, download the Big Sur installer, and run this command in terminal: sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

I suggest renaming your USB (which needs to be 16GB or larger) to MyVolume, so that you dont have to edit the command. Just copy, paste, and hit enter.
If you dont have a mac that you can use, I suggest heading to Google and searching for some kind of disk image you can flash onto your USB. Be careful with some of these hackintosh websites that claim to have a download link. Most often thell try and get you to sign up for an account and then once you do sign up there's no file to download.

2) Mount the EFI partition of your USB installer.
Again; if you've made your installer on mac, you can head to the github repo for EFIMounter, although I find its easier to use disk utility's terminal commands since most EFI partitions are read only without SUDO permissions and EFIMounter usually just errors out.
  a) If you want to use the disk utility commands, here's what youve gotta do. First: Open up disk utility and click on your new installer USB. Find out what your disk identifier is (should be something like disk#, where the # represents any number). Then, the EFI partition should be disk#s0 if it exists, or disk#s1.
  b) Once you've got your disk identifier, type in this command into the terminal: sudo diskutil mount disk#s#
  c) Once that's done, you can fire up Finder and you should see it in your sidebar.
  
## Setting up the config.plist
For this section, head to my repo called hackintosh-tools to grab ProperTree and GENSMBIOS. (Note, these will only work on macOS systems, so if you're on Windows I recommend scouring the internet for Windows versions.
To start, copy the files from this repo into that EFI partition. You should replace the entire EFI folder with the one you download from here, since merging the two will almost certainly cause either a kernel panic or simply a failed boot. 

Next, open up GENSMBIOS and hit the number 1 to download and install MacSerial. Once that's done, youll be back at the main page of GENSMBIOS and you'll be ready to start creating your mac serial number and uuid. 

  First, hit the number 2, and point the terminal to your config.plist. You'll find this in the following location: /Volumes/EFI/EFI/OC/config.plist.
  Once that's done, hit number 3 and it'll ask you for a Mac identifier. You'll want to type MacBookPro16,4 1. That extra 1 tells GENSMBIOS we only want one serial number since we're only making one hackintosh. 
  Once it's done, press enter and it'll add the relevant information into your config.plist and you'll be set to boot up from the USB on your XPS.
 As soon as your EFI folder is all set up, go ahead and grab your USBC to USBA adapter and plug your new USB in to the XPS.

## Setting up your BIOS

## Booting the System
Okay, so this is where the hard part begins. The biggest problem with hackintoshing is the fact that it almost never works first try. I can't predict if you'll have any issues, since this exact EFI boots my XPS just fine with no issues whatsoever. If you do run into issues, though, I'm always available over on the Issues page to help you out.

To boot off your usb: hit the power button on your XPS and then immediately start pressing f12(end) key over and over until you get to the boot menu. Should be a white screen with a couple of boot options listed on the left side. Pick the one that says UEFI: OpenCore (or the name of your USB drive).

If all goes to plan, you should be greeted with a very macOS-looking style device picker. You should see your Windows installation, and your macOS Big sur installer. Using the arrow keys, your mouse, or the touchscreen, click on the macOS installer and give that a second to start booting. You're going to see A LOT of command-line looking stuff. Not to worry, this is what happens behind the scenes when a mac is booting. Our EFI has Verbose mode enabled, which basically just means that macOS is showing you what's happening instead of the Apple logo with a progress bar.

The first boot of the installer does take quite a while, so if it hangs for a minute or two, not to worry.
Once you're booted up to the installer, go ahead and scroll to the next section to prepare your internal SSD for a macOS Installation.

## Installing macOS
So chances are you've got one of these situations
1) You're running Windows on your internal SSD, and don't have a second one you can install.
2) You're running Windows on your internal SSD, and you do have a second one you can install.
3) You're running some other OS on your internal SSD, and you don't have a second one you can install.
4) You're running some other OS on your internal SSD and you do have a second one you can install. 
5) You're not running any OS on your internal SSD.


I Suggest you scroll down to the section that applies to you, or if you're bored you can read the whole thing.

### Situation 1: One SSD, running Windows
Alright, so you want to dual boot macOS and Windows together. Here's what you gotta do:
First of all, boot into Windows normally and fire up Disk Management. Easiest way to do this is hit the windows key and type "disk partitions" and hit enter.

Once you're there, find your SSD and right click that puppy. Hit shrink volume and wait for it to find out how much space you've got left. Once it finishes that, you can enter in the amount of space (in megabytes) that you want to allocate to macOS. You'll want to put this value in the box where it says "Enter the amount of space to shrink in MB". Once that's done, hit shrink give it a second to do its thing.
When it's done, right click your new segment on your disk (should be black and labelled "Unallocated") and click new simple volume. Click next over and over until you get to the "Format Partition" page. You'll want to change the "File System" option to ExFAT (or if ExFAT isn't an option, FAT32 will work just fine) and then hit next. The reason you can't use NTFS is because the macOS installer usually doesn't detect NTFS (Windows format) partitions. Once your new partition is all done and made, just reboot and boot from your USB like you did before in the booting the system section. 
Once you're into the installer, head over to disk utility so we can prepare our macOS Partition.

First thing you need to do is switch to disks mode instead of volumes. To do this, you'll need to click "View" in the menu bar and then click "Show all devices".
If you changed the name of your partition in Windows, you should see that same name in disk utility. Otherwise, you should see your partition labelled as "New Volume".
You'll want to click that partition and hit Erase in the top left. Don't worry, we're only erasing a partition that's already blank, so you won't be losing any data.
Change the format to APFS, not encrypted, and then give it a name. This will be the name that shows up in the Finder and at the boot disk selector, so keep that in mind. I named mine "Macintosh SSD" on my desktop and "macOS Big Sur" to go along with "Windows" on my XPS 15. 
Then, click erase and wait for it to finish.
When it's done, quit out of disk utility and click on "Install a new copy of macOS"
Follow the instructions on screen and pick your new partition to install onto. 
Let macOS install like it normally would and make sure to keep beside your laptop and pay attention when it reboots since you've gotta select the operating system you want each time. If you leave it, it's likely to automatically boot into windows and your macOS installation won't continue until you boot back into macOS. 

And that's it. Once your installation is complete set up your new MacBook as you like. Connect to your Wi-Fi and sign in to your Apple ID if you like as well. Then, scroll down to the next step where we make it so you can boot your XPS without the installer USB.

