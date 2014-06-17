Adaptive Streaming Setup
========================

Let's say we want to set up adaptive streaming on our server - how would we do that? First we need to decide which protocol to stream so let's pick two of them, that between them will cover most modern browsers.

* MPEG-DASH
* HLS (HTTP Live Streaming)

In order to adaptive stream media we need to split the media up into chunks. We are required to provide several different quality files split up over several time points. The more qualities and time points there are, the more 'adaptive' your stream will be, but we will usually want to find a pragmatic balance between size, time to encode and adaptiveness.

The good news is that once we have encoded our media in the appropriate format we pretty good to go. For adaptive streaming over HTTP no special serverside components are required.

Both MPEG-DASH and HLS use a playlist format to structure the component piece of media that make the possible streams. Various bitrate streams are broken into segments and placed in appropriate server folders - we provide the media players with a link to lookup files or playlists which specify the name and location of these stream folders.


MPEG-DASH Encoding
------------------

One useful piece of software for encoding to MPEG-DASH is [Dash Encoder](https://github.com/slederer/DASHEncoder) 

> *Note 1* You will need to be comfortable with make files and installing dependencies to get it up and running.

> *Note 2* Since MPEG-DASH decoding is done partially using JavaScript and MSE files are often grabbed using XHR which means that same origin rules apply.


Once encoded your file structure may look something like this:

`````
play list ->                /segments/news.mp4.mpd
  
main segment folder ->      /segments/main/

100 Kbps segment folder ->  /segments/main/news100 contains (1.m4s, 2.m4s, 3.m4s ... )

200 Kbps segment folder ->  /segments/main/news200 contains (1.m4s, 2.m4s, 3.m4s ... )

300 Kbps segment folder ->  /segments/main/news300 contains (1.m4s, 2.m4s, 3.m4s ... )

400 Kbps segment folder ->  /segments/main/news400 contains (1.m4s, 2.m4s, 3.m4s ... )
`````

The playlist or .mpd file contains XML that explicitly lists where all the various bitrate files reside.

`````xml
<MPD xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="urn:mpeg:DASH:schema:MPD:2011" xsi:schemaLocation="urn:mpeg:DASH:schema:MPD:2011" profiles="urn:mpeg:dash:profile:isoff-main:2011" type="static" mediaPresentationDuration="PT0H9M56.46S" minBufferTime="PT1.0S">
<BaseURL>
http://www-itec.uni-klu.ac.at/ftp/datasets/mmsys12/BigBuckBunny/bunny_1s/
</BaseURL>
<Period start="PT0S">
<AdaptationSet bitstreamSwitching="true">
<Representation id="0" codecs="avc1" mimeType="video/mp4" width="320" height="240" startWithSAP="1" bandwidth="46986">
<SegmentBase>
<Initialization sourceURL="bunny_1s_50kbit/bunny_50kbit_dashNonSeg.mp4" range="0-862"/>
</SegmentBase>
<SegmentList duration="1">
<SegmentURL media="bunny_1s_50kbit/bunny_50kbit_dashNonSeg.mp4" mediaRange="863-7113"/>
<SegmentURL media="bunny_1s_50kbit/bunny_50kbit_dashNonSeg.mp4" mediaRange="7114-14104"/>
<SegmentURL media="bunny_1s_50kbit/bunny_50kbit_dashNonSeg.mp4" mediaRange="14105-17990"/>

...


`````

[XML EXPLANATION GOES HERE]


See Also
--------

- [ITEC – Dynamic Adaptive Streaming over HTTP](http://www-itec.uni-klu.ac.at/dash/?page_id=207)
- [DASHEncoder](https://github.com/slederer/DASHEncoder)
- [Dynamic Adaptive Streaming over HTTP Dataset](http://www-itec.uni-klu.ac.at/bib/files/p89-lederer.pdf)
- [Adaptive Streaming in the Field](http://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Adaptive-Streaming-in-the-Field-73017.aspx)
- [MPEG DASH Media Source Demo](https://dash-mse-test.appspot.com/media.html)
- [HTTP Live Streaming](https://developer.apple.com/streaming/)
- [MPEG-DASH and streaming reference and resources (MSDN)](http://msdn.microsoft.com/en-us/library/dn551370(v=vs.85).aspx)



