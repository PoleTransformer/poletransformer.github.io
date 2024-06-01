---
layout: post
title: "Incident Response"
date: 2024-01-23
categories: website
---

This morning, I woke up to alerts telling me that 2 of my docker containers went offline. What a great way to start the day! I immediately VPNed into my home network to check on Portainer. I noticed that both containers are just missing, not showing at all in the dashboard. Thankfully, the volumes were not deleted.

My first suspicion was the Watchtower container. You see, Watchtower updates docker container automatically. Looking at the logs I see this:

`invalid config for network host: invalid endpoint settings: network-scoped alias is supported only for containers in user defined networks`

The 2 containers in question required host network connectivity. Reason is they use a large number of ephermoral ports, which will cause Portainer to crash if I try to map more than 1000 ports to the host container. How do I know this? From too much experimentation... The words "user defined networks" gave me a hint to manually create a bridge network for testing. Upon switching to my manual bridge network, the error went away and everything started working again.

Obviously, I cant use a bridged network as the ports are not exposed to the host by default and for the reasons I explained earlier. So after deleting the docker container and recreating it with the host network, no errors occured. It seems like the problem only happens when a container is recreated. My next step was to temporarily put Watchtower in manual mode to ensure it doesn't update containers automatically for now. If an update is released, I will go and manually update.

I wanted to be notified when a new update is available. Thankfully, Watchtower provides shoutrrr notifcations and I am able to connect to my self hosted Matrix server to send messages to myself. Using the flags:

`--notification-url matrix://homeserver_goes_here --monitor`

This way I can get a notification when a new update is available and Watchtower won't recreate the containers. The incident was resolved at 2:40pm and started at 4am. Lesson learned here to not always trust automatic updates as they can break stuff. You can track the GitHub issue [here.](https://github.com/portainer/portainer/issues/10993)
