Live Streaming Web Audio and Video
==================================

Often shortened just to streaming, live streaming is the process of transmitting media 'live' to computers and devices.

We're going to be concentrating on the process of streaming media to the browser.

The key consideration of streaming media to a browser is the fact that rather than playing a finite file we are relaying a file that is being created on the fly, a file that has no pre-determined start or end.

Streaming technology is often employed to relay live events such as sports, concerts and more generally TV and Radio programmes that are output live.


Key Differences Between Streamed and Static Media
-------------------------------------------------

In this case we are using static media to describe media that is represented by a file, whether it be an mp3 or webm file, this file sits on a server and can be delivered like most other files, to the browser. This is often known as VOD or AOD (Video On Demand / Audio On Demand).

Live streamed media lacks a finite start and end time as rather than a static file, it is a stream of data that the server passes on down the line to the browser and is often adaptive. Usually we require different formats and special server-side software.


Adaptive Streaming
------------------

One of the main priorities for live streaming is to keep the player synchronized with the stream, adaptive streaming is a method for doing this in the case of low bandwith. The idea is that the data transfer rate is monitored and if it looks like it's not keeping up, we drop down to a lower bandwidth (and consequently lower quality) stream. In order to have this capability, we need to use formats that facilitate this. Live streaming formats generally allow adaptive streaming by breaking streams into a series of small segments and making those segments available at different qualities and bit rates.


Streaming Audio and Video on Demand
-----------------------------------

Streaming technology is not used exclusively for live streams. It can also be used instead of the traditional progressive download method for Audio and Video on demand:

There are a couple of advantages :

- Latency is generally lower so media will start playing more quickly
- Adaptive streaming allows better experiences on a variety of devices




Streaming Protocols
-------------------

While static media is usually served over HTTP, there are several streaming protocols, let's take a look at the options.

### HTTP

For now HTTP is by far the most common protocol used to transfer media on demand or live and supported by browsers.

### RTMP

Real Time Messaging Protocol is a propriety protocol developed by Macromedia (now Adobe) and supported by the Adobe Flash plugin. RTMP comes in various flavours including RTMPE (Encrypted), RTMPS (Secure over SSL/TLS) and RTMPT (encapuslated within HTTP requests).

### RTSP

Real Time Streaming Protocol (RTSP) controls media sessions between endpoints and is often used together with Real-time Transport Protocol (RTP) and with Real-time Control Protocol (RTCP) for media stream delivery. Using RTP with RTCP allows for adaptive streaming. Not yet supported natively in the browser.

