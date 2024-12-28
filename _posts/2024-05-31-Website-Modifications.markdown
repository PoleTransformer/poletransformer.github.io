---
title: "Website Modifications"
date: 2024-05-31
categories: website
---
# This site will NEVER have analytics or any third party garbage!!

Im not a big fan of relying on an internet connection to develop any project. In the past, this blog required establishing a connection to `codeload.github.com` in order to pull the remote moonwalk theme. I have integrated the moonwalk theme directly into the repository. Now no internet connection is required to build this blog.  External javascript dependancies for Mathjax are now local. Since the moonwalk theme is now local, I can modify the CSS without the risk of it being overwritten. This allows me to modify the site's frontend. Like usual, Javascript is completely optional, if disabled you won't be able to change the theme(light/dark mode) and Latex equations will not render. Obviously don't trust me and look at the source code for yourself if you don't believe me. Get into the habit of using your browser developer tools to audit the websites you visit, it tells you a lot.

I also want to keep all resources local, including all images. Not a great practice since it uses a lot of space, but this also serves as a mini internet archive. If any images were to get removed from external websites in the future, you can still see them. If I go pass Github pages size limit, I will host this website on one of my Oracle servers. Also better privacy, you are not reaching out to any 3rd party servers and I want to keep it this way.