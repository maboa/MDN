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
<div class="holder">
  <video id="myVideo" controls>
    <source src="http://jplayer.org/video/m4v/Big_Buck_Bunny_Trailer.m4v" 
          type='video/mp4' />
    <source src="http://jplayer.org/video/webm/Big_Buck_Bunny_Trailer.webm" 
          type='video/webm' />
  </video>
 
  <form>
    <input id="pbr" type="range" value="1" min="0.5" max="4" step="0.1" >
 
    <p>Playback Rate <span id="currentPbr">1</span></p>
  </form>
</div>

`````

