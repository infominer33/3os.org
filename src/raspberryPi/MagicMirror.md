title: Raspberry Pi - Magic Mirror Installation Guide
description: LRaspberry Pi - Magic Mirror Installation how to, guides, examples, and simple usage

# Magic Mirror

<img src="https://w.3os.org/3os/magicMirror/IMG_2134.jpg" width=1000>

## Magic Mirror Build Pictures

23" Samsung screen power resoldering:

<img src="https://w.3os.org/3os/magicMirror/IMG_1719.jpg" width=800>

Wooden frame initial fitting test on a glass with duel mirror film applied:

<img src="https://w.3os.org/3os/magicMirror/IMG_1717.jpg" width=800>

Testing the screen installation (frame removed) with power cords:

<img src="https://w.3os.org/3os/magicMirror/IMG_1722.jpg" width=800>

Testing black&white picture from a laptop after frame assembly:

<img src="https://w.3os.org/3os/magicMirror/IMG_1732.jpg" width=800>

Power, Lan, Usb external ports cutouts:

<img src="https://w.3os.org/3os/magicMirror/IMG_1766.jpg" width=800>

Fitted extended ports with wood filler:

<img src="https://w.3os.org/3os/magicMirror/IMG_1767.jpg" width=800>

Extended ports:

<img src="https://w.3os.org/3os/magicMirror/IMG_1768.jpg" width=800>

Assembly With screen, Raspberry Pi, cable routing, black material which do not pass light where there is no screen:

<img src="https://w.3os.org/3os/magicMirror/IMG_1771.jpg" width=800>

Adding some color for the frame:

<img src="https://w.3os.org/3os/magicMirror/IMG_1978.jpg" width=800>

Testing everything is working as it should be:

<img src="https://w.3os.org/3os/magicMirror/IMG_1984.jpg" width=800>

Full assembly behind the mirror:

<img src="https://w.3os.org/3os/magicMirror/IMG_1985.jpg" width=800>

Final Product:

<img src="https://w.3os.org/3os/magicMirror/IMG_2134.jpg" width=800>

## Configuration Setup

### Change Display Rotation

```bash
sudo nano /boot/config.txt
```

Add one of those according to your setup to the config file:

| Code                   | Description     |
| ---------------------- | --------------- |
| display_rotate=0       | Normal          |
| display_rotate=1       | 90 degrees      |
| display_rotate=2       | 180 degrees     |
| display_rotate=3       | 270 degrees     |
| display_rotate=0x10000 | horizontal flip |
| display_rotate=0x20000 | vertical flip   |

`NOTE: You can rotate both the image and touch interface 180º by entering lcd_rotate=2 instead`

### Disabling the Screensaver

Change to OPEN GL Driver

```bash
sudo nano /boot/config.txt
```

add this:

```bash
dtoverlay=vc4-fkms-v3d
```

----
(Please note, you will need the x11-xserver-utils package installed.)

edit ~/.config/lxsession/LXDE-pi/autostart:

```bash
sudo nano ~/.config/lxsession/LXDE-pi/autostart
```

Add the following lines:

```bash
@xset s noblank
@xset s off
@xset -dpms
```

Edit /etc/lightdm/lightdm.conf:

```bash
sudo nano /etc/lightdm/lightdm.conf
```

Add the following line below [SeatDefaults]

```bash
xserver-command=X -s 0 -dpms
```

### OS UI Finishes

Make the Background Black:

`Right click the Desktop` -> `Desktop Preferences` and Change:
`Layout -> no image`
`Colour -> #000000`

Hit ok.

`Right click on the top panel` -> `Panel Preferences` -> `Appearance`

Select `Solid Color (With Opacity)` make sure `Opacity at 0`

### Disable WiFi Power Save

Edit /etc/modprobe.d/8192cu.conf

```bash
sudo nano /etc/modprobe.d/8192cu.conf
```

Add the following lines

```bash
# Disable power saving
options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1
```

For Raspberry Pi 3 (Jesse and below)
Edit /etc/network/interfaces

```bash
sudo nano /etc/network/interfaces
```

Add the following line under the wlan0 section

```bash
wireless-power off
```

Reboot your PI

```bash
sudo reboot
```

### Disable Cursor on Startup

```bash
sudo apt-get install unclutter

```

### Installation

first install node.js and npm

```bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

and then run:

```bash
sudo npm install -g npm@latest
```

If you need to remove node and npm run this:

```bash
sudo apt-get remove nodejs nodejs-legacy nodered
```

Installation:

```bash
bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)"
```

`say no to PM2 auto start - will be install manually`

To Start from SSH:

```bash
cd ~/MagicMirror && DISPLAY=:0 npm start
```

### pm2 auto start installation

```bash
sudo npm install -g pm2
cd ~
nano mm.sh
```

add this to mm.sh and save:

```bash
#!/bin/sh

cd ~/MagicMirror
DISPLAY=:0 npm start
```

```bash
chmod +x mm.sh
pm2 start mm.sh
pm2 save
pm2 startup
```

pm2 commands:

```bash
pm2 restart mm
pm2 stop mm
pm2 start mm
pm2 log
pm2 show mm
```

## Logrotate Installation

This will Retain for 14 days compress the logs.

```bash
pm2 install pm2-logrotate
pm2 set pm2-logrotate:compress true
pm2 set pm2-logrotate:retain 14
pm2 set pm2-logrotate:max_size 10M
```
