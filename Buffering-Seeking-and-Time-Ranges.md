Media Buffering, Seeking and Time Ranges
========================================

Sometimes it's useful to know how much audio or video has downloaded or is playable without delay. 

Buffered
--------

The ```buffered``` attribute will tell us which parts of the media has been downloaded. It returns a ```TimeRanges``` object which will tell us which chunks of media have been downloaded. This is usually contiguous but if the user jumps about while media is buffering, it may contain holes.

Seekable
--------

The ```seekable``` attribute returns a ```TimeRanges``` object and tells use which parts of the media can be played without delay, this is irrespective of whether that part has been downloaded or not. Some parts of media may be seekable but not buffered if byte-range requests are enabled on the server. Byte-range requests allow parts of the media file to be delivered from the server and so can be ready to play almost immediately - thus seekable.

Consider we have created an audio element, 

`````html
<audio id="my-audio" controls src="music.mp3">
</audio>

`````

we can access these attributes like so:

`````javascript
var myAudio = document.getElementById('my-audio');


var bufferedTimeRanges = myAudio.buffered;
var seekableTimeRanges = myAudio.seekable;

`````

TimeRanges Object
-----------------

TimeRanges are a series of non-overlapping ranges of time with start and stop times. More about [TimeRanges](https://developer.mozilla.org/en-US/docs/Web/API/TimeRanges)

A TimeRanges Object consists of the following properties:

- ```length``` - number of time ranges
- ```start(index)``` - start time in seconds of a time range
- ```end(index)``` - end time in seconds of a time range

Without any user interaction there is usually only one time range, but conceivably if you jump about in the media, more than one time range can appear.


It may help to visualize them:

------------------------------------------------------
| =========== |                    | ========= |     |






