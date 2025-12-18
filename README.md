# BroadcomWiFi-BCM4364-
Installing Broadcom BCM4364 drivers on iMac 19,1 

Here is how I managed to get the Wi-Fi drivers working on my iMac 19,1 running Linux Mint. It took a bit of trial and error with the firmware script and a manual fix for the specific board file, but here is the process that finally worked for me.
# Step 1: Install Dependencies
First, I needed to make sure I had the necessary tools to handle Apple disk images. I realized the script would fail if dmg2img wasn't installed, so I grabbed that first.

sudo apt update

sudo apt install dmg2img


# Step 2: Download the Firmware Script
Next, I downloaded the firmware extraction script from the T2Linux wiki and made it executable so I could run it.
curl -O https://wiki.t2linux.org/tools/firmware.sh

chmod +x firmware.sh


# Step 3: Run the Script and Download macOS Monterey
I ran the script with sudo. When prompted, I selected Option 3 (Download a macOS Recovery Image).
I initially tried to download Sonoma, but it gave me network errors. I switched to Monterey (Option 5), and that downloaded and extracted without any issues.

sudo ./firmware.sh

Selected Option 3 (Download Recovery Image)

Selected Option 5 (Monterey)


# Step 4: The Crucial Manual Fix (Symlink)
Even after the script finished, the Wi-Fi didn't come up immediately. I checked the kernel logs and saw it was failing to load a file ending in ,nihau.txt. The script had extracted the files, but the specific text file for my iMac's board (Nihau) wasn't named exactly what the driver expected.
I went into the firmware directory and created a symbolic link to map the correct calibration file:

cd /lib/firmware/brcm/

sudo ln -sf brcmfmac4364b2-pcie.apple,nihau-HRPN-m.txt brcmfmac4364b2-pcie.apple,nihau.txt


# Step 5: Reload the Drivers
Once the link was created, I just had to unload and reload the Broadcom driver to force it to read the new file.

sudo modprobe -r brcmfmac

sudo modprobe brcmfmac

After doing that, I checked my  Wi-Fi status using hciconfig -a, and everything was up and running!
# I pasted my terminal code into A.I and made it generate the step by step based on all that I did to get it to work. I am just glad my Sound and WiFi is finally running. Hopefully this helps someone
