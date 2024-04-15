# Onion Omega Media Center

![Open Source Hardware](/images/open-source-hardware-logo.png)
![Open Source Software](/images/open-source-software-logo.png)

![DSC_0733](https://github.com/sonocotta/raspberry-media-center/assets/5459747/2a6729d0-b9ed-4b83-95d5-de66f2ad8bb1)

Onion Omega Media Center is a series of Onion Omega based media center devices. They share a similar look, and compared to my earlier designs, they have a great-looking aluminum case.

## Motivation

I did few audio projects in the past, some using [ESP32](https://hackaday.io/project/173620-loud-esp), some using larger [Orange Pi](https://hackaday.io/project/191936-orange-pi-home-media-center) and [Raspberry Pi](https://hackaday.io/project/195272-raspberry-pi-media-center) devices. Each has its pros and cons, and each iteration I'm trying to focus on the details that were working best for me, while actually using them. 

What is special about the Onion Omega is how it is lightweight and powerfull at the same time. Based on OpenWRT firmware it is impossible to break (same are used bu domestic wifi-routers). It uses onboard SPI storage, so it is not wear out, like SD-cards are. It boots in seconds and support major high level languages, as well as C/C++. It's up to you and your skillset.

Sure, compared to the ESP32 platform it is not as lightweight, but it is as close as it gets, while giving you plenty of memory and space. But when it comes to rapid development, it os other world, compared to ESP32.

## Onion Omega HiFi

Onion Omega HiFi is a first-in-line product that uses the legendary PCM5100 series DAC with supreme audio quality. It exposes line-level output that you can plug into a stereo amplifier. Spend as much as you need on the external amp to deliver the sound you like (personally I prefer late 80's audio gear).

![DSC_0727](https://github.com/sonocotta/raspberry-media-center/assets/5459747/029e5175-1ee1-4876-8ba3-91d49730c29f)

## Loud and Louder Onion Omega

This is a work in progress. Spoiler alert: Loud uses dual MAX98357 DACs with 3W per each of two channels. Louder in its turn uses TI TAS5805M DAC with dual 22W output.

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
| Onion Omega Zero | 3      | 1       | 2     | 

### Screen

|       | SPI CLK  |SPI MOSI| SPI MISO | SPI CS   | DC   | RES  |  LED | 
|-------|----------|--------|----------|-----------|-----------|-----------|---------|
| Onion Omega Zero |  7     |  8   |   9      |   6  | 18      |     19     | 15        | 


### Other

|       | RGB LED  | IR INPUT | RELAY OUTPUT | 
|-------|----------|--------|----------|
| Onion Omega Zero |  17      |  16    |   11      |  


## Software samples

todo


## Hardware

| Front | Back | PCB |
|---|---|---|
| ![DSC_0730 (copy 1) JPG-mh (1)](https://github.com/sonocotta/raspberry-media-center/assets/5459747/c281dab2-9842-4760-be31-8ad52d836f4d) | ![DSC_0733 (copy 1) JPG-mh](https://github.com/sonocotta/raspberry-media-center/assets/5459747/ba11555c-9e0c-47eb-b77e-4ac4b4ca1a99) | ![DSC_0739 (copy 1) JPG-mh](https://github.com/sonocotta/raspberry-media-center/assets/5459747/b438fd68-108c-42b6-b3b1-4c2507fbd568)

Please visit [hardware](/hardware/) section for board schematics and PCB designs. Note that PCBs are shared as multi-layer PDFs as well as Gerber archives.

## Where to buy

You may support our work by ordering this product at Tindie (coming soon)
