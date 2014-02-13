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

As mentioned above ``controls`` is an attribute that we specify when we require the browser to provide us with a play-back controls.

`````html
<audio preload="controls">
  ...
</audio>
`````

> Note - on mobile platforms controls will often not include volume as this is handle by the underlying OS.

Fallbacks
---------

Although the vast majority of browsers now support the audio element, you may want to cater for those that don't. Usually these browsers are catered for by using fallbacks written for plugins such as Adobe Flash or Microsoft Silverlight.

> Note - you should be aware that Flash and Silverlight code require that the user has the appropriate plugin installed and significantly that the *browser cannot guarantee the security* aspects of code running on those plugin platforms.



Manipulating Audio Properties with JavaScript
---------------------------------------------





