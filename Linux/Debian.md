---
layout: page
title: Installing Debian and Setting up Linux
---
June 2026

---
# 1. Installing the OS
We are going to **flash Debian to a usb stick**. To do this, download [Rufus](https://rufus.ie/en/) on another computer and get whatever version of Debian you need from [the official site](https://www.debian.org/distrib/netinst). When flashing the dongle, you probably want to **use UEFI rather than legacy** (if the computer offers). **Make sure to flash DD**! Not ISO...

![Flashing a usb via Rufus](Linux/figures/Rufus-flash-debian.png)

Now, plug the dongle into the actual computer you want to install Debian on and boot from the stick in the BIOS. Then, follow the instructions till you have it installed.

After doing the installation, get sudo privileges:
```bash
Su - 

adduser your_username sudo
```
# 2. Internet Connection
Next, get wifi up and running. You likely need to **plug in ethernet** to start. If the computer doesn't have a wifi card, **use a usb stick**. Verify that you are connected by:
```bash
ping -c 4 8.8.8.8
```
**Make sure APT is working**, this is how we are going to install everything (including networking tools!)
```bash
sudo nano /etc/apt/sources.list
```
If that is empty or "cdrom", **replace everything** with
```bash
deb http://deb.debian.org/debian trixie main contrib non-free-firmware

deb http://deb.debian.org/debian trixie-updates main contrib non-free-firmware

deb http://security.debian.org/debian-security trixie-security main contrib non-free-firmware
```
Now **update the package lists**:
```bash
sudo apt update
sudo apt upgrade -y
```
install networking tools:
```bash
sudo apt install network-manager wpasupplicant isc-dhcp-client
```
Enable **NetworkManager**:
```bash
sudo systemctl enable --now NetworkManager
```
Find and **connect to Wifi**:
```bash
nmcli dev wifi list
nmcli dev wifi connect "YourSSID" password "YourPassword"
```
# 3. Get some basic packages and set up SSH
Install the basics:
```bash
sudo apt install git tmux openssh-server avahi-daemon python3-pip python3-venv
```
Verify SSH works
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```
Verify Avahi works:
```bash
systemctl status avahi-daemon
```
Set up ssh with a host name:
```bash
sudo hostname set-hostname name
```
run ssh:
```bash
ssh username@hostname.local
```
# 3. Set up a Python Environment
Create the VE:
```bash
mkdir -p ~/venvs

python3 -m venv ~/venvs/main
```
activate it:
```bash
source ~/venvs/main/bin/activate
```
upgrade pip:
```bash
pip install --upgrade pip
```
install scientific libraries:
```bash
pip install numpy scipy matplotlib pandas jupyterlab notebook
```
Set up Jupyter notebook (remotely!):
```bash
source ~/venvs/main/bin/activate

jupyter lab --ip=0.0.0.0 --no-browser
```
Now you can ssh from another computer and type in
```bash
http://hostname:portnumber/lab?token=...
```
# 4. Set up SDR tools
Get the basics:
```bash
sudo apt install rtl-sdr gqrx-sdr rtl-433 direwolf gpredict fldigi, wsjtx, cqrlog, satdump, qsstv, chirp, libhamlib-utils, libhamlib-dev, gnuradio
```
# 5. Get a desktop environment (XFCE in this case!)
We're going to get the **full XFCE desktop** since it's lightweight and easy to use. Run this to get XFCE desktop, panel, file manager, settings tools, and some extra plugins/utilities:
```bash
sudo apt install xfce4 xfce4-goodies -y
```
Now we're going to set up a login screen, lets get **lightdm**:
```bash
sudo apt install lightdm -y
```
Enable it:
```bash
sudo systemctl enable lightdm
sudo systemctl start lightdm
```
