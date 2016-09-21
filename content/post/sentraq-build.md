+++
categories = ["diy", "keyboard"]
date = "2016-09-20T22:23:13+01:00"
description = "Sentraq S60-X build"
tags = ["sentraq"]
title = "Building the Sentraq S60-X"
+++
<br>
Bought from [Massdrop](https://www.massdrop.com/buy/sentraq-60-diy-keyboard-kit), the Sentraq S60-X is like the name hints, a 60% DIY mechanical keyboard. Marketed as an entry level introduction to DIY keyboards, the Sentraq features a [TMK](https://github.com/tmk/tmk_keyboard) flashable chipset and EZ \*5~ piece assembly; perfect for my two left hands.

\*circuit board, plate, case, stabilizers and switches

<br>

__<i>the finished result</i>__

{{< figure src="/end-result.png" >}}

<i>White plastic case, black plate, blank pbt keycaps and cherry brown switches.</i>
<!--more-->
<br>

## Getting started

Apart from the obvious equipment like a soldering iron, the simplicity of the build means that nothing extra is needed, all the components are already included inside the kit, even an ugly usb micro cable, <i>score!</i>

There's 130~ solder joints to make but don't let that put you off, the sockets are nicely separately and the action of clicking in the switch to the plate helps to keep the switch pins nice and tight. A soldering novice would have no problems making a decent connection, just make sure the you have a clean tip.

<br>

## Stabilizers

Stabilizers stabilizers stabilizers!1! It's tempting to start by inserting the switches straight into the plate <i>(I did and ended up having to undo all of them)</i> but hold off, it will make the build 10x harder, if not impossible to line up the plate to the board.

{{< figure src="/stabilizers_2.png" >}}

The stabilizers themselves are a little fiddly to put together, but simple enough once you've done a pair. When mounting the stabilizers be careful to align them correctly to your layout; I went with [ansi](/ansi_layout.webp)).

[Jack Humbert](https://www.youtube.com/channel/UCUodUNwfU_X_W_8nkUNQDkg) provides a fantastic [video](https://www.youtube.com/watch?v=S2FApwzVxAQ) on how to assemble and mount the stabilizers, it's for a different DIY board but it's still relevant.

<br>

## Plate

Next up is mounting the plate. Part of the reason for inserting the stabilizers first is because the plate goes on top! <i>(dur)</i>. When resting on just the stabilizers the plate is a bit wobbly, this goes away as more switches are soldered to the board but it does make the first few a bit tricky.

{{< figure src="/mount_the_plate.png" >}}

Soldering the switches around the corner positions tighted the plate up to the board and meant the remaining switches needed less pressure to keep the pins fully penetrated.

<br>

## Switches, case & keycaps

Not much else to say other than continue soldering the rest of the switches and then screw in the case. I would advise not screwing the board into the case until every key has been tested

{{< figure src="/without_caps.png" >}}

<br>

## ruh roh

As with everything I build, there's always a problem of some sort, this time I had a non functioning key.

After assembling the entire board and what I thought was a firmware problem, I realised that the bottom left super key was not triggering. Checking, replacing and resoldering the switch made no difference, something else was wrong.

<i>And that's when I noticed...</i>

{{< figure src="/d60.png" >}}

D60 looks a bit different to D59, wtf. Diode no.60 is missing a case and the right pin no longer makes a connection to the body, ruh roh.
Massdrop support were helpful and fast to reply but they wanted to replace the board I had just poured hours of love into, not happening.

Due to having the patience of a five year old at christmas, I started to question the existence of D60. Why does a keyboard need diodes?

Simply put, diodes ensure electrical current can only pass in one direction ([wiki](https://en.wikipedia.org/wiki/Diode)). Keyboards care about this because switches are wired in parallel with multiple keys sharing the same paths with the detection of key presses being based on shorting the path. To allow for multiple  
keys to be pressed at the same time along the same paths we need a clean signal path, which is exactly what the diodes are doing ([source](http://blog.komar.be/how-to-make-a-keyboard-the-matrix/])).

Much deliberation with the keyboard gods later, and a lack of care for antighosting it was decided. I shorted D60 and directly joined the two pins together. With the use of [mircosoft's ghosting demo](https://www.microsoft.com/appliedsciences/antighostingexplained.mspx) it turns out I got away lucky this time.  

<br>
// Disclaimer I'm no electrical engineer.
<br>
// TBC <i>firmware and footpedals.</i>
