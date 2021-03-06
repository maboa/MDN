Media Buffering, Seeking and Time Ranges
========================================

Sometimes it's useful to know how much audio or video has downloaded or is playable without delay - a good example of this is the buffered progress bar of an audio or video player.

Buffered
--------

The ```buffered``` attribute will tell us which parts of the media has been downloaded. It returns a ```TimeRanges``` object which will tell us which chunks of media have been downloaded. This is usually contiguous but if the user jumps about while media is buffering, it may contain holes.


This will work with video too but consider we have created an audio element, 

`````html
<audio id="my-audio" controls src="music.mp3">
</audio>

`````

we can access these attributes like so:

`````javascript
var myAudio = document.getElementById('my-audio');

var bufferedTimeRanges = myAudio.buffered;

`````

TimeRanges Object
-----------------

TimeRanges are a series of non-overlapping ranges of time, with start and stop times. More about [TimeRanges](https://developer.mozilla.org/en-US/docs/Web/API/TimeRanges)

A TimeRanges Object consists of the following properties:

- ```length``` - number of time ranges
- ```start(index)``` - start time in seconds of a time range
- ```end(index)``` - end time in seconds of a time range

Without any user interaction there is usually only one time range, but if you jump about in the media, more than one time range can appear.


It may help to visualize them:

`````
------------------------------------------------------
|=============|                    |===========|     |
------------------------------------------------------
0             5                    15          19    21
`````

Imagine the above diagram represents two buffered time ranges - one spanning 0 to 5 seconds and the second spanning 15 to 19 seconds.

`````javascript

myAudio.buffered.length;   // returns 2
myAudio.buffered.start(0); // returns 0
myAudio.buffered.end(0);   // returns 5
myAudio.buffered.start(1); // returns 15
myAudio.buffered.end(1);   // returns 19

`````

To try out and visualize buffered ```TimeRanges``` we can write a little bit of HTML:

`````HTML
<p>
  <audio id="my-audio" controls>
    <source src="music.mp3" type="audio/mpeg">
  </audio>
</p>
<p>
  <canvas id="my-canvas" width="300" height="20">
  </canvas>
</p>

`````

and a little bit of JavaScript:

````` javascript
  window.onload = function(){ 

    var myAudio = document.getElementById('my-audio');
    var myCanvas = document.getElementById('my-canvas');
    var context = myCanvas.getContext('2d');

    context.fillStyle = 'lightgray';
    context.fillRect(0, 0, myCanvas.width, 20);
    context.fillStyle = 'red';
    context.strokeStyle = 'white';
    
    var inc = myCanvas.width / myAudio.duration;

    // display TimeRanges
    
    myAudio.addEventListener('seeked', function() {
      for (i = 0; i < myAudio.buffered.length; i++) {

        var startX = myAudio.buffered.start(i) * inc;
        var endX = myAudio.buffered.end(i) * inc;

        context.fillRect(startX, 0, endX, 20);
        context.rect(startX, 0, endX, 20);
        context.stroke();
      }
    });
  }

`````

This works better with longer pieces of audio or video, but press play and click about the player progress bar and you should get something like this. Each red filled white rectangle represents a time range.

![buffered TimeRanges displayed](https://raw.github.com/maboa/MDN/master/images/bufferedtimeranges.png)


Seekable
--------

The ```seekable``` attribute returns a ```TimeRanges``` object and tells use which parts of the media can be played without delay, this is irrespective of whether that part has been downloaded or not. Some parts of media may be seekable but not buffered if byte-range requests are enabled on the server. Byte-range requests allow parts of the media file to be delivered from the server and so can be ready to play almost immediately - thus seekable.

`````javascript
var seekableTimeRanges = myAudio.seekable;
`````

Creating our own Buffering Feedback
-----------------------------------

If we wish to create our own custom player, we may want to provide feedback on how much of the media is ready to be played. In practice a good way to do this is use the seekable attribute. Although as we have seen above, seekable parts of the media are not neccessarily contiguous, they often are and we can safely approximate this information to give the user an indication of which parts of the media can be played directly. We can find this point in the media using the following line of code:

`````javascript
var seekableEnd = myAudio.seekable.end(myAudio.seekable.length - 1);
`````

Note : ```myAudio.seekable.end(myAudio.seekable.length - 1)``` actually tells us the end point of the last time range that is seekable (not all seekable media). In practice this is good enough, as the browser either enables range requests or it doesn't. If it doesn't then ```audio.seekable``` will be equivalent to ```audio.buffered``` which will give a valid indication of the end of seekable media.

However if range requests are enabled this value usually becomes the duration of the media almost instantly.

Better perhaps to give an indication of how much media has actually downloaded, this what the browser's native players seem to display.

So let's build this:

`````css
.buffered { 
  height: 20px; 
  position: relative;
  background: #555;
  width: 300px;
}

#buffered-amount {
  display: block;
  height: 100%;
  background-color: #777;
  width: 0;
}

.progress { 
  margin-top: -20px;
  height: 20px;  
  position: relative;
  width: 300px;
}

#progress-amount {
  display: block;
  height: 100%;
  background-color: #595;
  width: 0;
}
`````

HTML :

`````html
<audio id="my-audio" preload controls>
  <source src="music.mp3" type="audio/mpeg">
</audio>
<div class="buffered">
  <span id="buffered-amount"></span>
</div>
<div class="progress">
  <span id="progress-amount"></span>
</div>
`````

and the JavaScript :

`````javascript
window.onload = function(){ 

  var myAudio = document.getElementById('my-audio');

  myAudio.addEventListener('progress', function() {
    var bufferedEnd = myAudio.buffered.end(myAudio.buffered.length - 1);
    var duration =  myAudio.duration;
    if (duration > 0) {
      document.getElementById('buffered-amount').style.width = ((bufferedEnd / duration)*100) + "%";
    }
  });

  myAudio.addEventListener('timeupdate', function() {
    var duration =  myAudio.duration;
    if (duration > 0) {
      document.getElementById('progress-amount').style.width = ((myAudio.currentTime / duration)*100) + "%";
    }
  });
}
`````

this should give you results similar to this, where the lighter grey represents the buffered progress and green the played progress:

![buffered progress displayed](https://raw.github.com/maboa/MDN/master/images/bufferedprogress.png)


A quick word about Played
-------------------------

It’s worth mentioning the played property. ```played``` tells us which time ranges have been played within the media. For example:

`````javascript
var played = audio.played; // returns a TimeRanges object
`````

This could be useful for analysing what parts of your media is being watched or listened to.




