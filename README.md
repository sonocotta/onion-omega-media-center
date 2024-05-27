# Onion Omega Media Center

![Open Source Hardware](/images/open-source-hardware-logo.png)
![Open Source Software](/images/open-source-software-logo.png)
<a href="https://www.tindie.com/stores/sonocotta/?ref=offsite_badges&utm_source=sellers_andrey-malyshenko&utm_medium=badges&utm_campaign=badge_medium"><img src="https://d2ss6ovg47m0r5.cloudfront.net/badges/tindie-mediums.png" alt="I sell on Tindie" width="150" height="78"></a>
<br />
[![Dev Chat](https://img.shields.io/discord/1233306441469657140?logo=discord&label=discord&style=flat-square)](https://discord.gg/PtnaAaQMpS)

![DSC_0717](https://github.com/sonocotta/onion-omega-media-center/assets/5459747/e4f8ee2f-6547-494d-874e-77a47a807049)

Onion Omega Media Center is a series of Onion Omega-based media center devices. They share a similar look, and compared to my earlier designs, they have a great-looking aluminum case.

## Table of Contents

- [Onion Omega Media Center](#onion-omega-media-center)
  - [Motivation](#motivation)
  - [Onion Omega HiFi](#onion-omega-hifi)
  - [Loud and Louder Onion Omega](#loud-and-louder-onion-omega)
  - [HiFi Onion Features](#hifi-onion-features)
      - [Onion Omega itself](#onion-omega-itself)
      - [DAC section](#dac-section)
      - [Other Peripheral](#other-peripheral)
  - [Board Pinout](#board-pinout)
    - [Audio](#audio)
    - [Screen](#screen)
    - [Other](#other)
  - [Software](#software)
    - [Audio](#audio-1)
    - [Running pulse-server](#running-pulse-server)
    - [Screen, IR, RGB led.](#screen-ir-rgb-led)
  - [Hardware](#hardware)
    - [Useful links](#useful-links)
  - [To Do](#to-do)
  - [Where to buy](#where-to-buy)


## Motivation

I did few audio projects in the past, some using [ESP32](https://hackaday.io/project/173620-loud-esp), some using larger [Orange Pi](https://hackaday.io/project/191936-orange-pi-home-media-center) and [Raspberry Pi](https://hackaday.io/project/195272-raspberry-pi-media-center) devices. Each has its pros and cons, and with each iteration, I'm trying to focus on the details that were working best for me, while actually using them. 

What is special about the Onion Omega is how it is lightweight and powerful at the same time. Based on OpenWRT firmware it is impossible to break (same are used by domestic wifi-routers). It uses onboard SPI storage, so it does not wear out, like SD cards are. It boots in seconds and supports major high-level languages, as well as C/C++. It's up to you and your skill set.

Sure, compared to the ESP32 platform it is not as lightweight, but it is as close as it gets while giving you plenty of memory and space. But when it comes to rapid development, it is another world, compared to ESP32.

## Onion Omega HiFi

Onion Omega HiFi is a first-in-line product that uses the legendary PCM5100 series DAC with supreme audio quality. It exposes line-level output that you can plug into a stereo amplifier. Spend as much as you need on the external amp to deliver the sound you like (personally I prefer late 80's audio gear).

![DSC_0722](https://github.com/sonocotta/onion-omega-media-center/assets/5459747/210d950e-39bd-49e9-8bcf-66397146397f)

## Loud and Louder Onion Omega

This is a work in progress. Spoiler alert: Loud uses dual MAX98357 DACs with 3W per each of the two channels. Louder in its turn uses TI TAS5805M DAC with dual 22W output.

## HiFi Onion Features

#### Onion Omega itself

- MIPS MT7688AN CPU running at 580 Mhz 
- 64MB (OM-O2) or 128MB (OM-O2P) of RAM
- 16MB (OM-O2) or 32MB (OM-O2P) of Flash storage
- 2.4GHz WiFi
- 10M/100M Ethernet
- USB 2.0 Host
- MicroSD slot (OM-O2P only)

#### DAC section

- [PCM5100A](https://www.ti.com/product/PCM5100A) 32bit Stereo DAC
- 2.1 VRMS Line level output
- -100 dB typical noise level
- Triple [LP5907](https://www.ti.com/lit/ds/symlink/lp5907.pdf) 3.3 V Ultra-Low-Noise LDO
- 5V USB-C power adapter
- Mechanical dimensions (WxHxD): 88mm x 38mm x 100mm

#### Other Peripheral

- Onboard USB-Serial Bridge (UART0)
- Onboard WS2812B LED and LED-Strip header
- IR reader
- TFT SPI Screen header
- External Relay Driver 

## Board Pinout

### Audio

|       | I2S CLK | I2S DATA | I2S WS | 
|-------|---------|----------|--------|
| Onion Omega | 3      | 1       | 2     | 

### Screen

|       | SPI CLK  |SPI MOSI| SPI MISO | SPI CS   | DC   | RES  |  LED | 
|-------|----------|--------|----------|-----------|-----------|-----------|---------|
| Onion Omega |  7     |  8   |   9      |   6  | 18      |     19     | 15        | 


### Other

|       | RGB LED  | IR INPUT | RELAY OUTPUT | 
|-------|----------|--------|----------|
| Onion Omega |  17      |  16    |   11      |  


## Software

### Audio

The first thing to do is to follow the official [tutorial](https://onion.io/2bt-omega-i2s-audio/). The only pitfall is that you need to use a firmware build before b195. I used b193 to be more specific. A full list of old builds can be found [here](https://docs.onion.io/omega2-docs/manual-firmware-installation.html), and instructions on how to flash custom firmware are [here](https://docs.onion.io/omega2-docs/manual-firmware-installation.html). Short version is below

```bash
$ cd /tmp
# For Omega 2 Plus
$ wget http://repo.onion.io.s3.amazonaws.com/omega2/images/omega2p-v0.2.0-b193.bin
$ sysupgrade ./omega2p-v0.2.0-b193.bin
# or Omega 2 
$ wget http://repo.onion.io.s3.amazonaws.com/omega2/images/omega2-v0.2.0-b193.bin 
$ sysupgrade ./omega2-v0.2.0-b193.bin 
```

The issue with later versions is that at some point I2S [was disabled](https://community.onion.io/topic/3255/trying-to-get-the-i2s-audio-working/7) in favor of PWM working on the same pins. It can be restored, but requires a bit more effort.

As soon as you have proper firmware installed you need to issue the following commands

```bash
# install necessary packages
$ opkg update
$ opkg install alsa-utils mpg123
# enable I2S
$ omega2-ctrl gpiomux set i2s i2s

# Verify that souncard is present
$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: AudioI2S [Audio-I2S], device 0: ralink-i2s-HiFi HiFi-0 []
  Subdevices: 1/1
  Subdevice #0: subdevice #0

# Test audio with any audio file or stream, I used my local stream here

$ mpg123 http://192.168.1.42:8000/fg
High Performance MPEG 1.0/2.0/2.5 Audio Player for Layers 1, 2 and 3
	version 1.22.3; written and copyright by Michael Hipp and others
	free software (LGPL) without any warranty but with best wishes

Directory: http://192.168.1.42:8000/
Playing MPEG stream 1 of 1: fg ...
ICY-NAME: Fabio & Grooverider
ICY-URL: http://www.icecast.org/

MPEG 1.0 layer III, 128 kbit/s, 44100 Hz joint-stereo

ICY-META: StreamTitle='ÿþR - ÿþF';
```

Voila, we have a sound!

### Running pulse-server 

There is quite an old [write-up](https://hackaday.io/project/173621/log/188685-streaming-audio-over-network) that I did in the past. It should be still applicable and fully compatible with HiFi Omega

### Screen, IR, RGB led.

This is still a work in progress since it is not one-step instruction. Some links might help to get started

- [Ledchain for MT7688](https://github.com/plan44/plan44-feed/tree/master/p44-ledchain)
- [LIRC on Onion](https://community.onion.io/topic/951/lirc-or-arduino-serial/2) and [another one](https://community.onion.io/topic/371/has-anyone-gotten-lirc-cross-compiled)

## Hardware

| Front | Back | PCB |
|---|---|---|
| Image coming soon | ![DSC_0717_small JPG-mh](https://github.com/sonocotta/onion-omega-media-center/assets/5459747/43d97a98-52ca-4b67-bd46-684fa9093b58) | ![DSC_0714_small JPG-mh](https://github.com/sonocotta/onion-omega-media-center/assets/5459747/9597f89d-0ac6-4a34-801f-b61629dded29)

Please visit [hardware](/hardware/) section for board schematics and PCB designs. Note that PCBs are shared as multi-layer PDFs as well as Gerber archives.

### Useful links

- [Using antennas with the Omega](https://onion.io/2bt-u-fl-antennas-with-the-omega/)
- [turning off](https://community.onion.io/topic/2000/turning-off-wifi) or [disabling](https://community.onion.io/topic/1502/disable-wifi-hardware) WiFi

## To Do

Many things I'd try to do if I had a few more extra hours in the day.

- Cross-compile [librespot](https://github.com/librespot-org/librespot)
- Cross-compile [snapcast](https://github.com/badaix/snapcast)
- Cross-compile Airplay
- Cross-compile LMS client
- Connect to Home Assistant using any of the above
- Configure the TFT Screen and show the VU-meter on it
- Configure LED Strip and IR reader

If you have more time and skills than I do and are ready to check some of those out, please [contact me](mailto:andriy@sonocotta.com) directly. I will try sponsoring the board for you, as long as I have stock available.

## Where to buy

You may support our work by ordering this product at [Tindie](https://www.tindie.com/products/sonocotta/onion-omega-media-center/)
