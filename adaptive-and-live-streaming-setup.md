Adaptive Streaming Setup
========================

Let's say we want to set up adaptive streaming on our server - how would we do that? First we need to decide which protocol to stream so let's pick two of them, that between them will cover most modern browsers.

* MPEG-DASH
* HLS (HTTP Live Streaming)

In order to adaptive stream media we need to split the media up into chunks. We are required to provide several different quality files split up over several time points. The more qualities and time points there are, the more 'adaptive' your stream will be, but we will usually want to find a pragmatic balance between size, time to encode and adaptiveness.

The good news is that once we have encoded our media in the appropriate format we pretty good to go. For adaptive streaming over HTTP no special serverside components are required.



MPEG-DASH Encoding
------------------

One useful piece of software for encoding to MPEG-DASH is [Dash Encoder](https://github.com/slederer/DASHEncoder) 

> Please note that you will need to be comfortable with make files and installing dependencies to get it up and running.

Once encoded your file structure should look something like this:



See Also
--------

- [ITEC – Dynamic Adaptive Streaming over HTTP](http://www-itec.uni-klu.ac.at/dash/?page_id=207)
- [DASHEncoder](https://github.com/slederer/DASHEncoder)
- [Dynamic Adaptive Streaming over HTTP Dataset](http://www-itec.uni-klu.ac.at/bib/files/p89-lederer.pdf)
- [Adaptive Streaming in the Field](http://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Adaptive-Streaming-in-the-Field-73017.aspx)
- [HTTP Live Streaming](https://developer.apple.com/streaming/)



