---
layout: post
title: Software Defined Radio
---

Sometime around March of 2022, I watched a video by Samy Kamkar about radio hacking. He was able to unlock vehicles and garage doors using software defined radios connected to a laptop. I was instantly hooked and this was when I started the radio hobby. When I was experimenting around with 433mHz transmitters, I was curious on what other radio transmissions are around us. I know law enforcement and first responders use radios in order to communicate with one another efficiently. I now wanted to listen in and see whats on the radio spectrum.
\
\
This is where the software defined radio came into the picture, specifically the RTL-SDR Blog V3. I found this device after searching online for the best software defined radios as well as watching a couple of YouTube videos. I chose the RTL-SDR Blog V3 because it had the most features for the price, which was extremely competitive and affordable.

<figure>
    <img src="/images/rtl.jpg" alt="sdr front" width="400"/>
    <figcaption>RTL SDR Dongle</figcaption>
 </figure>

 This SDR can receive frequencies from 500kHz to 1.75GHz. Especially for this price, it is a great deal. I decided to buy the RTL SDR bundle for 55 CAD, which comes with everything you need to get started since I didn't have any antenna hardware. This kit comes with a dipole antenna with a tripod and other mounting hardware.

 <figure>
    <img src="/images/rtl_kit.jpg" alt="sdr front" width="400"/>
    <figcaption>RTL Bundle</figcaption>
 </figure>

 The first thing I did was plug the dongle into my laptop and explore the radio spectrum(after installing drivers and SDR#). I listened to a couple of FM broadcasts in my local area before moving on to the 400MHz band. The first thing I noticed was a rather strong transmission at 433.92MHz. At first I was confused as to where this was coming from, however from further analysis, this signal was actually coming from my outdoor weather station transmitting to the indoor unit. This transmission happened approximately every 10 seconds, so this was very interesting to both see and hear the transmission. The 400MHz ISM band also contained a couple analog voice transmissions. Upon looking them up in a radio database, I discovered these were for my cities parking services and I could actually hear transmissions about others license plate numbers and parking fines. Going into the 900MHz ISM band, I discovered two strange sounding transmissions at around 929MHz. I consulted the lovely community on Reddit to help give me an explanation. They pointed out that these were actually pagers using the FLEX protocol. This really surprised me as I didn't know that pagers were still being used to this day. These transmissions weren't encrypted in any way, so I was now on a mission to decode it.
 \
 \
 I used the program [PDW](https://discriminator.nl/pdw/index-en.html) and [VB Audio Virtual Cable](https://vb-audio.com/Cable/) in order to pipe the audio from SDR# into the program. A couple moments later, there was a bunch of numbers and text popping up. The first thing I noticed was these messages were all related to patients and hospitals in my area. After a couple minutes of analysis, I concluded this was my local hospital paging network. I could see pages relating to emergency xrays or broken HVAC systems.
 \
 \
 Next, I wanted to listen to ATC, so I brought the setup upstairs for a better view of my local international airport. I tuned the dipole to approximately 120MHz and attached it to the window using the suction cup. Using this setup, I was able to listen to ATC with no problem. It is interesting to hear that once planes land, the signal quality drops significantly compared to when they are in the air. This is likely caused because of obstructions, reducing the radio line of sight. I was able to listen to the tower controller no problem, likely because all ATC towers are high above the ground, allowing further signal propagation. The equipment in use and power levels is also likely much greater compared to the standard commercial airliner. I also opened up FlightRadar24 to track the planes in near real time. It is always fun to follow a plane from ground to when it gets handed over to another jurisdiction outside my radio range.

## Project Update July 24, 2022

Its been some time since the last update, however I have done quite a few cool projects. The first thing I did was to repurpose my Raspberry Pi 3B+ into something more useful. I decided to apply for a broadcast permit in order to stream a fire dispatch radio channel in my neighbouring area online. The reason is that I can get a premium account for free by becoming a broadcaster as well as sharpen my Raspberry Pi skills. Unfortunately, the majority of first responder radio traffic in my area has gone encrypted, however a neighbouring fire dispatch broadcast is still in the clear. I am also using the Raspberry Pi as a smart home server for more privacy and security. The brains of this operation is a open source project called [RTLSDR-Airband](https://github.com/szpajder/RTLSDR-Airband). This was actually intended for streaming AM transmissions(Aircraft and ATC) to Icecast servers, however the awesome developers added FM decode support, allowing NFM to be streamed online.
\
\
The project was built from scratch, using ***cmake*** to generate the initial build files, and then ***make*** and ***sudo make install*** to install the program. Make sure to add ***-DNFM=ON*** when generating build files in order to enable NFM decoding, or else you will run into trouble later. The configuration file was created in ***/usr/local/etc/rtl_airband.conf*** and modified to suit my needs. One tip is to add a ***notch*** under ***channel configuration*** if the frequency uses a PL tone. Without this notch, you will hear this annoying sound in the final broadcast. Finally, a systemwide service was registered from the configuration file under the ***init.d*** directory.
\
\
The RTLSDR is plugged into the Raspberry Pi and I am using the included dipole antenna mounted to a window using the suction cup as well as the extension SMA cable. The Raspberry Pi is connected via a gigabit ethernet connection for maximum reliability.
\
\
With my only SDR in use, I decided to buy two more dongles from the official seller on Aliexpress. Be very careful where you purchase these dongles as there are counterfeits everywhere. I wanted to be able to listen to a couple trunked radio systems in my area. A trunked radio system usually uses a control channel that all radios tune into. When someone wants to speak to someone else in the same talkgroup, the control channel assigns that group a frequency to tune into. All radios in that talkgroup tune in to listen. If another person in that talkgroup wants to transmit, the control channel may or may not stay in the same frequency. The frequency could change or stay the same for that next transmission. This makes it impossible to follow a trunked radio system using only one dongle. One dongle must be constantly listening to the control channel, while the other tunes to the different frequencies. The advantage to this is that fewer frequencies can be allocated, which frees up the radio spectrum. All of the trunked radio systems in my area are digital, which means I need additional software to decode the transmissions. This is different from encrypted digital transmissions, where an decryption key must be known in order to hear whats being said. Fortunately, most trunked radio systems in my area are not encrypted, however all police, fire, and ambulance transmissions are encrypted.
\
\
I decided to follow a NXDN system in the ISM region at around 410-422MHz. I simply used a female SMA connector and a 1/4 whip antenna(~18cm) to make the antenna. Unfortunately, I live in an area far from the majority of radio communications, so this setup is not ideal, however I am still able to pick up quite a lot of frequencies. I used the program [DSDPlus](https://www.dsdplus.com/). Unfortunately, I had to switch to Windows from Linux because of software issues. All software such as SDRTrunk were not able to decode NXDN systems yet, however DSDPlus is only a Windows program. Using it through Wine is no problem, however the driver side of the program was written in Windows, which makes it unusable even with wine in Linux. There was a bit of configuration needed, however I was able to get all of the information I needed from an online radio database.
\
\
At last I was able to listen and follow conversations from people. The signal strength in my area is not the best, so sometimes I get a couple dropouts. This particular trunking system is shared by quite a few buisinesses. It is used by the security in several hospitals, however I rarely hear conversations from these hospitals. The majority of the conversation comes from local bus and truck drivers. There are also a couple encrypted channels, which are used by a local whale watching business as well as an unknown channel. After figuring out priority settings, I was able to ignore these encrypted channels and only listen to the ones in the clear. This was extremely fun and interesting, I sat and listened for over 3 hours one day.

## Project Update September 22, 2022

Over the summer, I spent a couple days receiving weather satellites from space. Specifically, the NOAA and METEOR satellites. These take pictures of the earth using a variety of cameras, then beam these signals to earth with a analog or digital transmission. The NOAA satellites are the easiest to receive because it is an analog signal, the signal can be decoded even with a weaker signal strength. Unfortunately, the METEOR satellites use a digital transmission, which means much higher SNR is required.

## Project Update January 14, 2024
My [Broadcastify](https://www.broadcastify.com/webPlayer/38213) feed has been running for 2 years now. There are times where RTL Airband crashes, but other than not not really any problems. There have been reports of users hearing unintelligible traffic, I have fixed this issue somewhat by using a dipole antenna mounted as high as possible. In the future, I plan on adding a second RTLSDR dongle and building dedicated VHF and UHF yagi antennas. This allows me to remotely use the RTLSDRs from another location. A great project that connects to my homelab projects.

Unfortunately, in August 2023, Translink switched to ECOMM's encrypted network. I used to listen to Translink all the time and its sad that everyone is going encrypted P25 PII. I only hear periodic testing of radios in the clear, everything else is encrypted. Obviously, many will suggest safety issues and privacy especially for police agencies, but the fire department and translink?? This makes no sense. I want to know what is happening if im on the skytrain and it suddenly stops. Is broadcasting that information in the clear really that much of a safety issue? Sorry for my rant, I just wish less agencies were going encrypted. Thank you for the Burnaby Fire Dispatch for staying in the clear!! :)
