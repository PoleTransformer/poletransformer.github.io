---
layout: post
name: Homelab
---
<link rel="stylesheet" href="main.css">

## January 14, 2024
This all started when I found a free laptop while biking. It was a gaming laptop from 2013 with a 3rd generation 4 core Intel processor(with hyperthreading) and 8GB of DDR3 PC3-12800 RAM in a dual channel configuration. The first thing I did was take it apart and clean the insides. This was a while ago, so I don't remember exact details. I only know it was dusty and had dry thermal paste. I used Arctic MX4 and repasted the CPU and GPU. Next, I installed Ubuntu Server 20.04 LTS. Unfortunately, I didn't document everything online, but I will attempt to catch you up on this blog.

## DIY Router Time!!
Using a Dell Optiplex SFF I obtained from the university, I installed Proxmox, which is a virtualization environment. This allows me to run LXC and virtual machines. I wanted to replace my trash ISP router as it was quite limiting. I wanted more control over my equipment and network routing. Installing PFsense was quite easy, simply download the ISO file using Proxmox and install it. I bought a Intel dual port gigabit NIC off of eBay for around 20CAD. What I love about these Dell Optiplex's is how easy it is to install or remove hardware. You take the front panel off, pop out the PCIE cover, put the network card in, and close everything up. A bit easier said than done, do be careful when removing the PCIE metal bracket as its quite sharp, you don't want to cut your finger open.

## Oracle Cloud Experiments
Somehow I was able to signup for [Oracle Cloud Free Tier](https://cloud.oracle.com/free). After a bunch of failed attempts, switching networks was my solution to signing up. *wink wink* hint to those that are strugging to signup. I decided to spin up one AMD instance. In the free tier, you get access to 2 AMD x86 instances and a A1 arm based vps. You can spin up as many A1 instances, but you must be within the 200GB storage, 24GB ram, and 4 CPU limit. If you stay within these limits, in theory, its free forever. Only time will tell, but lets hope my account doesn't get suspended for no reason. REMEMBER: The cloud is just someone else's computer. Assume everything you store on there can be read in plain text and modified. DONT STORE ANY IMPORTANT DATA ON THE CLOUD, ESPECIALLY A FREE CLOUD PROVIDER!!!!! MAKE BACKUPS REGULARILY!!!!
