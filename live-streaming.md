Live Streaming Web Audio and Video
==================================

Often shortened just to streaming, live streaming is the process of transmitting media 'live' to computers and devices.

We're going to be concentrating on the process of streaming media to the browser.

The key consideration of streaming media to a browser is the fact that rather than playing a finite file we are relaying a file that is being created on the fly, a file that has no pre-determined start or end.

Streaming technology is often employed to relay live events such as sports, concerts and more generally TV and Radio programmes that are output live.


Key Differences Between Streamed and Static Media
-------------------------------------------------

In this case we are using static media to describe media that is represented by a file, whether it be an mp3 or webm file, this file sits on a server and can be delivered like most other files, to the browser. This is often known as VOD or AOD (Video On Demand / Audio On Demand).

Streamed media lacks a finite start and end time as rather than a static file, it is a stream of data that the server passes on down the line to the browser. Usually we require different formats and special server-side software.


Streaming Protocols
-------------------

While static media is usually served over HTTP, there are several streaming protocols, let's take a look at the options.

### HTTP

For now HTTP is by far the most common protocol used to transfer media on demand or live and supported by browsers.

### RTSP

Real Time Streaming Protocol (RTSP) controls media sessions between endpoints and is often used together with Real-time Transport Protocol (RTP) and with Real-time Control Protocol (RTCP) for media stream delivery. Using RTP with RTCP allows for adaptive streaming. Not yet supported natively in the browser.

> Note: some vendors implement propriety transport protocols, such as RealNetworks and their Real Data Transport (RDT)

### RTSP 2.0

In development and not backward compatible with RTSP 1.0

Although the ```<audio>``` and ```<video>``` tags are protocol agnostic, no browser currently supports anything other than HTTP without requiring plugins.

A Quick Note on Adaptive Streaming
----------------------------------

One of the main priorities for live streaming is to keep the player synchronized with the stream, adaptive streaming is a method for doing this in the case of low bandwith. The idea is that the data transfer rate is monitored and if it looks like it's not keeping up we drop down to a lower bandwidth and consequently lower quality stream. In order to this we need to use protocols that facilitate this. 




