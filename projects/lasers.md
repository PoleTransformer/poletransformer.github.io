---
layout: post
title: Laser Experiments
---

A couple years ago, I watched a video from SmarterEveryDay about using lasers to remotely activate smart home devices. Of course I was immediately interested and wanted to replicate the experiment. I thought I needed lab grade parts to perform this experiment, however modulating a laser with a audio signal can be done easily. For the laser, I decided to use a red laser salvaged from a dvd rewriter. This laser diode was extremely powerful and could set matches on fire, so please be careful around these devices. In order to prevent the laser diode from burning out, I mounted it in a case with focusing ring bought on Aliexpress. I also drilled a hole in a old square heatsink to mount the module for easy focusing and aiming. The power supply was a simple LM317 constant current source. No matter what the voltage was, the power supply will always try to deliver the set current into the device. This also prevents thermal runaway to ensure the laser diode does not burn.
\
\
In order to modulate the laser signal, I watched a couple videos and did some research online. Many articles I found used a 1:1 audio transformer placed in series with the laser power supply in order to change the current feeding the laser, thus varying the output amplitude. Below is the schematic I used for wiring:

<figure>
    <img src="/images/Laser_NoAmp.png" alt="No Amp" width="600"/>
    <figcaption>Schematic V1</figcaption>
 </figure>

 I used a transformer I found from a old microwave logic circuit. This was a step down transformer from 120VAC -> 12VAC. In order to identify the primary and secondary, measure the impedance using a multimeter of the leads sticking out of the transformer. In a step down transformer, you should find one winding with a very high impedance(500Ohms). This is the primary. You should also find one or more windings with a low impedance(<50Ohms). This is the secondary. You must use a transformer with either two identical secondaries or a 1:1 transformer. These transformers have the same primary and secondary impedance. Make sure the impedance is not too high, or the laser will struggle to output brightly.
\
\
I hooked up my phone with a prerecorded audio sample of me saying: "Alexa, what time is it?". Next, I turned on the PSU and set it around 4V with a max current draw of about 90mA and aimed the beam directly into the Amazon Alexa center microphone hole. Playing the audio clip did nothing, even at max volume. I was a bit confused as there was clearly sound being outputted as I could hear the audio using a solar panel and amplifier.
\
\
After a bit of thinking and pacing around the room, I thought of using an amplifier to boost the current going into the transformer. Maybe this will increase the amplitude, so the Alexa can now "hear" it? Below is the next schematic I used. Its the exact same, but I just added a modified bluetooth class D amplifier between my phone and transformer:

<figure>
    <img src="/images/Laser_Amp.png" alt="Amp" width="600"/>
    <figcaption>Schematic V2</figcaption>
 </figure>

I played the audio sample again and the Alexa lit up! I held in my excitement before it gave me the time: "It's xx:xx PM. Have a wonderful evening.". I was overwhelmed with joy, feeling very accomplished. How was I able to activate Alexa using a frickin red laser!? MEMS microphones are interesting. Project is dead as of 2024-01-14.
