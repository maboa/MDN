Web Audio Basics
================

Basic Audio
-----------

`````html
<audio controls>
  <source src="audiofile.mp3" type="audio/mpeg">
  <source src="audiofile.ogg" type="audio/ogg">
  <!-- fallback for non supporting browsers goes here -->
  Your browser does not support the audio element.
</audio>
`````

Here we define an audio element with multiple sources, we do this as not all browsers support the same audio formats and so for reasonable coverage we should specify at least two. The two formats that will give you maximum coverage are mp3 and ogg vorbis.

We do this using the ``<source>`` tag which takes attributes ``src`` and ``type``.

Similarly to its use within the ``<img>`` tag, ``src`` contains a reference to the file to be loaded.
``type`` is used to inform the browser of the file type. If omitted, most browsers will attempt to guess this from the file extension.

If the ``<audio>`` tag is not supported then ``<audio>`` and ``<source>`` will be ignored. However any supported text or elements that you define within ``<audio>`` will be displayed or acted upon. So the ideal place to create a fallback or inform of incompatibility is before the closing ``</audio>``.

``controls`` is an attribute that we specify when we require the browser to provide us with a play-back controls. 

> *Note* these are the browser's controls which can evolve and will look a little different on each browser.

Support
-------

### Desktop

| Desktop Browser   | Version |
| ----------------- | ------- |
| Chrome            | 4 +     |
| Firefox           | 3.5 +   |
| Internet Explorer | 9 +     |
| Opera             | 10.5 +  |
| Safari            | 4 +     |


### Mobile

| Mobile Browser    | Version |
| ----------------- | ------- |
| Chrome (Android)  | 32 +    |
| Firefox (Android) | 26 +    |
| IE Mobile         | 10 +    |
| Opera Mobile      | 11 +    |
| Safari (iOS)      | 4 +     |
| Android Browser   | 2.3 +   |
| Blackberry        | 7 +     |


Priming Audio with HTML Attributes
----------------------------------

We can specify a number of attributes with the audio tag to further determine the way audio is initialized.

### autoplay

Specifying ``autoplay`` will mean that the audio will start playing as soon as possible and without any user interaction - in short, the audio will auto-play.

`````html
<audio autoplay>
  ...
</audio>
`````
> Note - this value is often ignored on mobile platforms.

### loop

The ``loop`` attribute will ensure that upon getting to the end, the audio will start again from the beginning - in short, once played the audio will loop.

`````html
<audio loop>
  ...
</audio>
`````

### muted

If you want the audio to start muted (no volume), add the ``muted`` attribute.

`````html
<audio muted>
  ...
</audio>
`````

> Note - this value is often ignored on mobile platforms.

### preload

The ```preload``` attribute allows you to specify a preference for how the browser preloads the audio, in other words which part of the file it downloads when the audio element is initialized.

```preload``` can take 3 values:

1. ```preload = "none"```
2. ```preload = "meta"```
3. ```preload = "auto"```

> Note - this value is often ignored on mobile platforms.

`````html
<audio preload="auto">
  ...
</audio>
`````

### controls

Mentioned above, ``controls`` is an attribute that we specify when we require the browser to provide us with a play-back controls.

`````html
<audio controls>
  ...
</audio>
`````

### src

Mentioned above, you can use the ```<source>``` element to specify one or more source audio files. Alternatively you can use the src attribute directly within the ```<audio>``` tag to specify a single source file.

`````html
<audio src="audiofile.mp3">
  ...
</audio>
`````

### type

Mentioned above, to be sure that the browser knows what type of file is being specified, it's good practice to specify a type with the src attribute. The type attribute specifies the MIME type or Internet Media Type of the file.

`````html
<audio src="audiofile.mp3" type="audio/mpeg">
  ...
</audio>
`````


Fallbacks
---------

Although the vast majority of browsers now support the audio element, you may want to cater for those that don't. Usually these browsers are catered for by using fallbacks written for plugins such as Adobe Flash or Microsoft Silverlight.

