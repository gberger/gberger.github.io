---
layout: post
title: "Our safety app got 1st place at HackPUC!"
permalink: /blog/9/hackpuc
redirect_from:
  - /blog/9/
---

This weekend, I participated in [HackPUC 2015](http://hackpuc.com), [my school](http://www.puc-rio.br)'s second iteration of its yearly hackathon. 

Long story short: we made a "panic button" app that sends a location tracking and audio streaming link to your previously chosen friends. It's activated via wearables such as the Apple Watch or the Myo, making the activation discreet, imperceptible to your aggressor. **And we got first place!**


## The Setup

College hackathons are only now beginning to spring up in Brazil, after seeing immense popularity in the US and Europe in the past few years. In fact, HackPUC was Brazil's first college hackathon!

PUC-Rio's beautiful campus was home to 120 hackers who for 36 hours ate pizza, drank Red Bull, played arcade games, slept on beanie bags, and, of course, hacked on 30 amazing projects.

The themes were politics, safety, and sports. These are very much in line with current topics in Brazil: corruption scandals and political instabilities plague the cuntry, while the Olympic Games are set to come to Rio in 2016, a city known to be more dangerous than usual. 


## Our Project

We decided to create a safety app designed to help in more serious cases of aggression, such as kidnapping, rape, or a hostage situation.

The app's initial flow asks you to select a few friends or family members that should be notified of potential emergency situations that might happen to you. You can define a custom message that will be sent to them. 

You can then activate the app by making a discreet gesture using Myo, a gesture control armband. The gesture can be something like clenching your fist and twisting your hand, or touching your pinky finger and your thumb together. **What matters most is that by using the Myo your attacker is very unlikely to realize you are making an effective call for help, so he won't be worried about turning off your phone.**

Then, a link is sent to your trusted friends via SMS. This links enables them to track your GPS location and listen in to sound captured from your phone. The link is short, so it can easily be communicated verbally (to police officers, for example). 

![](/img/hackpuc-2.png)

_ "In danger since 11:17:00" / "Last updatedat 11:17:40" / [current address] _

As soon as you are safe, you should type in your passcode so your friends know that you are safe once again. This deactivates the tracking, but your location history and recorded audio will still be stored in our servers as they can be handed over to police by you to help with an investigation.


## Alternatives

There have been a few apps developed with this concept already. For example, [this app]() will alert friends when you press a button -- but only after pulling out your phone, unlocking it, and finding the app, too much to ask of someone in a situation of distress. Other apps require you instead to hold the screen, and if you let go for too long, it assumes you are in danger. This is also not a possibility in Rio: if you walk around with your phone in hands, you will surely have it taken from you by criminals.

Our solution was to use Myo, an armband that recognizes gestures you make with your hand. So if you are wearing it, you can just clench your fist and twist your hand a certain way and the emergency mode will be activated.

Now, we know a Myo is expensive, and walking around with one is not something it's intended for. We thought of other solutions, and although we did not implement them, the app was developed in a way that enabled you to select the device you wished to use with it. 

* Apple Watch: a popular device and natural to carry around
* [Pressy](http://get.pressybutton.com/): when inserted in your phone's headphone plug, becomes a button
* Custom device: we thought that a small custom device, equipped with Bluetooth and a button, that fits in your pocket, could be dedicated to this purpose

**All of these have something in common: they are quick and easy to activate, and they are discreet. You want the emergency activation to be imperceptible to the attacker, so they don't become even more aggressive towards you.**

## Details

### The Team

* Guilherme Berger
* João Vicente Nassar
* Victor Augusto
* Tauan Binato Flores

### Tech Stack

* Swift for the iOS app
* Node.js for the backend
* MongoDB for the data store
* Twilio for sending SMS messages
* Socket.io to update the tracking page in real time
* OpenTok to capture and play streaming audio
* Heroku for hosting

![](/img/hackpuc-1.jpg)