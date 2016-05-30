---
layout: post
title:  "Build a Raspberry Pi-Powered DIY Amazon Echo"
date:   2016-05-23 00:00:00 +0900
categories: jekyll update
type: Fun
excerpt_separator: <!--more-->
---
<!--more-->

Overview
---

Things You Need
---
- Raspberry Pi
- speaker with 3.5mm jack
- USB microphone [(SANWA SUPPLY MM-MCUSB16 USBマイクロホン)][R1]
- A button connected between GPIO 18 and GND
- A developer account at [http://developer.amazon.com][R2]
- (Optionally) A Dual colour LED (or 2 signle LEDs) Connected to GPIO 24 & 25

Steps
---
#1. Prepare your Raspberry Pi, with a fresh install of Raspbian.

#2. git clone https://github.com/sammachin/AlexaPi

#3. Go to Amazon Developer Console. At the top of menu, choose `APPS & SERVICES` -> `Alexa`

#4. Select `Alexa Voice Service`

#5. Select `Register a Product Type` -> `Device`, then input

```
Device Type ID: RaspberryEcho
Display Name: RaspberryEcho
```
#6. Generate a security profile. Choose `Create a new profile`, then input

```
Security Profile Name: Raspberry Echo
Security Profile Description: Raspberry Echo
```
Edit `Web Settings`, add another as:

```
Allowed Origins: http://localhost:5000
Allowed Origins: http://192.168.1.116:5000 (Your Device IP)
```
This only allow above IP to interface with Alexa.

```
Allowed Return URLs: http://localhost:5000/code
Allowed Return URLs: http://192.168.1.116:5000/code
```

#7. Device Details.

```
Category: Other
Description: raspberry pi echo
What is your expected timeline for commercialization? Longer than 4 months
How many devices are you planning to commercialize?: 1
```
Amazon Music, No.

After submitting, you will see security profile below:
![Security Profile](http://i.imgur.com/FwMyYss.png)

#8. sudo ./setup.sh

```
Enter your ProductID: RaspberryEcho
Enter your Security Profile Description: Raspberry Echo
Enter your Security Profile ID:
Enter your Security Client ID:
Enter your Security Client Secret:
```

#9. Open your browser, type `http://192.168.1.116:5000`, hit `Continue`, it then generates refresh token key which allows you to talk to Alexa Voice Service.

You may encounter a 'SyntaxError', just `sudo nano creds.py`, edit the last line to

```
refresh_token = "Atzr|IQ...."
```
save it, test it by calling `python main.py` before you reboot.


GPIO
---
B+ Layout:

![Layout1](http://i.imgur.com/Qy9Mmxx.png)

Pin like this:

![Layout2](http://i.imgur.com/1BwUiMn.jpg)


Issues I Met
---
Somehow I just can not hear "Hello" after running `python main.py`, and I modified the one-line code below:

``` python
<!-- os.system('mpg123 -q {}1sec.mp3 {}hello.mp3'.format(path, path)) -->
os.system('omxplayer {}hello.mp3'.format(path))
```

Instead of using mpg123 by omxplayer.

I can hear "Hello" now, but Alexa seems not recording.

(ongoing...)


Test Your USB Microphone with Recording/Playing
---
To record:

```
$ arecord -D plughw:1,0 -f cd test.wav
```

To playback:

```
$ aplay test.wav
```


Microphone Setup with USB Sound Card
---
If you prepare a USB sound card for voice recording, you probably need to set up microphone with USB sound card.

#1. Plug in the dongle and check the version of your sound card with lsusb

```
$ lsusb
Bus 001 Device 004: ID 0d8c:0134 C-Media Electronics, Inc.
```

#2. To enable USB audio output, load the sound driver:

```
$ sudo modprobe snd_bcm2835
```

#3. Showing that the microphone is visible and its usb extension.

```
$ cat /proc/asound/cards
0 [ALSA           ]: bcm2835 - bcm2835 ALSA
                      bcm2835 ALSA
1 [Device         ]: USB-Audio - USB PnP Audio Device
```                      C-Media Electronics Inc. USB PnP Audio Device at usb-3f980000.usb-1.4, full spe

#4. To prevent the internal sound card to appear at the top comment out line, and change index from -2 to 1:

```
$ sudo nano /etc/modprobe.d/alsa-base.conf

options snd-usb-audio index=1
```

#5. Enable USB audio output by default

```
$ sudo nano /etc/asound.conf

pcm.!default {

type plug

slave {

pcm "hw:1,0"

}

}

ctl.!default {

type hw

card 1

}
```

#6. Reboot now.

```
$ sudo reboot
```

#7. To record:

```
$ arecord -D plughw:1,0 -f cd test.wav
```

#8. To playback:

```
$ aplay test.wav
```

#9. You may want to adjust some volumes:

```
$ alsamixer
```

#10. To save your settings:

```
$ sudo alsactl store
```

Reference
---
[Build a Raspberry Pi-Powered DIY Amazon Echo][R3]<br />
[AlexaPi][R4]<br />
[Crunch Report | Try Amazon Alexa In A Web Browser][R5]<br />
[Amazon puts Alexa in the browser with Echosim.io][R6]<br />
[RASPBERRY PI MICROPHONE SETUP WITH USB SOUND CARD][R7]

[R1]: http://www.amazon.co.jp/%E3%82%B5%E3%83%B3%E3%83%AF%E3%82%B5%E3%83%97%E3%83%A9%E3%82%A4-MM-MCUSB16-SANWA-SUPPLY-USB%E3%83%9E%E3%82%A4%E3%82%AF%E3%83%AD%E3%83%9B%E3%83%B3/dp/B0027WPY82/ref=sr_1_6?ie=UTF8&qid=1459303168&sr=8-6&keywords=%E3%83%9E%E3%82%A4%E3%82%AF+USB
[R2]: http://developer.amazon.com
[R3]: http://lifehacker.com/build-a-raspberry-pi-powered-diy-amazon-echo-1762678229
[R4]: https://github.com/sammachin/AlexaPi
[R5]: http://techcrunch.com/2016/05/27/crunch-report-try-amazon-alexa-in-a-web-browser/
[R6]: http://techcrunch.com/2016/05/27/amazon-puts-alexa-in-the-browser-with-echoism-io/
[R7]: http://www.linuxcircle.com/2013/05/08/raspberry-pi-microphone-setup-with-usb-sound-card/