> Note - you should be aware that Flash and Silverlight code require that the user has the appropriate plugin installed and significantly that the *browser cannot guarantee the security* aspects of code running on those plugin platforms.



Manipulating the Audio Element with JavaScript
----------------------------------------------

In addition to being able to specify various attributes in HTML, the audio element comes complete with several properties that you can manipulate via JavaScript.

Given the following HTML:

`````html
<audio id="my-audio" src="audiofile.mp3">
  ...
</audio>
`````

you can grab the audio element like this:

`````javascript
var myAudio = document.getElementById('my-audio');
`````
Alternatively you can create a new one. Here's an example of creating an audio element, setting the media, playing, pausing and playing from a certain time:

`````javascript
var myAudio = document.createElement('audio');

if (myAudio.canPlayType('audio/mpeg')) {
  myAudio.setAttribute('src','audiofile.mp3');
}

if (myAudio.canPlayType('audio/ogg')) {
  myAudio.setAttribute('src','audiofile.ogg');
}

alert('play');

myAudio.play();

alert('stop');

myAudio.pause();

alert('play from 5 seconds in');

myAudio.currentTime = 5; 
myAudio.play();

`````


### play

The ```play``` method is used to tell the audio to play. It takes no parameters.

`````javascript
myAudio.play();
`````

### pause

The ```pause``` method is used to tell the audio to pause. It takes no parameters.

`````javascript
myAudio.pause();
`````
> Note - there is no stop method.

### canPlayType

```canPlayType``` asks the browser whether a certain audio file type is supported. 

It takes an Internet Media Type as a parameter and returns one of three values:

1. "probably"
2. "maybe"
3. "" (the empty string).


`````javascript
if (myAudio.canPlayType('audio/mpeg')) {
  // It's supported.
  // Do something here!
}
`````

In practice, we usually check if the result is truthy or falsey. Non empty strings are truthy.

> Note - very early specs specified that the browser should return "no" instead of empty string, thankfully the number of people using these browsers are few and far between.

### currentTime

```play``` does not take a parameter so we need to set the point to play from separately using the ```currentTime``` property. If not set the default is zero.

We can get ```currentTime``` as well as set it. The value is a number which represents the time in seconds.

`````javascript
if (myAudio.currentTime > 5) {
  myAudio.currentTime = 3;
}
`````

### volume

The ```volume``` property allows us to set the audio volume. You specify a number between 0 and 1.

`````javascript
// set the volume at 50%
myAudio.volume = 0.5;

`````

Creating your own Audio Player
------------------------------

The JavaScript API allows you to create your own custom player. Let's take a look at a very minimal example. We can combine HTML and JavaScript to create a very simple player with a play and a pause button.

We can set up the audio in the HTML, without the controls attribute, since we create our own controls.

`````html
<audio id="my-audio">
  <source src="audiofile.mp3" type="audio/mpeg">
  <source src="audiofile.ogg" type="audio/ogg">
  <!-- place fallback here as <audio> supporting browsers will ignore it -->
  <a href="audiofile.mp3">audiofile.mp3</a>
</audio>

<!-- we create our play and pause button next -->
<a id="play" href="#">play</a>
<a id="pause" href="#">pause</a>
`````

Next we attach some functionality to the player using JavaScript:

`````javascript
window.onload = function(){ 

  var myAudio = document.getElementById('my-audio');
  var play = document.getElementById('play');
  var pause = document.getElementById('pause');

  // associate functions with the 'onclick' events
  play.onclick = playAudio;
  pause.onclick = pauseAudio;

  function playAudio() {
    myAudio.play();
  }

  function pauseAudio() { 
   myAudio.pause();
  }

}

`````



Media Loading Events
--------------------

Above we have shown how you can create a very simple audio player, but what if we want to show progress, buffering and only activate the buttons when the media is ready to play? Fortunately there are a whole bunch of events we can use to let our player know exactly what is happening.

First let's take a look at the media loading process in order:

