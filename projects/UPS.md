---
layout: post
title: UPS
---

I found a free APC BackUPS 1500 at the e-waste bin at University. Plugged it in to test and of course battery is shot. This UPS uses a 24V 9aH battery, which is quite weak to my tastes, I want to upgrade to a much bigger one. Of course, it is important to understand the risks of doing this. First, this UPS has no active cooling, so don't put a 100aH battery in there and expect it to continuously provide ~900W for 2 hours, it will probably overheat and MOSFET explodes!!! Next, charging the battery can take a very long time, haven't measured the charge current, but assume these UPSs use a CC method to charge batteries.

~~I plan on replacing the batteries with 2 12V 18aH LiFePO4 in series.~~ This will give my 24V @ 18aH. The original UPS had 2 SLA batteries in series. Downsides to SLA batteries is much lower charge cycles(~500) compared to LiFE(~2000+) and a lower discharge limit(~50% of capacity), reason is even deep cycle SLAs can have their lifespan significantly shortened if you discharge them fully. With LiFE, this is no problem, so a 18aH LiFE is roughly equal to a 36aH SLA battery since you can fully discharge them without ruining their lifespan. A downside to LiFE is the higher charging voltage, I will try to reprogram my UPS first to see if its even possible. Remember its a horrible idea to use car batteries. First, they are flooded, which means you need to top them up and there is a risk of hydrogen production(NICE!!). Second, they are not designed for deep cycle applications such as a UPS, so they will die very quickly.

Welp after some reconsideration, I decided to purchase 2 SLA batteries 35ah. The reason is the ups I have is not the Smart series, so I am unable to program anything with a serial port. This means I can't change the battery float voltage, this needs to be higher for LiFE batteries.

I want to open and inspect the UPS for any damaged internals before I put this into production. Good practice to do this with all equipment you find for free. There were 3 hidden screws behind the front plastic cover. Use a screwdriver or opening pick and start from the bottom and work your way around. Don't pry from where the buttons are because that will break the plastic. There was also another hidden screw near the battery door, make sure you remove that.
<div style="text-align: center;">
    <figure style="display: inline-block;">
        <img src="/images/UPS_front.jpg" width="400"/>
        <figcaption>Hidden Screws behind Front Plate</figcaption>
    </figure>
    <figure style="display: inline-block;">
        <img src="/images/hidden_screw.jpg" width="400"/>
        <figcaption>Hidden Screw near Battery Door</figcaption>
    </figure>
</div>

Wiggling the case and pulling up on both sides, I was able to seperate the case and see the internals. Upon initial inspection, everything looks good, no black marks on the pcb, so I applied 24V using my PSU to simulate a battery.

<div style="text-align: center;">
    <figure style="display: inline-block;">
        <img src="/images/closeup1.jpg" width="400"/>
        <figcaption>Closeup of PSU connection and PCB</figcaption>
    </figure>
    <figure style="display: inline-block;">
        <img src="/images/UPS_internal_wide.jpg" width="400"/>
        <figcaption>Wide angle shot of internals</figcaption>
    </figure>
</div>

I pressed the power button and UPS immediately starts to beep with error code F02. Quickly shutting everything off, I consult the manual.

<div style="text-align: center;">
    <figure style="display: inline-block;">
        <img src="/images/ups_manual.png" width="400"/>
        <figcaption>Error codes</figcaption>
    </figure>
    <figure style="display: inline-block;">
        <img src="/images/pcb_back.jpg" width="400"/>
        <figcaption>Back of PCB, very clean</figcaption>
    </figure>
    <figure style="display: inline-block;">
        <img src="/images/display_cable.jpg" width="400"/>
        <figcaption>Remove display connector carefully!!</figcaption>
    </figure>
</div>

Ok so I have on battery output short. I use my multimeter in continuity to check for short circuit on the output outlets and nothing. Then spent the next hour checking for short circuits on capacitors on the PCB, still nothing. Took the PCB out and checked the H bridge MOSFETs from Gate-Source, and Source-Drain for the body diode. Everything checks out fine. Eventually, I went on the internet and someone mentioned a weak battery also causing this issue. Now my power supply is set at a current limit of 500mA, I increased this to around 1A and no more beeping!! The UPS drew a CC of 700mA and I can hear a 60HZ hum from the huge transformer. Measuring the output, I see 110V, a bit low but its working. I have confirmed the power electronics work, now I need to make sure I can interface with the UPS via USB. I plan on using this UPS to safely shutdown my Proxmox server during a power outage.

Unfortunately, this UPS doesn't have a USB port, but uses an ethernet port. WHY??? Cmon APC just use a bloody USB C connector... Anyways, I needed to figure out the pinout so I can make my own connector because I am not paying the 30 dollars for a new one. A bit of searching and I found this [website](https://pinoutguide.com/UPS/apc_usb_cable_pinout.shtml) showing me the pinout of the connector. Of course APC had to use a RJ50 connector instead of RJ45, so I needed to solder jumpers to the PCB.

<div style="text-align: center;">
    <figure style="display: inline-block;">
        <img src="/images/rj50.jpg" width="400"/>
        <figcaption>RJ50, observe 10 slots, 8 pins, 2 unused</figcaption>
    </figure>
    <figure style="display: inline-block;">
        <img src="/images/serial_test.jpg" width="400"/>
        <figcaption>Testing before crimping port</figcaption>
    </figure>
</div>

The connector size is the same as RJ45, so I can use a standard ethernet connector and crimp in a USB cable. Since I cannot access the most left and most right pin of the connector, I needed to jump them to other pins. Thankfully, not all pins were used for data communication, only 4/8 are used. Pin 1(most left) is +5v and Pin 10(most right, counting all slots) is D+. Remember I can only access pins 2-9 with a standard RJ45 connector. I soldered Pin 1 -> Pin 2 and soldered Pin 10 -> Pin 8.

This is how I crimped my USB cable to the ethernet port, for black wire you can use either pin 3 or pin 6:
<div style="display: flex; justify-content: center; align-items: flex-start;">
  <div style="margin-right: 20px;">
    <pre><code>
    1 -> +5V
    2 -> NC
    3 -> GND
    4 -> X
    5 -> X
    6 -> GND
    7 -> D+
    8 -> D-
    </code></pre>
  </div>
  <div>
    <figure>
      <img src="/images/diy_serial_cable.jpg" width="400" />
      <figcaption>The abomination I made with a USB cable</figcaption>
    </figure>
  </div>
</div>