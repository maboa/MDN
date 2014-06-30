Audio and Video Delivery
========================


###HTML Audio

`````html
<audio controls preload="auto">
  <source src="audiofile.mp3" type="audio/mpeg">
  
  <!-- fallback for browsers that don't suppport mp3 -->
  <source src="audiofile.ogg" type="audio/ogg">
  
  <!-- fallback for browsers that don't support audio tag -->
  <a href="audiofile.mp3">download audio</a>
</audio>
`````

The code above will create an audio player that attempts to preload as much audio as possible for smooth playback.

> Note: preload may be ignored by some mobile browsers.

For further info see [Cross Browser Audio Basics (HTML5 Audio In Detail)](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Cross-browser_audio_basics#HTML5_audio_in_detail)


###HTML Video

`````html
<video controls width="640" height="480" poster="initialimage.png" autoplay muted>
  <source src="videofile.mp4" type="video/mp4">
  
  <!-- fallback for browsers that don't suppport mp4 -->
  <source src="videofile.webm" type="video/webm">
  
  <!-- specifying subtitle files -->
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English">
  <track src="subtitles_no.vtt" kind="subtitles" srclang="no" label="Norwegian">
  
  <!-- fallback for browsers that don't support video tag -->
  <a href="videofile.mp4">download video</a>
</video>
`````

The code above creates a video player of dimensions 640x480 pixels, displaying a poster image until the video is played. We instruct the video to autoplay but to be muted by default.

> Note: autoplay may be ignored by some mobile browsers.

###JavaScript Audio

`````javascript
var myAudio = document.createElement('audio');

if (myAudio.canPlayType('audio/mpeg')) {
  myAudio.setAttribute('src','audiofile.mp3');
} else if (myAudio.canPlayType('audio/ogg')) {
  myAudio.setAttribute('src','audiofile.ogg');
}

myAudio.currentTime = 5;
myAudio.play();
`````

We set the source of the audio depending on the type of audio file the browser supports we then set the play-head 5 seconds in and attempt to play.

> Note: Play will be ignored by some mobile browsers unless issued by a user-initiated event.

###JavaScript Video

`````javascript
var myVideo = document.createElement('video');

if (myVideo.canPlayType('video/mp4')) {
  myVideo.setAttribute('src','videofile.mp4');
} else if (myVideo.canPlayType('video/webm')) {
  myVideo.setAttribute('src','videofile.webm');
}

myVideo.width = 480;
myVideo.height = 320;
`````
We set the source of the video depending on the type of video file the browser supports we then set the width and height of the video.

###Web Audio API

`````javascript
var context;
var request; 
var source;

try { 
  context = new (window.AudioContext || window.webkitAudioContext)();
  request = new XMLHttpRequest(); 
  request.open("GET","myaudio.mp3",true); 
  request.responseType = "arraybuffer"; 
  
  request.onload = function() { 
    context.decodeAudioData(request.response, function(buffer) { 
      source = context.createBufferSource();  
      source.buffer = buffer; 
      source.connect(context.destination); 
      // auto play
      source.noteOn(0); 
    }); 
  }; 
  
  request.send(); 
  
} catch(e) { 
  alert('web audio api not supported'); 
} 


`````

In this example we retrieve an MP3 file via XHR, load it into a source and play it.

Media Source Extensions (MSE)
-----------------------------

[Media Source Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/media-source.html) is a W3C working draft that plans to extend HTMLMediaElement to allow JavaScript to generate media streams for playback. Allowing JavaScript to generate streams facilitates a variety of use cases like adaptive streaming and time shifting live streams.

Encrypted Media Extensions (EME)
--------------------------------

[Encrypted Media Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/encrypted-media/encrypted-media.html) is a W3C proposal to extend HTMLMediaElement providing APIs to control playback of protected content.

The API supports use cases ranging from simple clear key decryption to high value video (given an appropriate user agent implementation). License/key exchange is controlled by the application, facilitating the development of robust playback applications supporting a range of content decryption and protection technologies.

One of the principle uses of EME is to allow browsers to implement [DRM (Digital Rights Management)](http://en.wikipedia.org/wiki/Digital_rights_management) which helps protect web-based content (especially video) from being copied.
