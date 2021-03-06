Audio and Video Delivery
========================

We can deliver audio and video on the web in a number of ways, ranging from 'static' media files to adaptive live streams. 

This article is intended as a starting point for exploring the various delivery mechanisms of web based media and compatibility with popular browsers.

The Audio and Video Elements
----------------------------

Whether we are dealing with pre-recorded audio files or live streams, the mechanism for making them available through the browser's audio and video elements remains pretty-much the same. Currently to support all browsers we need to specify two formats although with the adoption of mp3 and mp4 formats in Firefox and Opera, this is changing fast.

[Audio Codec Compatibility Table](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Audio_Codec_Support) and [Audio/Video Codec Compatibility Table](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats#Browser_compatibility)


(Below - Intended as a Flow Chart)
``````
Check format 1 --> If supported set format 1 as source
               --> If not supported check format 2 --> If supported set format 2 as source
                                                   --> If format 1 and format 2 not supported provide a fallback
``````

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

For further info see [video element](https://developer.mozilla.org/en/docs/Web/HTML/Element/video) and [Creating a cross-browser video player](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/cross_browser_video_player)

###Audio and Video Fallback

You can create a more comprehensive Fallback using Flash. Using [flashmediaelement.swf](https://github.com/johndyer/mediaelement/blob/master/build/flashmediaelement.swf) is one way.

`````html
<audio controls>
  <source src="audiofile.mp3" type="audio/mpeg">
  <source src="audiofile.ogg" type="audio/ogg">
  <!-- fallback for non supporting browsers goes here -->
  <a href="audiofile.mp3">download audio</a>
  <object width="320" height="30" type="application/x-shockwave-flash" data="flashmediaelement.swf">
    <param name="movie" value="flashmediaelement.swf" />
    <param name="flashvars" value="controls=true&isvideo=false&file=audiofile.mp3" />
  </object>
</audio>
`````

The process is very similar with video - just remember to set `````isvideo=true`````.

[More options for Fallbacks](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Fallbacks).

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

It's also possible to feed a an audio element base64 encoded wav file, allowing to generate audio on the fly.

`````html
<audio id="player" src="data:audio/x-wav;base64,UklGRvC..."></audio>
`````
[Speak.js](https://github.com/kripken/speak.js/) employs this technique. [Try the demo](http://speak-demo.herokuapp.com).


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
    request.open("GET","http://jplayer.org/audio/mp3/RioMez-01-Sleep_together.mp3",true); 
    request.responseType = "arraybuffer"; 

    request.onload = function() { 
      context.decodeAudioData(request.response, function(buffer) { 
        source = context.createBufferSource();  
        source.buffer = buffer; 
        source.connect(context.destination); 
        // auto play
        source.start(0); // start was previously noteOn
      }); 
    }; 

    request.send(); 

  } catch(e) { 
    alert('web audio api not supported'); 
  } 
`````

[Try it for yourself](http://jsbin.com/facutone/1/edit?js)

In this example we retrieve an MP3 file via XHR, load it into a source and play it.

###getUserMedia / Stream API

It's also possible to plug a live stream from a webcam and/or microphone using `````getUserMedia````` and the Stream API. This makes up part of a wider technology known as WebRTC (Web Real-Time Communications) and is compatible with the atest versions of Chrome, Firefox and Opera.

To grab the stream from your webcam, first set up a video element:

`````html
<video id="webcam" width="480" height="360"></video>
`````

Next, if supported connect the webcam source to the video element:

`````javascript
navigator.getUserMedia ||
  (navigator.getUserMedia = navigator.mozGetUserMedia ||
  navigator.webkitGetUserMedia || navigator.msGetUserMedia);

window.URL = window.URL || window.webkitURL || window.mozURL || window.msURL;

if (navigator.getUserMedia) {
    navigator.getUserMedia({
        video: true,
        audio: false
    }, onSuccess, onError);
} else {
    alert('getUserMedia is not supported in this browser.');
}

function onSuccess(stream) {
    var video = document.getElementById('webcam');
    video.autoplay = true;
    video.src = window.URL.createObjectURL(stream);
}

function onError() {
    alert('There has been a problem retreiving the streams - are you running on file:/// or did you disallow access?');
}
`````

Mediastream Recording
---------------------

New standards are being rolled out to allow your browser to grab media from your mic or camera using getUserMedia and record it using a new MediaRecorder API.

You effectively take the stream you receive from getUserMedia, pass it to the MediaRecorder, take the resulting output and feed it to your audio or video source*.

The main mechanism is outline below.

`````javascript
var onSuccess = function(stream) {

  var mediaRecorder = new MediaRecorder(stream);

  mediaRecorder.ondataavailable = function(e) {
    var audio = document.createElement('audio');
    audio.src = window.URL.createObjectURL(e.data);
  }
}

navigator.getUserMedia({audio:true}, onSuccess);
`````
See [MediaRecorder API](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder_API) for more details.

> *Note. This is only supported in Firefox and only for audio, however there are some interesting workarounds including a library called [RecordRTC](https://github.com/muaz-khan/WebRTC-Experiment/tree/master/RecordRTC).

###Media Source Extensions (MSE)


[Media Source Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/media-source.html) is a W3C working draft that plans to extend HTMLMediaElement to allow JavaScript to generate media streams for playback. Allowing JavaScript to generate streams facilitates a variety of use cases like adaptive streaming and time shifting live streams.

###Encrypted Media Extensions (EME)


[Encrypted Media Extensions](https://dvcs.w3.org/hg/html-media/raw-file/tip/encrypted-media/encrypted-media.html) is a W3C proposal to extend HTMLMediaElement providing APIs to control playback of protected content.

The API supports use cases ranging from simple clear key decryption to high value video (given an appropriate user agent implementation). License/key exchange is controlled by the application, facilitating the development of robust playback applications supporting a range of content decryption and protection technologies.

One of the principle uses of EME is to allow browsers to implement [DRM (Digital Rights Management)](http://en.wikipedia.org/wiki/Digital_rights_management) which helps prevent web-based content (especially video) from being copied.

###Adaptive Streaming

New formats and protocols are being rolled out to facilitate adaptive streaming. Adaptive streaming media means that the bandwidth and typically quality of the stream can change in real-time in reaction to the user's available bandwidth. Adaptive streaming is often used in conjunction with live streaming where smooth delivery of audio or video is paramount. 

The main formats used for adaptive streaming are [HLS](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video#HLS) and [MPEG-DASH](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video#MPEG-DASH) 

MSE has been designed with DASH in mind. MSE defines byte streams according to [ISOBMFF](https://dvcs.w3.org/hg/html-media/raw-file/tip/media-source/isobmff-byte-stream-format.html) and [M2TS](http://en.wikipedia.org/wiki/M2ts) (both supported in DASH, the latter supported in HLS).

Generally speaking, if you are interested in standards, are looking for flexibility or wish to support most modern browsers you are probably better of with DASH. 

> Note. Currently Safari does not support DASH although dash.js will work on newer versions of Safari scheduled for release with OSX Yosemite.

DASH also provides a number of profiles including simple onDemand profiles that no preprocessing and splitting up of media files.

There are also a number of cloud based services that will convert your media to both HLS and DASH.

For further info see [Live streaming web audio and video](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Live_streaming_web_audio_and_video)



Debugging Audio / Video Issues
------------------------------

Having issues playing back audio or video? Try the following check-list.

###Does the browser support supplied formats?

Use the following verified sources within your audio and video elements to check support.

- Audio MP3 - (type="audio/mpeg") - [http://jPlayer.org/audio/mp3/Miaow-01-Tempered-song.mp3](http://jPlayer.org/audio/mp3/Miaow-01-Tempered-song.mp3) ([try it](http://jsbin.com/gekatoge/1/edit))
- Audio MP4 - (type="audio/mp4") - [http://jPlayer.org/audio/m4a/Miaow-01-Tempered-song.m4a](http://jPlayer.org/audio/m4a/Miaow-01-Tempered-song.m4a) ([try it](http://jsbin.com/gekatoge/2/edit))
- Audio Ogg - (type="audio/ogg") - [http://jPlayer.org/audio/ogg/Miaow-01-Tempered-song.ogg](http://jPlayer.org/audio/ogg/Miaow-01-Tempered-song.ogg) ([try it](http://jsbin.com/gekatoge/4/edit))

- Video MP4 - (type="video/mp4") - [http://jPlayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v](http://jPlayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v) ([try it](http://jsbin.com/gekatoge/5/edit))
- Video WebM - (type="video/webm") - [http://jPlayer.org/video/webm/Big_Buck_Bunny_Trailer.webm](http://jPlayer.org/video/webm/Big_Buck_Bunny_Trailer.webm) ([try it](http://jsbin.com/gekatoge/6/edit))
- Video Ogg - (type="video/ogg") - [http://jPlayer.org/video/ogv/Big_Buck_Bunny_Trailer.ogv](http://jPlayer.org/video/ogv/Big_Buck_Bunny_Trailer.ogv) ([try it](http://jsbin.com/gekatoge/7/edit))


If these don't play then the browser you are testing doesn't support the given format. Consider using a different format or using a fallback.

If these work but the files you are supplying don't, there are two possible issues:

###1. The media server is not delivering the correct mime types with file

Although this is usutally supported, you may need to add the following to your media server's .htaccess file.

`````
# AddType TYPE/SUBTYPE EXTENSION
 
AddType audio/mpeg mp3
AddType audio/mp4 m4a
AddType audio/ogg ogg
AddType audio/ogg oga
 
AddType video/mp4 mp4
AddType video/mp4 m4v
AddType video/ogg ogv
AddType video/webm webm
AddType video/webm webmv

`````

###2. Your files have been encoded incorrectly

Your files may have been encoded incorrectly - try encoding using on one of the following encoders:

- [Audacity](http://audacity.sourceforge.net/)
- [Miro](http://www.getmiro.com/)
- [Handbrake](http://handbrake.fr/)
- [Firefogg](http://firefogg.org/)
- [FFmpeg2](https://www.ffmpeg.org/)
- [Libav](https://libav.org/)
- [Vid.ly](http://m.vid.ly/)
- [Archive.org](https://archive.org/)


Customising Your Media Player
-----------------------------

You may decide that you want your audio or video player to have a consistent look across browsers, or just wish to tweak it to match your site. The general technique for achieving this is to omit the controls attribute so that the default browser controls are not displayed, then use JavaScript to link your controls to the audio/video API.

If you need something extra, it's possible to add features that are not currently present in default players, such as play-back rate, quality stream-switches or even audio spectrums. You can also choose how to make your player responsive - for example you might remove the progress-bar under certain conditions.

You may detect click, touch and/or keyboard events to trigger actions such as play, pause and scrubbing. It's often important to remember keyboard controls for user convenience and accessibility.

A quick example - first set up your audio and custom controls in HTML:

`````html
  <audio id="my-audio" src="http://jPlayer.org/audio/mp3/Miaow-01-Tempered-song.mp3"></audio>
  <button id="my-control">play</button>

`````

add a but of JavaScript to detect events to play and pause the audio:

`````javascript
window.onload = function() {
  
  var myAudio = document.getElementById('my-audio');
  var myControl = document.getElementById('my-control');
  
  function switchState() {
    if (myAudio.paused == true) {
      myAudio.play();
      myControl.innerHTML = "pause";
    } else {
      myAudio.pause();
      myControl.innerHTML = "play";
    }
  }
  
  function checkKey(e) {
    if (e.keycode == 32 ) { //spacebar
      switchState();
    }
  }
  
  myControl.addEventListener('click', function() {
    switchState();
  }, false);
  
  window.addEventListener( "keypress", checkKey, false );
} 

`````

[Try it out here](http://jsbin.com/jujeladu/2/edit).

[Creating your own custom audio player](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Cross-browser_audio_basics#Creating_your_own_custom_audio_player)


Audio/Video JavaScript Libraries
--------------------------------

A number of audio and video JavaScript libaries exist. The most popular libraries allow you to choose a consistent player design over all browsers and provide a fallback for browsers that don't support audio and video natively. Fallbacks often use Adobe Flash or Microsoft Silverlight plugins. Other functionality such as the track element
for subtitles can also be provided through media libraries.

Popular libraries that extend the audio and video element are:

###Audio only
- [SoundManager](http://www.schillmania.com/projects/soundmanager2/)

###Video only
- [flowplayer](https://flowplayer.org/) - Gratis with a flowplayer logo watermark. Open source (GPL licensed)
- [JWPlayer](http://www.jwplayer.com) - Requires registration to download. Open Source Edition (Creative Commons License)
- [SublimeVideo](http://www.sublimevideo.net/) - Requires registration. Form based set up with domain specific link to CDN hosted library.
- [Video.js](http://www.videojs.com/) - Gratis and Open Source (Apache 2 Licensed)


###Audio and Video
- [jPlayer](http://jPlayer.org) - Gratis and Open Source (MIT Licensed)
- [mediaelement.js](http://mediaelementjs.com/) - Gratis and Open Source (MIT Licensed)


There's also an increasing number of libaries that take advantage of the newer Web Audio API and we're also seeing specialized libraries for manipulating video appear.

###Web Audio API
- [AudioContext monkeypatch](https://github.com/cwilso/AudioContext-MonkeyPatch) - (A polyfill for older versions of the Web Audio API) - Open Source (Apache 2 Licensed)


[More info on audio/video libraries]()


See also
--------

- [Media events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Media_events)
- [Using HTML5 audio and video](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML5_audio_and_video)
- [HTMLVideoElement](https://developer.mozilla.org/en/docs/Web/API/HTMLVideoElement)
- [Cross-browser audio basics](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/Cross-browser_audio_basics)
- [Media Source](https://developer.mozilla.org/en-US/docs/Web/API/MediaSource)
- [Easy audio capture with the MediaRecorder API](https://hacks.mozilla.org/2014/06/easy-audio-capture-with-the-mediarecorder-api/)
- [Media Recording Example](https://github.com/PinZhang/sample-codes/blob/master/html5/media_recording.html)
- [Buffering, Seeking and Time Ranges](https://developer.mozilla.org/en-US/Apps/Build/Manipulating_media/buffering_seeking_time_ranges)
