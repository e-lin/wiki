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
- A developer account at [http://developer.amazon.com][http://developer.amazon.com]

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



Reference
---
[Build a Raspberry Pi-Powered DIY Amazon Echo][R2]<br />
[AlexaPi][R3]

[R1]: http://www.amazon.co.jp/%E3%82%B5%E3%83%B3%E3%83%AF%E3%82%B5%E3%83%97%E3%83%A9%E3%82%A4-MM-MCUSB16-SANWA-SUPPLY-USB%E3%83%9E%E3%82%A4%E3%82%AF%E3%83%AD%E3%83%9B%E3%83%B3/dp/B0027WPY82/ref=sr_1_6?ie=UTF8&qid=1459303168&sr=8-6&keywords=%E3%83%9E%E3%82%A4%E3%82%AF+USB
[R2]: http://lifehacker.com/build-a-raspberry-pi-powered-diy-amazon-echo-1762678229
[R3]: https://github.com/sammachin/AlexaPi