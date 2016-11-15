+++
categories = ["diy", "alexa"]
date = "2016-09-25T17:06:26+01:00"
description = ""
draft = true
tags = ["alexa", "echo", "espruino", "javascript", "infrared"]
title = "\"Alexa, turn on the tv\""

+++
<br>
Just a quick one before you read the title and rush out to buy an Echo.

- The hardware is <i>~</i>
- The usefulness of Alexa is <i>~</i>
- The 3rd party skills are <i>super~</i>

The redeeming quality of Alexa (and the echo devices) is the ability to develop custom functionality <i>butttttt</i> it comes at a cost of unnatural unique invocation names or limited subset of actions <i>sigh</i>.

/rant

## Overview

The aim of this project is to be able to say "Alexa, turn on the tv" and of course, have the TV turn on.

##

Hardware:

- Panasonic TX-48AS640 TV
- Amazon [Echo](https://www.amazon.co.uk/dp/B01GAGVIE4/ref=fs_dop) / [Echo Dot](https://www.amazon.co.uk/Amazon-Echo-Dot-Generation-Black/dp/B01DFKBL68)
- Espruino [Pico](http://www.espruino.com/Pico) with [ESP8266](http://www.espruino.com/ESP8266)  / [WiFi](http://www.espruino.com/WiFi)
- [Infrared LED](https://www.rapidonline.com/kingbright-l-34-f3c-3mm-infrared-emitting-diode-58-0100) & [Infrared receiver](https://www.rapidonline.com/vishay-tsop4838-950nm-ir-receiver-module-38khz-49-4728)
- Router that allows port forwarding
- [Raspberry Pi](https://www.raspberrypi.org/) running [Raspbian](https://www.raspbian.org/) acting as a reverse proxy and Dynamic DNS (optional)

Software:

- Javascript (NodeJS & Espruino)
- Nginx (optional)
- NoIP Dynamic DNS (optional)


The flow will be

Alexa voice command ->  
Amazon lamdba ->  
Espruino webserver ->  
Trigger Espruino infrared ->  
TV turns on


- Panasonic TV
- Infrared emitter
- Webserver to trigger infrared  
- link to Alexa


 which means we'll need to use the built in Smart Home API/Skill Adapter.


That's the voice command side of things planned, now for the hardware.

aim: control a tv (panasonic) via an Amazon Echo

The type of tv will become important as it will dictate the infrared protocol and payload.

things -  

  javascript,
  static ip or dynamic dns


- TV  
- Espruino [Pico](http://www.espruino.com/Pico) with esp8266 or [Wifi](http://www.espruino.com/WiFi)  
- Infrared LED  
- Infrared receiver  
- Breadboard  
- Amazon [Echo](https://www.amazon.co.uk/Amazon-SK705DI-Echo-Black/dp/B01GAGVIE4) or [Echo Dot](https://www.amazon.co.uk/dp/B01DFKBL68/ref=fs_bis/252-0280083-8770855)


## Infrared

https://learn.adafruit.com/ir-sensor/overview
http://www.learnabout-electronics.org/Oscillators/osc40.php

-decoding

## Programming the Espuino


## Alexa, AWS & the smart home api

- <b>Alexa</b>

Amazon recently released the Echo and Echo dot in the UK, hardware clients to the Amazon voice service and interface, Alexa. Alexa is Amazon's entry to the ever growing voice assistant market with the major differentiator being the support for 3rd party integrations via its open~ api.

Interactions with Alexa depend on whether a built in feature already covers the voice command (sample utterances) or a 3rd party skill has been installed and is available to handle the request.

For example <b>play</b> is reserved for the built in ability to play music from the selected Amazon authorised source, configurable from within the Alexa app.   

- "Alexa, <b>[play]</b> <i>{artist or song}</i>"

The Smart Home API also reserves a number of keywords/phrases such as <b>turn on|off</b>.

- "Alexa, <b>[turn on]</b> the <i>{smart home device name}</i>"

3rd party skill:

- "Alexa, ask <b>[skill-name]</b> <i>{skill action}</i>"

- "Alexa, ask <b>[skill-name]</b> <i>{skill action}</i>"

- "Alexa, <i>{skill action}</i> using <b>[skill-name]</b>"

- <b>Smart Home API</b>

yep, big enough to need its own section.

- discovery
- interactions
