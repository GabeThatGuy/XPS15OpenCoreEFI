# XPS15OpenCoreEFI
An EFI to get (mostly) everything working on macOS Big Sur & OpenCore 0.6.2.

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


## Booting the System
Okay, so this is where the hard part begins. The biggest problem with hackintoshing is the fact that it almost never works first try. 

