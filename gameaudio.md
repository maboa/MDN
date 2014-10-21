Audio for Web Games
===================

Audio is an important part of any game - it adds feedback and atmosphere. Web based audio is maturing fast, but there are still many browser differences to negotiate.

We often need to prioritise the audio parts that are essential for our games, which are nice to have and devise a strategy based on those decisions.

Mobile
------

By far the most difficult platforms to provide support for are mobile. Unfortunately these are also the platforms that people often use to play games. There's a couple of differences between desktop and mobile browsers that may have caused broswer vendors to make choices that then make mobile difficult for games audio developers to work with.



### Autoplay

Many mobile browsers will simply ignore any instructions to autoplay audio. Playback for audio needs to be started by a user initiated event. This means you will have to structure your audio playback to take account of that. This is usually mitigated against by loading the audio in advance and starting any -- say -- background music in advance so that playback occurs when a start button is pressed. 

For more passive audio autoplay, for example background music that starting as soon as a game loads, one trick is to detect any user initiated event and play back at that point.

> Note - adding a web app to your mobile home screen may change its capabilities. In the case of autoplay and iOS, this appears to be the case currently.



### Volume

Volume is also one of those properties that is often disabled by mobile browsers. The rationale is often that the user should be in control of the volume and this shouldn't be overriden.

### Buffering

Likely an attempt to mitigate runaway mobile network data use, we may also find that buffering is disabled. Buffering is the process of the browser downloading the media in advance, which we often need to do to ensure smooth play back.

### Concurrent Audio Playback

A requirement of many games is to play more than one piece of audio at the same time. Although the situation is set to get better with the adoption of the Web Audio API. Currently using the vanilla audio element will result in patchy results on mobile.

### Testing and Support

