+++
categories = ["diy", "alexa"]
date = "2016-09-25T17:06:26+01:00"
description = ""
draft = true
tags = ["alexa", "echo", "espruino", "javascript", "infrared"]
title = "\"Alexa, turn on the tv\""

+++

- overview
- infrared
- espruino & porting arduino lib
- alexa & smart home api


## Overview

aim: control a tv (panasonic) via an Amazon Echo

The type of tv will become important as it will dicatate the infrared protocol and payload.

things -  
- TV  
- Espruino [Pico](http://www.espruino.com/Pico) with esp8266 or [Wifi](http://www.espruino.com/WiFi)  
- Infrared LED  
- Infrared receiver  
- Breadboard  
- Amazon [Echo](https://www.amazon.co.uk/Amazon-SK705DI-Echo-Black/dp/B01GAGVIE4) or [Echo Dot](https://www.amazon.co.uk/dp/B01DFKBL68/ref=fs_bis/252-0280083-8770855)


## Infrared

https://learn.adafruit.com/ir-sensor/overview
http://www.learnabout-electronics.org/Oscillators/osc40.php

## Programming the Espuino


## Alexa, AWS & the smart home api

Why the smart home api over the standard skill kit

- discovery
- interactions
