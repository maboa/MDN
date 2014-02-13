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

myAudio.currentTime(5); // play from 5 seconds in

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

> Note - very early specs specified that the browser should return "no" instead of empty string. Thankfully the number of people using these browsers are few and far between.

### currentTime

```play``` does not take a parameter so we need to set the point to play from separately using the ```currentTime``` property. If not set the default is zero.

We can get ```currentTime``` as well as set. The value is number that specifies the time in seconds.

`````javascript
if (myAudio.currentTime > 5) {
  myAudio.currentTime = 3;
}
`````