Concurrent audio play-back is tested using [this example](http://jsfiddle.net/aqyvgcz2/) where we attempt to play three pieces of audio at the same time using the standard audio API. 

Simple autoplay functionality is tested with [this example](http://jsfiddle.net/4qra96gc/).


| Mobile Browser    | Version | Concurrent | Autoplay |
| ----------------- | ------- | ---------- | -------- |
| Chrome (Android)  | 32+     |  N         |   N      |
| Firefox (Android) | 26+     |  Y         |   Y      |
| IE Mobile         | 10+     |  ?         |   ?      |
| Opera Mobile      | 11+     |  N         |   Y      |
| Safari (iOS)      | 7+      | Y/N [*]    |   N      |
| Android Browser   | 2.3+    |  N         |   N      |

> **Note** : Safari has issues playing if you try and start all pieces of audio contempororarily. If you stagger playback you may have limited success.


Mobile Workarounds
------------------

Although mobile browsers can present problems, there are ways to work around these issues.

### Audio Sprites

Audio sprites take their name from CSS sprites - a visual technique for using CSS with a single graphic resource to break it into a series of sprites. We can apply the same principle to audio so that rather than have a series of small audio files that take time to load and play, we have one larger audio file containing all the smaller audio snippets we need. To play those audio parts we just use established start and stop times.

The advantage is that we can prime one piece of audio and have our sprites ready to go. To do this we can just play and instantly pause the larger piece of audio. You'll also reduce the number of server requests and save bandwidth.

`````javascript
var myAudio = createElement("audio");
myAudio.src = "mysprite.mp3";
myAudio.play();
myAudio.pause();

`````

You'll need to sample the current time to know when to stop. If you space your individual sounds by at least 500ms then using the timeUpdate event which fires every 250ms should be sufficient. Your files may be slightly longer than they strictly need to be, but silence compresses well.

Here's an example of an audio sprite player - first let's set up the user interface in HTML:

`````html
<audio id="myAudio" src="jPlayer.org/tmp/countdown.mp3"></audio>
<button type="button" data-start="18" data-stop="19">0</button>
<button type="button" data-start="16" data-stop="17">1</button>
<button type="button" data-start="14" data-stop="15">2</button>
<button type="button" data-start="12" data-stop="13">3</button>
<button type="button" data-start="10" data-stop="11">4</button>
<button type="button" data-start="8"  data-stop="9">5</button>
<button type="button" data-start="6"  data-stop="7">6</button>
<button type="button" data-start="4"  data-stop="5">7</button>
<button type="button" data-start="2"  data-stop="3">8</button>
<button type="button" data-start="0"  data-stop="1">9</button>
`````

Now we have buttons with start and stop time in seconds. The countdown MP3 file consists of a number being spoken every 2 seconds, the idea is that we play back that number when the corresponding button is pressed.

Let's add some JavaScript to make this work:

`````javascript
var myAudio = document.getElementById('myAudio');
var buttons = document.getElementsByTagName('button');
var stopTime = 0;

for (var i = 0; i < buttons.length; i++) {
  buttons[i].onclick = function() {
    myAudio.currentTime = this.getAttribute("data-start");
    stopTime = this.getAttribute("data-stop");
    myAudio.play();
  };
}

myAudio.addEventListener('timeupdate', function() {
  if (this.currentTime > stopTime) {
    this.pause();
  }
}, false);
`````

[Try it out](http://jsfiddle.net/59vwaame/)


> **Note 1** : On mobile we may need to trigger this code from a user initiated event. Perhaps the 'start' button being pressed? 

> **Note 2** : Watch out for bitrates. Encoding your audio at lower bitrates means smaller file sizes but lower seeking accuracy.

Music
-----
Music in games can have a powerful emotional effect. You can mix and match various music samples and assuming you can control the volume of your audio element you could cross-fade different musical pieces. Using the [playbackRate](https://developer.mozilla.org/en-US/Apps/Build/Audio_and_video_delivery/HTML5_playbackRate_explained) method you can even adjust the speed of your music without affecting the pitch.

All this is possible using the standard audio element and associated API but becomes much easier with the more advanced Web Audio API.


Web Audio API for Games
-----------------------

Now supported in all modern browsers bar Internet Explorer and Opera Mini an acceptable approach for many is to use the Web Audio API. The Web Audio API is an advanced audio JavaScript API that is ideally suited to gaming. Developers can generate audio and manipulating audio samples as well as position sound in 3D game space.

A feasible strategy would be to provide basic audio using the standard audio element and enhance the experience using the Web Audio API where supported.

Significantly iOS Safari now supports the Web Audio API, which means it's now possible to write web-based games with native-quality audio on these platforms.

As the Web Audio API allows precise timing and control of audio play-back we can use it to play samples at specific moments which is a crucial immersive aspect of gaming.


Background Music with the Web Audio API
---------------------------------------

Although we can use the audio element to deliver linear background music that doesn't change in reaction to your game environment, the Web Audio API is ideal for implementing a more dynamic musical experience. You may want music to change depending on whether you are trying to build suspense or encourage the player in some way. Music is an important part of the gaming experience and depending on the type of game you are making you may wish to invest significant effort into getting it right.

One way you can make your soundtrack more dynamic is by splitting it up into component loops or tracks. This often the way that musicians compose music anyway and the Web Audio API is extremely good at keeping these parts in sync. Once you have the various tracks that make up your piece you can bring tracks in and out as appropriate.

We can also apply filters or effects to music. Is your character in a cave? Increase the echo. Maybe you have underwater scenes - apply a filter that muffles the sound.

Let's look at the methods of dynamically adjusting music from it's base tracks.

### Loading Your Tracks

With the Web Audio API you can load separate tracks and loops individually using XHR, which means you can load them synchronously or in parallel. Loading synchronously might mean parts of your music are ready earlier and you can start playing them, while others load.

Either way you may want to synchronise tracks or loops. The Web Audio API contains the notion of an internal clock that starts ticking the moment you create an audio context. You'll need to take account of the time between creating an audio context and when the first audio track starts playing. Recording this offset and querying the playing track's current time give you enough information to synchronise separate pieces of audio.

To see this in action, let's lay out some separate tracks:

`````html
<ul>
  <li><a class="track" href="http://jPlayer.org/audio/mp3/gbreggae-leadguitar.mp3">Lead Guitar</a></li>
  <li><a class="track" href="http://jPlayer.org/audio/mp3/gbreggae-drums.mp3">Drums</a></li>
  <li><a class="track" href="http://jPlayer.org/audio/mp3/gbreggae-bassguitar.mp3">Bass Guitar</a></li>
  <li><a class="track" href="http://jPlayer.org/audio/mp3/gbreggae-horns.mp3">Horns</a></li>
  <li><a class="track" href="http://jPlayer.org/audio/mp3/gbreggae-clav.mp3">Clavi</a></li>
</ul>

`````
All of these tracks are the same tempo and are designed to be synchronised with each other.

`````javascript
window.AudioContext = window.AudioContext || window.webkitAudioContext;

var offset = 0;
var context = new AudioContext();

function playTrack(url) {
  var request = new XMLHttpRequest();
  request.open('GET', url, true);
  request.responseType = 'arraybuffer';

  var audiobuffer;

  // Decode asynchronously
  request.onload = function() {
    
    if (request.status == 200) {
      
      context.decodeAudioData(request.response, function(buffer) {
        var source = context.createBufferSource();
        source.buffer = buffer;
        source.connect(context.destination);
        console.log('context.currentTime '+context.currentTime);

        if (offset == 0) {
          source.start();
          offset = context.currentTime;
        } else {
          source.start(0,context.currentTime - offset);
        }

      }, function(e) {
        console.log('Error decoding audio data:' + e);
      });
    } else {
      console.log('Audio didn\'t load successfully; error code:' + request.statusText);
    }
  }
  request.send();
}

var tracks = document.getElementsByClassName('track');

for (var i = 0, len = tracks.length; i < len; i++) {
  tracks[i].addEventListener('click', function(e){

    playTrack(this.href);
    e.preventDefault();
  });
}

`````

Here we set up a new AudioContext and create a function (playTrack) that loads and starts playing a track.

> Note - start (formerly known as noteOn) will start playing an audio asset. Start takes three (optional) parameters:
1. when (the absolute time to commence playback) 2. where (the part of the audio to start playing from) and 3. how long (duration). Stop takes one optional parameter - when (delay before stopping).

If the second parameter -- the offset -- is zero we start playing from the start of the given piece of audio, which is what we do in the first instance. We then store the AudioContext currentTime offset of when the first piece began playing and subtract that from any subsequent currentTimes to calculate the actual time and use that to synchronise our tracks.

In the context of your game world you may have loops and samples that are played depending on circumstance and it can be useful to be able to synchronise with other tracks for a more seamless experience.

> Note that this example does not wait for the beat end, we could do this if we knew the BPM (Beats Per Minute) of the tracks.


See Also
--------

- [Making HTML5 Audio Actually Work on Mobile](http://pupunzi.open-lab.com/2013/03/13/making-html5-audio-actually-work-on-mobile/)
- [Audio Sprites (and fixes for iOS)](http://remysharp.com/2010/12/23/audio-sprites/)
- [Developing Game Audio with the Web Audio API (HTML5Rocks)](http://www.html5rocks.com/en/tutorials/webaudio/games/)
