---
title: "weekend_project // ESP32 Deep Sleep Timerbox"
date: 2021-04-14T08:58:38+01:00
draft: true
---


Last weekend I started a little project for my mum. She takes medicines daily and, like any other human being, often forgets that she has taken it a second later. So, I thought of a pillbox that shows how long it has been since it was last closed.


I obviously did some research first for a ready-to-buy solution. [This](https://www.timercap.com) is the only product I could find. Problem is, it is produced and sold in the US, and would cost 20$ + shipping and tax. Not to mention the delivery times. 

## Idea

So, I decided to make my own version using a TTGO T-display ESP32.

PROS
- Cost (7€ vs more than 20$)
- Delivery time (I already had it lying around in my studio)
- On-board buttons (connected to RTC pins) and IPS display

CONS
- IPS display may affect battery power consumption


The plan is to keep powered off everything but one RTC timer, which calculates the elapsed time and is responsible for waking up the chip. When the button is pressed, the ESP32 wakes up and displays the time for a 2 second interval before returning to deep sleep mode.


## Dependencies
The ESP32 system API has various methods for sleep modes and wakeup sources. In this case, I used the functions for "External wakeup (ext0)".

To keep track of the elapsed time, the C standard library does a perfect job. Finally, to control the IPS display I used the TFT_eSPI graphics library for ST7789V drivers.

## Software

## Box and Power Circuit

The system works as expected: when turned on, it starts counting. If the button is pressed, it displays the elapsed time. If the board is powered off or reset, the count goes back to 00:00:00.


