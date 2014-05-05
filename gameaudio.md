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

### Volume

Volume is also one of those properties that is often disabled by mobile browsers. The rationale is often that the user should be in control of the volume and this shouldn't be overriden.

### Buffering

Likely an attempt to mitigate runaway mobile network data use, we may also find that buffering is disabled. Buffering is the process of the browser downloading the media in advance, which we often need to do to ensure smooth play back.

### Contemporary Audio Playback

A requirement of many games is to play more than one piece of audio contemporarily. Although the situation is set to get better with the adoption of the Web Audio API. Currently using the vanilla audio element will result in patchy results on mobile.

Take (this example)[http://jsbin.com/visihopa] where we attempt to play three pieces of audio at the same time, on mobile browsers we get the following results. 


| Mobile Browser    | Version |     |
| ----------------- | ------- | --- |
| Chrome (Android)  | 32+     |  N  |
| Firefox (Android) | 26+     |  Y  |
| IE Mobile         | 10+     |  ?  |
| Opera Mobile      | 11+     |  N  |
| Safari (iOS)      | 5+      |  ?  |
| Android Browser   | 2.3+    |  ?  |


### Pages on Home Screens


