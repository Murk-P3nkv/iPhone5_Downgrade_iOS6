# Custom iOS 6.1.4 IPSW for iPhone 5,2 (A6) Without SHSH

Disclaimer:
This guide is experimental and intended for educational purposes only. Modifying and flashing custom firmware may brick your device. Use at your own risk.

------------------------------------------------------------
Overview
------------------------------------------------------------
This guide explains the steps to create a custom iOS 6.1.4 IPSW that bypasses SHSH checks. The process involves:
1. Unpacking the original iOS 6.1.4 IPSW.
2. Extracting and patching the boot chain components (iBoot and iBEC).
3. Reassembling the custom IPSW.
4. Flashing the custom IPSW using pwned DFU.

------------------------------------------------------------
Requirements
------------------------------------------------------------
Device: iPhone 5,2 (A6)
OS: macOS or Linux (Ubuntu/Debian) — Windows is not recommended.
Tools:
 - ipwndfu (https://github.com/axi0mX/ipwndfu) for entering pwned DFU
 - libimobiledevice (https://libimobiledevice.org/), idevicerestore, and irecovery
 - img4tool (https://github.com/tihmstar/img4tool)
 - xpwntool (https://github.com/planetbeing/xpwntool)
 - iBootPatcher (https://github.com/sbingner/iBoot64Patcher) (or a similar patcher for iBoot)

Original IPSW: iPhone5,2_6.1.4_10B350_Restore.ipsw

------------------------------------------------------------
Installation of Required Tools
------------------------------------------------------------

-- On macOS --
1) Install Homebrew (if not already installed):
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
(Restart your terminal after installation.)

2) Install dependencies:
brew install libusb libimobiledevice idevicerestore irecovery python3

-- On Linux (Ubuntu/Debian) --
sudo apt update
sudo apt install libusb-1.0-0-dev libimobiledevice-utils python3 python3-pip unzip

-- Clone and Build Additional Tools --

1) ipwndfu:
git clone https://github.com/axi0mX/ipwndfu.git
cd ipwndfu
pip3 install -r requirements.txt
cd ..

2) img4tool:
git clone https://github.com/tihmstar/img4tool.git
cd img4tool
make && sudo make install
cd ..

3) xpwntool:
git clone https://github.com/planetbeing/xpwntool.git
cd xpwntool
make && sudo make install
cd ..

4) iBootPatcher:
git clone https://github.com/sbingner/iBoot64Patcher.git
cd iBoot64Patcher
make && sudo make install
cd ..

-- Verify that the tools are installed --
ipwndfu --checkm8
irecovery -v
idevicerestore -h
img4tool -v

------------------------------------------------------------
Step-by-Step Process
------------------------------------------------------------

Step 1: Unpack iOS 6.1.4 IPSW
---------------------------------
1) Download the original IPSW:
   wget http://updates.cdn-apple.com/iPhone5,2_6.1.4_10B350_Restore.ipsw

2) Unzip the IPSW:
   unzip iPhone5,2_6.1.4_10B350_Restore.ipsw -d ios6_extracted

3) Extract iBoot and iBEC:
   cp ios6_extracted/Firmware/dfu/iBEC.n42.RELEASE.dfu iBEC6.dfu
   cp ios6_extracted/Firmware/all_flash/all_flash.n42ap.production/iBoot.n42.RELEASE.img3 iBoot6.img3

Step 2: Patch iBoot and iBEC (Remove SHSH Checks)
---------------------------------------------------
1) Patch iBoot (remove digital signature verification):
   iBootPatcher iBoot6.img3 patched_iBoot6.img3

2) Patch iBEC (allow loading of unsigned firmware):
   Note: You must supply your initialization vector (IV) and key.
   xpwntool iBEC6.dfu patched_iBEC6.dfu -iv $(cat iv) -k $(cat key)

Step 3: Create the Custom iOS 6.1.4 IPSW
-----------------------------------------
1) Replace the original files with the patched versions:
   cp patched_iBoot6.img3 ios6_extracted/Firmware/all_flash/all_flash.n42ap.production/iBoot.n42.RELEASE.img3
   cp patched_iBEC6.dfu ios6_extracted/Firmware/dfu/iBEC.n42.RELEASE.dfu

2) Repack the IPSW:
   zip -r Custom_iOS6.ipsw ios6_extracted

Step 4: Flash the Custom IPSW
-------------------------------
1) Enter pwned DFU mode with ipwndfu:
   ./ipwndfu -p

2) Load a patched iBSS (to bypass SHSH checks):
   irecovery -f patched_iBSS.dfu
   irecovery -c go

3) Restore the custom IPSW:
   idevicerestore -w Custom_iOS6.ipsw

4) Reboot your device and enjoy your fully untethered iOS 6.1.4!

------------------------------------------------------------
Final Notes
------------------------------------------------------------
- Risks: This process is experimental and may result in a non-booting device if not done correctly.
- Support: There is limited documentation for these methods. Check community forums and GitHub repositories (such as checkm8-related projects) for additional support.
- Backup: Always back up your device before attempting any modifications.

Feel free to fork this repository or submit issues if you encounter any problems. Happy hacking!

