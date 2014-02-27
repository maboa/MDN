Media Buffering, Seeking and Time Ranges
========================================

Sometimes it's useful to know how much audio or video has downloaded or is playable without delay. 

Buffered
--------

The ```buffered``` attribute will tell us which parts of the media has been downloaded. It returns a ```TimeRanges``` object which will tell us which chunks of media have been downloaded. This is usually contiguous but if the user jumps about while media is buffering, it may contain holes.

Seekable
--------

The ```seekable``` attribute returns a ```TimeRanges``` object and tells use which parts of the media can be played without delay, this is irrespective of whether that part has been downloaded or not. Some parts of media may be seekable but not buffered if byte-range requests are enabled on the server. Byte-range requests allow parts of the media file to be delivered from the server and so can be ready to play almost immediately - thus seekable.

Consider we have created an audio element, we can access these attributes like so:

`````javascript
var myAudio = document.createElement(audio);

myAudio.src = "music.mp3";

var bufferedTimeRanges = myAudio.buffered;
var seekableTimeRanges = myAudio.seekable;

`````



