---
title: "Using Wingo with the Ubiquiti Dream Router"
date: 2022-09-21T21:27:38+02:00
draft: true
categories: [ "internet", "home-office" ]
---

I've recently switched my internet provider and with that completely renewed the
devices I'm using for the setup. And while looking for the appropriate hardware 
on sites like [Galaxus / Digitec](https://galaxus.ch) I've noticed that I wasn't 
the only one who was wondering if certain Routers / Modems are compatible 
with particular providers in Switzerland. 

Thus, in case you are on the same journey, I want to briefly explain my setup.

## Choosing a provider in Switzerland

I'm one of the unfortunate who still don't have a fibre connection in Switzerland.
However, I'm lucky enough to have a *G.fast* compatible line which enables speeds up to 
500 Mbit/s / 150 Mbit/s. Make sure to check the available lines for your place with the providers
connection checker. 
In Switzerland, Swisscom provides a pretty good network infrastructure - thus I've choosen
[Wingo.ch](https://wingo.ch) for my home internet which operates on said Swisscom infrastructure.[^1]

## Wingo Hardware

The "drawback" with Wingo is that you'll get a Swisscom router with a 
Wingo sticker on the front as a poor man's rebranding option[^3] and is completely down stripped. 
You won't get any administration interface on the router itself. Instead browsing to the routers IP
will give you a static status page, like this:

... IMAGE TO BE ENTERED ...

You are able to change a few router settings in your [Internetanschluss](https://mywingo.wingo.ch/eCare/wireline/de/internet) 
page on [MyWingo](https://mywingo.wingo.ch), like the Wifi name and password[^2] or simple Port Forwarding.

The good thing is that you can and I wanted to use my own modem and router anyways.

## My Modem and Router

I went with the [Draytek Vigor166](https://www.galaxus.ch/de/s1/product/draytek-vigor166-router-14620989) as my G.fast-Modem 
in Bridge-Mode forwarding traffic to my [Ubiquiti Dream Router](https://www.galaxus.ch/de/s1/product/ubiquiti-dream-router-all-in-one-wifi-6-router-4x4-mu-mimoofdma-2x-poe-ports-router-21398637).

## Setup the Modem

Installing the Draytek modem is as simple as plugging it in and changing it to Bridge-Mode.

... IMAGE ...

Wingo / Swisscom by default will use [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT) 
so your IP address won't be a public one. You can easily change this by enabling the *DMZ Function* 
on the Wingo [Internetanschluss](https://mywingo.wingo.ch/eCare/wireline/de/internet) page and set the
IP address to `192.168.0.1`. If you release your DHCP lease you'll get public IP address assigned 
to your router.

[^1]: I'm still baffled by why someone would use Swisscom directly for home internet ... 🤷‍♂️

[^2]: Yes, the Wifi name is enforced to have at least 8 characters ...

[^3]: The router looks like it has been in use before.