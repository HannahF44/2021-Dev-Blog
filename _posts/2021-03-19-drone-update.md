---
title: Drone Update
date: 2012-03-19
---

## The Problem

Trying to make the drone fly through a loop in a different way.

## The Process

The process right now on the project is to make the drone curve through a loop. 
(The code below doesn't belong to me.)

```javascript
const dgram = require("dgram");
const server = dgram.createSocket("udp4");
const port = "8889"
const ipa = "192.168.0.197"
server.on("message", (msg, rinfo) =>
  console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`));


server.send(Buffer.from("command"), 8889, "192.168.0.197");
server.send(Buffer.from("takeoff"), 8889, "192.168.0.197");

setTimeout(
    () => server.send(Buffer.from("cw 90"), 8889, "192.168.0.197"),
    3000
 );
 
 setTimeout(
     () => server.send(Buffer.from("forward 20"), 8889, "192.168.0.197"),
     3000
 );

 setTimeout(
     () => server.send(Buffer.from("land"), 8889, "192.168.0.197"),
     3000
 )
 ```
 
## What I Did 

The codes below show what I've been doing.

* Created where the drone tells us how much time it flies.

```javascript
const dgram = require("dgram");
const server = dgram.createSocket("udp4");

server.on("message", (msg, rinfo) =>
    console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`));

server.send(Buffer.from("time?"), 8889, "192.168.0.197");
```

* Made where it tells us if we're connected to its wifi.

```javascript
const dgram = require("dgram");
const server = dgram.createSocket("udp4");

server.on("message", (msg, rinfo) =>
    console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`));

server.send(Buffer.from("wifi?"), 8889, "192.168.0.197");
```

* Created where the drone tells us the percentage of the battery every few seconds.

```javascript
const dgram = require("dgram");
const server = dgram.createSocket("udp4");

server.on("message", (msg, rinfo) =>
    console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`));

server.send(Buffer.from("battery?"), 8889, "192.168.0.197");
```

* Created where the drone tells us how fast it's going.

```javascript
const dgram = require("dgram");
const server = dgram.createSocket("udp4");

server.on("message", (msg, rinfo) =>
    console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`));

server.send(Buffer.from("speed?"), 8889, "192.168.0.197");
```

## Ah-Ha

What I learned during this process is remind the person who is doing the main coding in the group to show and tellthe other teammates what they're doing.