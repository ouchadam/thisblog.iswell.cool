+++
categories = ["diy"]
date = "2016-11-21T20:00:00+01:00"
description = ""
draft = false
tags = ["diy", "wol", "javascript", "nodejs", "raspberry pi"]
title = "Wake up, yo"
+++

<br>

Unless you're lucky enough to have one of those fancy routers running custom firmware like ddwrt, when it comes to wake on lan, you need to be on the local network. Or you can go down the painful and scary route of allowing external magic packets to be forwarded...

A simple solution is to make the wake on lan request from inside the network. Wake on lan is a <i>magic packet</i> (yes that's the real name), typically broadcast over UDP to the address of 255.255.255.255 (the entire local network).

Sounds like a job for a spare raspberry pi.

<br>

### Prerequisites

- A device/environment capable of running all the tools, such as a raspberry pi running [raspbian](https://www.raspbian.org/).
- [npm](https://www.npmjs.com/) & [nodejs](https://nodejs.org/en/).
- a static local ip address assigned to the device/environment.
- Port 80 forwarded by your router to the device/environment.
- [nginx](https://www.nginx.com/)
- dynamic dns (unless you have a static ip from your ISP).

<br>

### Back-end

First of all, we're going to need a server to listen to our external wake up requests.

Lets do this with nodejs by creating a new javascript file, `index.js`. We can run the server using `node index.js`. Any time `require('module-name')` is used, we'll need to run `npm install module-name` which will pull down the dependencies into `node_modules`.

```javascript

  const express = require('express');
  const app = express();
  const httpServer = require('http').Server(app);

  httpServer.listen(3001, function() {
    console.log('listening on *:3001');
  });

```

great, we've got a server now what? Let's create and send a magic packet using the [wake_on_lan](https://github.com/agnat/node_wake_on_lan) module.

```javascript

  const wol = require('wake_on_lan');

  app.get('/wake', function(req, res){
    wol.wake('12:AB:34:CD:EF:56');
    res.end();
  });

```

bam, we've got a back-end that will dispatch a wake on lan magic packet for a hardcoded mac address when the `/wake` route is hit.

If everything went well, with the server running we should be able to see our wake up route in action with:
```bash

  curl -v localhost:3001/wake

```

<br>

### Exposing to world

This is where things get <i>networky</i> and confusing. We need to hit our server from outside of the local network, no more localhost. If you're lucky enough to have a static ip then you can pair it up with a domain name and be on your way, for the rest of us we'll need to use a dynamic dns.  

There are plenty of dynamic dns providers, some provide free tiers such as [noip](http://www.noip.com/) and others like [dyndns](http://dyn.com/dns/) offer higher support with routers (rather than manually updating the dns).

There are plenty of guides out there so I won't go into much detail here. Once the dns is setup, it's time to forward those external requests to our internal server.

Using [nginx](https://www.nginx.com/) and assuming that port 80 has been forward to the device, here's a simple web server acting as a proxy pass for our nodejs server using the `/wake` route:

```bash

  server {
    listen 80;
    server_name mydomain.com;

    proxy_set_header        Host $host;
    proxy_set_header        X-Real-IP $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header        X-Forwarded-Proto $scheme;

    location /wake/ {
      proxy_pass http://localhost:3001;
    }
  }

```

At this point, rerunning the `curl` command from earlier with your exported domain name `curl -v mydomain.com/wake` will turn on your desired device from outside the network! If you're planning on providing your own way to triggering this skip to deployment, otherwise lets build a website.

<br>

### Front-end

Now that we have a back-end, we need a way to interact with it. This could be achieved with a bash script, android/iOS application, Alexa integration or even IFTTT, but for this example lets do a simple web page.

To keep things simple I'll be serving the web page from the same nodejs server we created earlier.

First up is creating the page <i>index.html</i> with a button that will be used to hit our back-end `/wake` endpoint.

```html

  <!DOCTYPE html>
  <html>
    <head>
      <title>Wake up</title>
    </head>
    <body>
      <h1>Wake up</h1>
      <button>Wake up, yo</button>
    </body>
  </html>

```

Super ugly but functional webpage. Next on the list is serving the page when we hit `localhost:3001/` in a browser by adding a route on `/` and sending the file we just created.

```javascript

  app.get('/', function(req, res) {
    res.sendFile(__dirname + '/index.html');
  });

```

At this point we should be able to see the page we created and click the wake button! but nothing happens yet... so lets link it up to the back-end with a get request using [jquery](https://jquery.com/).


```html

  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script>
    var onWake = function() {
      $.get("mydomain.com" + "/wake");
    };
  </script>

  ...

  <button onclick="onWake()">Wake up, yo</button>

  ...

```

<br>

### Deployment

Now that everything is up and running (hopefully), it's time to tidy up. First action is moving the code to a more conventional location, such as `/var/www/` with a sub directory `wake-up`.

Since we're using nodejs to spawn a server, we'll want to make sure to handle auto starting on boot and errors. `systemd` to the rescue.

To use systemd, we need to create a service `/etc/systemd/system/wake-up.service`

```bash

  [Unit]
  Description=Wake up server

  [Service]
  ExecStart=/path/to/node /var/www/wake-up/index.js
  Restart=always
  RestartSec=10
  StandardOutput=syslog
  StandardError=syslog
  SyslogIdentifier=wake-up
  Environment=NODE_ENV=production

  [Install]
  WantedBy=multi-user.target

```

- replace /path/to/node with the output of `whereis node`
- enable on boot with `systemctl enable wake-up.service`
- start the service manually with `systemctl start wake-up.service`

<br>

### Finito

I skipped a lot of details, flaw and features, this guide was mainly to get the creative juices flowing.

- The server should definitely be running on port 443/ssl, you can generate free certificates with [letsencrypt](https://letsencrypt.org/)

- Basic authentication could be used or a secret key as part of the expected `/wake` request body to avoid unknown clients connecting to the server.

- The website could ping the machine to check if it's already on!

For the full code and some extras see https://github.com/ouchadam/wake-up
