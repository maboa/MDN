Playback Rate
-------------

```playbackRate``` allows us to change the speed or rate that a piece of web audio or video is playing.

Here's an example:

`````javascript
var myAudio = document.createElement('audio');
myAudio.setAttribute('src','audiofile.mp3');
myAudio.playbackRate = 0.5;
`````

Here we create an ```audio``` element and set the ```src``` to a file of our choice. Next we set the ```playbackRate``` to 0.5 which represents half normal speed. The ```playbackRate``` is a multiple applied to the recorded rate.


> *Note* : Most browsers stop playing audio below a playbackRate of 0.5 and above 4. Video will continue to play silently however. So it's recommended for most applications that you limit the range to between 0.5 and 4.

