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

Concurrent audio play-back is tested using [this example](http://jsbin.com/visihopa) where we attempt to play three pieces of audio at the same time using the standard audio API. 

Simple autoplay functionality is tested with [this example](http://jsbin.com/rufinihi).


| Mobile Browser    | Version | Concurrent | Autoplay |
| ----------------- | ------- | ---------- | -------- |
| Chrome (Android)  | 32+     |  N         |   N      |
| Firefox (Android) | 26+     |  Y         |   Y      |
| IE Mobile         | 10+     |  ?         |          |
| Opera Mobile      | 11+     |  N         |   Y      |
| Safari (iOS)      | 7+      | Y/N [1]    |   N [2]  |
| Android Browser   | 2.3+    |  ?         |          |

> **Note 1** : Safari has issues playing if you try and start all pieces of audio contempororarily. If you stagger playback you may have limited success.

> **Note 2** : It has been reported to work on Safari iOS6 if you add your page to home screen.


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

[Try it out](https://thimble.webmaker.org/project/87621/remix)


> **Note 1** : On mobile we may need to trigger this code from a user initiated event. Perhaps the 'start' button being pressed? 

> **Note 2** : Watch out for bitrates. Encoding your audio at lower bitrates means smaller file sizes but lower seeking accuracy.


See Also
--------

- [Making HTML5 Audio Actually Work on Mobile](http://pupunzi.open-lab.com/2013/03/13/making-html5-audio-actually-work-on-mobile/)
- [Audio Sprites (and fixes for iOS)](http://remysharp.com/2010/12/23/audio-sprites/)
