---
layout: post
title: Homelab
---

## The Beginning
This all started when I found a free laptop while biking. It was a gaming laptop from 2013 with a 3rd generation 4 core Intel processor(with hyperthreading) and 8GB of DDR3 PC3-12800 RAM in a dual channel configuration. The first thing I did was take it apart and clean the insides. This was a while ago, so I don't remember exact details. I only know it was dusty and had dry thermal paste. I used Arctic MX4 and repasted the CPU and GPU. Next, I installed Ubuntu Server 20.04 LTS. Unfortunately, I didn't document everything online, but I will attempt to catch you up on this blog.

## DIY Router
Using a Dell Optiplex SFF I obtained from the university, I installed Proxmox, which is a virtualization environment. This allows me to run LXC and virtual machines. I wanted to replace my trash ISP router as it was quite limiting. I wanted more control over my equipment and network routing. Installing PFsense was quite easy, simply download the ISO file using Proxmox and install it. I bought a Intel dual port gigabit NIC off of eBay for around 20CAD. What I love about these Dell Optiplex's is how easy it is to install or remove hardware. You take the front panel off, pop out the PCIE cover, put the network card in, and close everything up. A bit easier said than done, do be careful when removing the PCIE metal bracket as its quite sharp, you don't want to cut your finger open.

## Suricata IPS/IDS

I am opening ports in the firewall, so its important to use an intrusion detection and prevention system to stop potential attacks. I admit that I am not a big target(yet), but I think learning Suricata is still important. Installing Suricata in pfSense is pretty easy, simply use the package manager. Its important to first run Suricata in IDS only mode to ensure that you don't start blocking legitimate traffic. Unfortunately, encrypted transports such as TLS and QUIC are pretty much the standard now, so there will be false positives. This is the reason why most workplaces use a technique called HTTPS inspection. I'm not a fan of this technique as it breaks the fundamental concept of encryption. Anyways lets not talk about that now.

![Suricata Blocked Page](/images/suricata_blocked.png){:.centered}

Suricata is enabled on the WAN interface. I'm only interested in Out > In traffic.As you can see, the majority of blocked IPs are port scanners. The blocked 8.8.8.8 Google DNS server is fine because I run my own recursive DNS server at home, which allows me to setup internal DNS and customize minimum TTLs. There were a couple of false positives as shown below:

![Suricata Disabled Rules](/images/suricata_disabled_rules.png){:.centered}

I actually was blocked by my own firewall twice. I couldn't VPN in because my computer apparently sent a 3 way handshake with wrong ack. Although I ran Suricata in IDS only mode for 12 hours and disabled over 40 rules, there is always that one rule that I miss. This shows how tuning Suricata is continuous thing, you never set it and leave it. I also need to thank my mobile data connection, which provided an unblocked backup IP in case I was locked out.

### May 31, 2024
In the end, I decided to run Suricata only on ports exposed to the internet. Not gonna go into much detail here, but this significantly reduced false positives.

## Oracle Cloud: THE CLOUD IS JUST SOMEONE ELSE'S COMPUTER!!
Somehow I was able to signup for [Oracle Cloud Free Tier](https://cloud.oracle.com/free). After a bunch of failed attempts, switching networks was my solution to signing up. *wink wink* hint to those that are strugging to signup. I decided to spin up one AMD instance. In the free tier, you get access to 2 AMD x86 instances and a A1 arm based vps. You can spin up as many A1 instances, but you must be within the 200GB storage, 24GB ram, and 4 CPU limit. If you stay within these limits, in theory, its free forever. Only time will tell, but lets hope my account doesn't get suspended for no reason.

### May 31, 2024
Well its been a couple months and I haven't been banned yet. Yay!! I only had one downtime, which lasted about a day affecting all free tier instances. Other than that, no issues.

## Exposing your homelab to the internet
The best answer is: You don't. However, that isn't really the answer many people want to hear. It is possible to "safely" expose your homelab. The first technique is VPN. You setup a OpenVPN or Wireguard server on your LAN and open a single port on the firewall. This allows you to access everything through an encrypted tunnel. Now VPNs are supposed to be used like this, not some BS third party server on the internet thats promoted by YouTube influencers(not related, but the Honey scam lol) that have no clue what they are talking about. Since the outer tunnel is encrypted, whatever you send through can be in the clear. This is the best if you are a single user accessing your homelab. Now what if you have a couple friends and family members that want external access? Not gonna be practical to install a VPN on all their devices. The answer is: Cloudflare proxy or reverse proxy.

Using Cloudflare proxy is fairly simple, you register a domain name, change the NS records to Cloudflare, add A record, turn on proxying, and done! Any traffic hitting your domain goes through Cloudflare's WAF and DDos mitigation rules. If your service gets attacked, its Cloudflare getting that traffic and not your homelab. You still need to open your firewall, except this time, you only allow Cloudflare endpoints to hit your internal network. So to all the bots and threat actors, your port looks closed from the outside. Of course this brings up privacy problems. Cloudflare is doing a MITM on your traffic, people say "you are letting Cloudflare inspect your traffic so its not a MITM". Unfortunately, you don't know what Cloudflare is doing with that traffic and the service is free. You might say: "Its to train their algorithms to protect against threats." You may be right, but I don't feel comfortable proxying sensitive homelab services through them.

The next best thing is setup a dedicated reverse proxy in a DMZ. This should be isolated from your internal LAN. From your reverse proxy, you should not be able to access anything else other than the backend. The backend should not have access to anything else other than reverse proxy. This ensures that if the machines get compromised, an attacker cannot perform lateral movement. You should also be running an IPS on the reverse proxy. The issue with running IPS at the edge router is it can't see inside encrypted traffic. Running IPS on the reverse proxy allows inspection of HTTP traffic after decryption. Your web application should also be running with least previlege, either in a Docker container or as a seperate user. The reverse proxy service should also be running as a non root user. Of course, enable unattended-upgrades for all VMs to keep everything up to date. Docker containers should always be running Watchtower for auto updates.

On the edge router, you should use a random ephermeral port and perform source IP geoblocks. I know, security through obscurity, but these will help reduce bot scans and prevent script kiddies. Not gonna stop a global adversary, but if your threat level is that high, you might want to reconsider exposing services to the internet or even using the internet...