> Note 1: [Firefox OS 1.3 supports RTSP](http://www.mozilla.org/en-US/firefox/os/notes/1.3/). 

> Note 2: some vendors implement propriety transport protocols, such as RealNetworks and their Real Data Transport (RDT)



### RTSP 2.0

In development and not backward compatible with RTSP 1.0

Although the ```<audio>``` and ```<video>``` tags are protocol agnostic, no browser currently supports anything other than HTTP without requiring plugins, although this looks set to change.

> Note: protocols other than HTTP may be subject to blocking from firewalls or proxy servers.


Specifying Various Protocols
----------------------------

The process of specifying the various protocols is reassuringly familiar if you are used to working with media over HTTP.

For example:

`````html
<video src="rtsp://myhost.com/mymedia.format">
 <!-- Fallback here -->
</video>
`````



Media Source Extensions (MSE)
-----------------------------
[Media Source Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/media-source.html) is a W3C working draft that plans to extend HTMLMediaElement to allow JavaScript to generate media streams for playback. Allowing JavaScript to generate streams facilitates a variety of use cases like adaptive streaming and time shifting live streams.

For example, in theory [you could implement MPEG-DASH using JavaScript while offloading the decoding to MSE](http://msopentech.com/blog/2014/01/03/streaming_video_player/).

> Note: Time Shifting is the process of cosnuming a live stream some time after it happened.


Video Streaming File Formats
----------------------------

A couple of HTTP based live streaming video formats are beginning to see support in the browser.

### MPEG-DASH

DASH stands for Dynamic Adaptive Streaming over HTTP and is a new format that has recently been adopted by Chrome and Internet Explorer 11 running on Windows 8.1. It is supported via Media Source Extensions which are used by JavaScript libraries such as [DASH.js](https://github.com/Dash-Industry-Forum/dash.js/) This approach allows us to download chunks of the video stream using XHR and "append" the chunks to the stream that's played by the <video> element. So for example, if we detect that the network is slow, we can start requesting lower quality (smaller) chunks for the next segment. This technology also allows an advertisement segment to be appended/inserted into the stream.

> Note: you can also [use WebM with the MPEG DASH adaptive streaming system] (http://wiki.webmproject.org/adaptive-streaming/webm-dash-specification)

### HLS

HLS or HTTP Live Streaming is a protocol invented by Apple Inc and supported on iOS, Safari and latest versions of Android browser / Chrome. HLS is also adaptive.

HLS can also be decoded using JavaScript which means we can support latest versions of Firefox, Chrome and Internet Explorer 10+. See this [HTTP Live Streaming JavaScript player](https://github.com/RReverser/mpegts).

At the start of the streaming session an [extended M3U (m3u8) playlist](http://en.wikipedia.org/wiki/M3U8#Extended_M3U_directives) is downloaded. This contains the metadata for the various sub-streams that are provided.


Streaming File Format Support
-----------------------------


| Browser                  | DASH   | HLS       | Opus (Audio) |
| ------------------------ | ------ | --------- | ------------ |
| Firefox 32               | ✓ [1]  | ✓ [2]     | ✓ 14+        |
| Safari 6+                |        | ✓         |              | 
| Chrome 24+               | ✓ [1]  | ✓         |              | 
| Opera  20+               | ✓ [1]  |           |              |
| Internet Explorer 10+    | ✓ 11   | ✓ [2]     |              |
| Firefox Mobile           | ✓      | ✓         | ✓            | 
| Safari iOS6+             |        | ✓         |              | 
| Chrome Mobile            | ✓      | ✓ [2]     |              | 
| Opera Mobile             | ✓ [1]  | ✓         |              | 
| Internet Explorer Mobile | ✓ 11   | ✓ [2]     |              | 
| Android                  | ✓      |           |              |

[1] Via JavaScript and MSE

[2] Via JavaScript and a CORS Proxy


Video Fallbacks
---------------

Between DASH and HLS we can cover a significant portion of modern browsers but we still need a fallback if we want to support the rest.

One popular approach is to use a Flash fallback that supports RTMP. Of course we have the issue that we need to encode in three formats.


Audio Streaming File Formats
----------------------------

### Opus

Opus is a royalty free and open format that manages to optimise quality at various bit-rates for different types of audio. Music and speech can be optimised in different ways and Opus uses the SILK and CELT codecs to achieve this.

Currently Opus is supported by Firefox and desktop and mobile as well as the latest versions of desktop Chrome and Opera.

> Note: [Opus is a mandatory format](http://tools.ietf.org/html/draft-ietf-rtcweb-audio-05) for WebRTC browser implementations.

### MP3, AAC, Ogg Vorbis

Most common audio formats can be streamed using specific serverside technologies.

> Note: It's potentially easier to stream audio using non streaming formats because unlike video there are no keyframes.


Serverside Streaming Technologies
---------------------------------

In order to stream live audio and video you will need to run specific streaming software on your server or use third party services.

### GStreamer

GStreamer is an open source cross-platform multimedia framework that allows you to create a variety of media-handling components, including streaming components.

Through it's plugin system GStreamer provides support for more than a hundred codecs (including MPEG-1, MPEG-2, MPEG-4, H.261, H.263, H.264, RealVideo, MP3, WMV, FLV)

GStreamer plugins such as [souphttpclientsink](http://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good-plugins/html/gst-plugins-good-plugins-plugin-soup.html) and [shout2send](http://gstreamer.freedesktop.org/data/doc/gstreamer/head/gst-plugins-good-plugins/html/gst-plugins-good-plugins-plugin-shout2send.html) exist to stream media over HTTP. You can also integrate with Python's Twisted framework or use something like [Flumotion](http://www.flumotion.net/features/) (open source streaming software).

For RTMP transfer you can use the [Nginx RTMP Module](https://github.com/arut/nginx-rtmp-module)


### SHOUTcast

SHOUTcast is a cross-platform proprietary technology for streaming media. Developed by Nullsoft, it allows digital audio content in MP3 or AAC format, to be broadcast. For web use, SHOUTcast streams are transmiited over HTTP.

> Note: [SHOUTcast urls may require a semi-colon to be appended to it](http://stackoverflow.com/questions/2743279/how-could-i-play-a-shoutcast-icecast-stream-using-html5).

### Icecast

The Icecast server is an open source technology for streaming media. Maintained by Xiph.org Foundation it streams Ogg Vorbis/Theora as well as MP3 and AAC format via the SHOUTcast protocol.

> Note: SHOUTcast and Icecast are among the most established and popular technologies, but there are [many more](http://en.wikipedia.org/wiki/List_of_streaming_media_systems#Servers).





Streaming Services
------------------

Although you can install software like GStreamer, SHOUTcast and Icecast you will also find [a lot of third party services](http://en.wikipedia.org/wiki/Comparison_of_streaming_media_systems) that will do much of the work for you.




See Also
--------

- [HTTP Live Streaming](http://en.wikipedia.org/wiki/HTTP_Live_Streaming)
- [HLS Browser Support](http://www.jwplayer.com/html5/hls/)
- [HTTP Live Streaming JavaScript player](https://github.com/RReverser/mpegts)
- [The Basics of HTTP Live Streaming](http://www.larryjordan.biz/app_bin/wordpress/archives/2369)
- [Dynamic Adaptive Streaming over HTTP (MPEG-DASH)](http://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)
- [MPEG-DASH Media Source Demo](http://dash-mse-test.appspot.com/media.html)
- [DASH Reference Client](http://dashif.org/reference/players/javascript/1.0.0/index.html)
- [Dynamic Streaming over HTTP](http://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)
- [The State of MPEG-DASH Deployment](http://www.streamingmediaglobal.com/Articles/Editorial/Featured-Articles/The-State-of-MPEG-DASH-Deployment-96144.aspx)
- [Look, no plugins: Live streaming to the browser using Media Source Extensions and MPEG-DASH](http://www.bbc.co.uk/rd/blog/2014/03/media-source-extensions)
- [Media Source Extensions (W3C)](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/media-source.html)
- [Icecast](http://en.wikipedia.org/wiki/Icecast)
- [SHOUTcast](http://en.wikipedia.org/wiki/Shoutcast)
- [GStreamer](http://en.wikipedia.org/wiki/GStreamer)
- [Streaming GStreamer Piplines Via HTTP](https://coaxion.net/blog/2013/10/streaming-gstreamer-pipelines-via-http/)
- [Streaming media using gstreamer on the web](http://www.svesoftware.com/passkeeper/cms/article/streaming-media-using-gstreamer-web/)
- [GStreamer and Raspberry Pi](http://nginx-rtmp.blogspot.it/2013/07/gstreamer-and-raspberry-pi.html)
- [Acceptance of Media Source Extensions as W3C Candidate Recommendation will accelerate adoption of dash.js](http://msopentech.com/blog/2014/01/09/acceptance-media-source-extensions-w3c-candidate-recommendation-will-accelerate-adoption-dash-js/)
- [Comparison of Streaming Media Systems](http://en.wikipedia.org/wiki/Comparison_of_streaming_media_systems)

