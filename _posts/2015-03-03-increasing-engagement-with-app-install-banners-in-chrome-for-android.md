---
layout: post
title: "Offline-first, fast, with the sw-precache module"
description: ""
article:
  written_on: 2015-03-03
  updated_on: 2015-03-03
authors:
  - paulkinlan
tags:
  - add to homescreen
  - webapp
  - install
permalink: /2015/03/increasing-engagement-with-app-install-banners-in-chrome-for-android
---
We recently enhanced the Add to Homescreen function in Chrome which allows users 
to add your Web App to their home screen with the addition of the 
standards-based "[web app manifest](http://updates.html5rocks.com/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android)". 
The manifest gives you extra control over the Add to Homescreen experience, 
allowing you to tell the browser what to launch, how to launch your app 
(fullscreen or in a browser window) and how it should appear to users on the 
home screen.

This improved things for users, but the ability to Add to Homescreen is still 
hidden behind a menu, meaning that your apps still aren't as discoverable as 
they should be.  To increase the chance of a user adding their app to the home 
screen a developer would have to try and guess if the site was already running 
as an Added to Homescreen app and if not, then tactically decide to give them an 
overlay that asked them to work around our poor UX.  This isn't great for users, 
and it is not good for developers.

In Chrome 42 (M42 - that will soon be in Beta) we are introducing "App Install 
Banners".  App Install Banners give you the ability to have your users quickly 
and seamlessly install your Web App as per the images below.

<img src="image00.png" width="253" height="450" /><img src="image01.png" 
width="252" height="449" />

"This looks great, I want it on my site" I hear you shout. "Please tell me how 
to add it!".  The good news is if you meet the following criteria Chrome will 
manage the prompting of users:

* You have a [web app manifest 
  file](http://updates.html5rocks.com/2014/11/Support-for-installable-web-apps-with-webapp-manifest-in-chrome-38-for-Android) 
  - The manifest defines how your app appears on the user's system and how it 
  should be launched - and you are required to have a \`short\_name\` and a 
  \`144x144\` png icon
* You have a [service 
  worker](http://www.html5rocks.com/en/tutorials/service-worker/introduction/) 
  registered on your site. We recommend a [simple custom offline page](https://github.com/GoogleChrome/samples/blob/gh-pages/service-worker/custom-offline-page/service-worker.js) 
  service worker
* Your site is served over 
  [HTTPS](https://docs.google.com/document/d/1oRXJUIttqQxuxmjj2tgYjj096IKw4Zcw6eAoIKWZ2oQ/edit) 
  (you need a service worker after all)
* The user has visited your site twice over two separate days during the course 
  of two weeks.

If you are interested in the implementation, check out [crbug 
452825](https://code.google.com/p/chromium/issues/detail?id=452825).  If you are 
interested in tracking what other things we are working on in this space, follow 
the 
[Cr-UI-Browser-AppShortcuts](https://code.google.com/p/chromium/issues/list?q=label:Cr-UI-Browser-AppShortcuts) 
label. 

## Frequently Asked Questions

**My app meets all the criteria, but I don't want the banner to display. Can I control this?**  
Not right now. We are working on ways to give developers the ability to cancel 
the display of the prompt.

**Can I detect if a user tapped "Add" at the prompt and added to home screen?**  
Not right now. We are working on ways to give developers the ability to 
understand whether the user has gestured Install or Cancel.

**If a user dismisses the banner, will it appear again?**  
No.  Not unless the user clears their history.  We want to make sure users have 
a good experience.  We will likely be changing all the heuristics over time.

**Can I decide when to prompt the user****?**  
No, we are not letting developers actively prompt the user to Add to Homescreen.

**You said that I will only get the banner if I visit the site on two different 
days.  How on earth do I test it?**  
Yeah, we had the same problem when we were building this.  Enable 
chrome://flags/\#bypass-app-banner-engagement-checks and you will see it as long 
as you have a manifest (configured correctly) and are on HTTPS and have a 
service worker.

**Why do I need a service worker?**  
We believe that when you add to the user's home screen you should be providing 
an app-like experience.  A service worker, especially one that supports push 
messaging or offline, is a strong indicator that you are developing a first 
class device experience.  We recommend the following simple Service Worker as a 
starting point. 

**Why do I need SSL?**  
Because you need a service worker.

**Are there any good examples of this in action?**  
Yes, Glad you asked: 

* [Google I/O 2015 web app](https://events.google.com/io2015/)
* [SVG OMG](https://jakearchibald.github.io/svgomg/) 
  [[Code](https://github.com/jakearchibald/svgomg)]
* [QR Snapper](https://qrsnapper.appspot.com) 
  [[Code](https://github.com/PaulKinlan/qrcode)]

**Examples are great, but do you have anything I can just copy and paste?**  
Yes. We have made a [minimal app install banner 
example](https://github.com/GoogleChrome/samples/tree/gh-pages/app-install-banner/) 
that you can copy, paste and change.