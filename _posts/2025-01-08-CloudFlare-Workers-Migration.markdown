---
title: "CloudFlare Pages migration and Macless Haystack endpoint"
date: 2025-01-08
categories: website
---

New year so lets make this blog more professional. I am migrating off of Github Pages to CloudFlare so I can get rid of the `github.io` TLD. I know you can define custom domains in settings, but doing a bit of research online, there seems to be certificate renewal errors. Also need to configure DNS and of course I am lazy.

Migration to CloudFlare workers is easy, just link your GitHub repo. Of course we practice least privilege, so only allow CloudFlare access to your GitHub pages repo and nothing else. An issue while building is locale generation. Thanks to [this](https://herrickfang.com/2023/11/26/cloudflare-workers/) blog post, I was able to solve the problem. Make sure to add `LC_ALL=C.UTF-8` to the build setting environment variables.

I have added a customized local Macless Haystack frontend to my blog as well. This version strips out as much third party server connections, so you only reach out to `openstreetmap.com`, everything else is local. I don't understand why these sites need to constantly fetch js files and fonts from `gstatic.com` when the files are literally in the repository... Macless Haystack allows you to use the Apple Findmy network without owning an Apple device, including Airtags. You turn an ESP or NRF into a bluetooth beacon. I will make another project post on exactly how it works.