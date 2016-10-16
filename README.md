# RASPBIAN JESSIE KIOSK

This documemt describes the process of turning Raspberry Pi running the original Raspbian OS into a kiosk machine, using Chromium browser, which has been recently enabled without the need of any workarounds since the Chromium browser was included in the latest version of Raspbian (2016-09-23).

- [Setup](#setup)
  - [Initial pi configuration](#initial-pi-configuration)
  - [Mouse cursor](#hiding-mouse-cursor)
  - [Kiosk mode](#kiosk-mode)
- [Additional tips](#additional-tips)
- [Contribution](#contribution)

## Tested on:
- **Devices**
  - `Pi 3`


- **Raspbian**
  - [`2016-09-23-raspbian-jessie`](https://downloads.raspberrypi.org/raspbian_latest)


## Setup
Download the latest [raspbian image](https://www.raspberrypi.org/downloads/raspbian/) and install the image on you SD card using the [standard procedure](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

#### Initial pi configuration
First, make sure everything is up to date

```shell
sudo apt-get update && sudo apt-get install upgrade -y
```

Set your wireless network preferences using [this guide](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md).

Run the Raspberry Pi configuration tool

```shell
sudo raspi-config
```
In the configuration tool make sure to:
- Expand the file system
- Disable overscan from `Advanced` menu. This will help ensure the display fills the entire screen.
- Make sure the default option to boot is set to GUI autologin.

Restart.
## Hiding mouse cursor
##### Display only
If you don't plan to use a touch screen, the solution is to use `unclutter`, which is a tool, that hides the cursor after some idle time.

```shell
sudo apt-get install unclutter
```

##### Touch screen
If you use touch screen to interact with you device, you probably don't want to see the mouse cursor appearing under your finger every time you touch the screen, so the answer here is to disable the mouse pointer alltogether. Just be sure your display is properly configured. (The 7" Raspberry Pi Display works pretty much out of the box)

In this case, instead of using unclutter, we simply edit the `lightdm.conf` file.
```shell
sudo nano /etc/lightdm/lightdm.conf
```

Uncomment the `xserver-command` line under `[SeatDefaults]` (below the documentation) and add `-nocursor` parameter.

```
xserver-command=X -nocursor
```

## Kiosk mode
To configure the pi to become a kiosk machine, all you need to do is edit the `autostart` file in `~/.config/lxsession/LXDE-pi/`

```shell
sudo nano ~/.config/lxsession/LXDE-pi/autostart
```

Disable the screensaver by commenting out this line:
```
# @screensaver -no-splash
```
Add these `xset` options to disable some of the power saving settings:

```
@xset s off
@xset s noblank
@xset -dpms
```

If you've decided to use `unclutter`, you can configure it by using commands described [here](http://manpages.ubuntu.com/manpages/wily/man1/unclutter.1.html), for example adding this line will set the mouse pointer to disappear after 3 seconds of inactivity:

```
unclutter -idle 3
```


Add this line to start the Chromium browser in kiosk mode after boot:
```
@chromium-browser --noerrdialogs --kiosk --incognito https://google.com
```
The `--noerrdialogs` parameter will make sure that no error messages will pop up after restart if something causes Chromium to end unexpetedly.
Save, exit and restart your pi.

## Additional tips
tba

## Contribution
If you have useful tips, trick or scripts, feel free to add them to this repo.

