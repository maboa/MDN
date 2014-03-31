Playback Rate
=============

```playbackRate``` allows us to change the speed or rate that a piece of web audio or video is playing.

Here's an example:

`````javascript
var myAudio = document.createElement('audio');
myAudio.setAttribute('src','audiofile.mp3');
myAudio.playbackRate = 0.5;
`````

Here we create an ```audio``` element and set the ```src``` to a file of our choice. Next we set the ```playbackRate``` to 0.5 which represents half normal speed. The ```playbackRate``` is a multiple applied to the original rate.


> *Note 1* : Most browsers stop playing audio below a playbackRate of 0.5 and above 4. Video will continue to play silently however. So it's recommended for most applications that you limit the range to between 0.5 and 4.

> *Note 2* : Interestingly the tone or the pitch of the audio track does not change, just the speed.

> *Note 3* : Negative values don't currently play the media backwards.


A Video Example
---------------

First let's set up our video and playback rate controls in HTML :

`````html
<video id="myVideo" controls>
  <source src="http://jplayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v" type='video/mp4' />
  <source src="http://jplayer.org/video/webm/Big_Buck_Bunny_Trailer.webm" type='video/webm' />
</video>

<form>
  <input id="pbr" type="range" value="1" min="0.5" max="4" step="0.1" >
  <p>Playback Rate <span id="currentPbr">1</span></p>
</form>
`````

Now we can attach some JavaScript to it :

`````javascript
window.onload = function () {
 
  var v = document.getElementById("myVideo");
  var p = document.getElementById("pbr");
  var c = document.getElementById("currentPbr");

  p.addEventListener('input',function(){
    c.innerHTML = p.value;
    v.playbackRate = p.value;
  },false);

};
`````
We listen to the ```input``` changing which allows us to react to the playback rate control being changed.


defaultPlaybackRate and ratechange
----------------------------------

In addition to playbackRate we also have ```defaultPlaybackRate``` which lets us set the default playback rate - the rate at which that the media resets to in the case that we change something like the source, or in some browsers when an ended event is generated. 

We would usually set ```defaultPlaybackRate``` before playing the media while playbackRate allows us to change it during video play-back.

We also have a new event called ```ratechange``` which fires every time the playbackRate changes. 


Limitations and Oddities
------------------------

Currently there are a few limitations and browser oddities to be aware of. 

* Most browsers stop playing audio below a playbackRate of 0.5 and above 4 - however, video will continue to play silently in these cases. 
* IE9+ will switch to the default playback rate when an ```ended``` event is encountered.
* Firefox generates a ratechange event when the media source is substituted.
* iOS 7 you can only affect the ```playbackRate``` when the media is paused (not while it's playing).


Support
-------

Chrome 20+ 	✔
Firefox 20+ 	✔
IE 9+ 	✔
Safari 6+ 	✔
Opera 15+ 	✔
Mobile Chrome (Android) 	✖
Mobile Firefox 24+ 	✔
IE Mobile 	✖
Mobile Safari 6+ (iOS) 	✔
Opera Mobile 	✖

See Also:

(Playback Rate Test)[http://hyperaud.io/lab/pbr-test/] 
