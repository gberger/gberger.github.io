---
layout: post
title: "Playing with the Twilio API"
description: "During HackGT, I built 3 apps using Twilio's API"
permalink: /blog/6/playing-with-the-twilio-api-at-hackgt
redirect_from:
  - /blog/6/
---

This past weekend, I attended [HackGT](http://hackgt.com/) at Georgia Tech.

[Twilio](https://www.twilio.com/) was one of the sponsors and they gave us 
all the support we needed to get going with their API, which enables you to 
send and receive text messages, media messages and phone calls.

Because of how simple, easy and fun it is to build something using Twilio, 
I actually ended up building 3 small, sample apps. Click the names to go 
to their GitHub repositories with screenshots and code.

### [jsms](https://github.com/gberger/jsms)

In a whopping 20 lines of CoffeeScript, I'm able to listen for incoming
text messages, evaluate the body as JavaScript code inside a sandbox,
and send back the result to the sender.

### [immsgur](https://github.com/gberger/immsgur)

Using an MMS-enabled Twilio phone number, I listen for incoming images
and upload them to [Imgur](https://imgur.com/). The URLs are sent back
to the user.

### [randwilio](https://github.com/gberger/randwilio)

"Omegle for SMS". Connects two random users and let them chat anonymously. 
The server matches people and proxies their communications, maintaining anonimity
and privacy. It also ensures two people can't match twice.

This was executed quite quickly and I didn't put too much branding into it.
I also didn't enter it as my hack.    
However, one of the top 10 teams at HackGT, [Phonely](http://hackgt2014.challengepost.com/submissions/26964-phonely),
independently had the same idea. I congratulate them on their execution
and recognition!

----

Using the Twilio API has been a great joy, and I hope I can incorporate
it into more projects. I extend my thanks to the Twilio reps at HackGT
who made it easy for us to get going with their platform!