### loadstart
The ```loadstart``` event tell us simply that load process has started and the browser is connecting to the media.
`````javascript
myAudio.addEventListener("loadstart", function() {
  //grabbing the file
});
`````

### durationchange
If you just want to know as soon as the duration of your media is established, this is the event for you. This can be useful because the initial value for duration is ```NaN``` (Not a Number) which you probably don't want to display.

`````javascript
myAudio.addEventListener("durationchange", function() {
  //you can display the duration now
});
`````

### loadedmetadata
Metadata can consist of more than just duration, if you want to wait for all the metadata to download before doing something, you can detect the ```loadedmetadata``` event.

`````javascript
myAudio.addEventListener("loadedmetadata", function() {
  //you can display the duration now
});
`````

### loaded
The ```loaded``` event is fired when the first bit of media arrives. The playhead is in position but not quite ready to play.

`````javascript
myAudio.addEventListener("loaded", function() {
  //you could display the playhead now
});
`````

### progress
The ```progress``` event indicates that the download of media is still in progress. It may be good practice to display some kind of 'loader' at this point.

`````javascript
myAudio.addEventListener("progress", function() {
  // you could let the user know the media is downloading
});
`````

### canplay

```canplay``` is a useful event to detect should you want to determine whether the media ia ready to play. You could for example disable custom controls until this event occurs.

`````javascript
myAudio.addEventListener("canplay", function() {
  //audio is ready to play
});
`````

### canplaythrough

```canplaythrough``` is similar to ```canplay``` but it lets you know that the media is ready to be played all the way through (that is to say that the file has completely downloaded or is estimated that it will download in time so that buffering stops do not occur).

`````javascript
myAudio.addEventListener("canplaythrough", function() {
  //audio is ready to play all the way through
});
`````

To recap, the order of the media loading events are:

```loadstart``` > ```durationchange``` > ```loadedmetadata``` > ```loadeddata``` > ```progress``` > ```canplay``` > ```canplaythrough```

We also have a few events that will fire in the case that there is some kind of interuption to the media loading process.

### suspend

Media data is no longer being fetched even though the file has not been entirely downloaded.

### abort

Media data download has been aborted but not due to an error.

### error

An error is encountered while media data is being download.

### emptied

The media buffer has been emptied, possibly due to an error or because the load() method was invoked to reload it.

### stalled

Media data is unexpectedly no longer available.



Media Playing Events
--------------------

We also have another set of events that are useful for reacting to the state of the media playback.

### timeupdate

The ```timeupdate``` event is fired every time the ```currentTime``` property changes. In practice this occurs every 250 milliseconds. This event can be used to trigger the displaying of playback progress.

`````javascript
myAudio.addEventListener("timeupdate", function() {
  //update something related to playback progress
});
`````


### playing 

The ```playing``` event is fired when playback is ready to start after having being paused due to lack of media data.

### waiting

The ```waiting``` event is fired when playback has stopped due to lack of media data, although it is expected to resume once datat becomes available.

### play

The ```play``` event is fired after the ```play()``` method is returned or when the autoplay attribute has caused playback to begin. In short when the state switches from paused to playing.


### pause

The ```pause``` event is fired after the ```pause()``` method is returned. In short when the states switches from playing to paused.


### ended

The ```ended``` event is fired when the end of the media is reached.

`````javascript
myAudio.addEventListener("ended", function() {
  //do something once audio track has finished playing
});
`````

### volumechange

The ```volumechange``` event signifies that the volume has changed and that includes being muted.


An Audio Player with Feedback
-----------------------------

Consider this snippet of HTML:

`````html
<div id="controls">
  Loading ...
  <a href="#" style="display:none" >play</a>
  <a href="#" style="display:none" >pause</a>
</div>
<div id="progress">
  <div id="bar"></div>
</div>

`````

Styled like so:

`````css
#controls
{
   width: 80px;
   float: left;
}
    
#progress
{
   margin-left: 80px;
   border: 1px solid black;
}

#bar {
   height: 20px;
   background-color: green;
}

`````



