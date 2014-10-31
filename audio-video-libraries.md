Comparison of Audio / Video Libraries
=====================================

Instead of using the built-in browser audio and video controls, you may decide that you want something custom and consistent over a range of browsers, perhaps with a fallback built in for browsers that don't support audio or video natively.

You could write a player yourself but dealing with the differences between browsers can be a pain. You could instead use one of a number of JavaScript libraries, but which one to choose?

Your decision will rely on a number of a criteria that you consider important, these may range from integration ease, licensing, pricing and support to visual aspects, accessibility, customization and media hosting. The aim of this article is to take a look at the various libraries out there and present the features of each one.

Not we have only included projects that appear to have been updated in the last 12 months - audio and video on the web is a fast moving area.

Licensing and Pricing
---------------------

Often one of the first considerations for many developers or designers are price and licensing so let's commence by looking at those two aspects.



### Audio and Video

| Library                | License             | Pricing                                             |
| ---------------------- | ------------------- | --------------------------------------------------- |
| Acorn Media Player     | MIT                 | Full version free                                   |     
| jPlayer                | MIT                 | Full version free                                   |
| Leanback Player        | GPL v3 / Commercial | Non commercial free / commercial licenses available |
| mediaelement.js        | MIT                 | Full version free                                   |
| OSMPlayer              | MIT                 | Full version free                                   |
| Video.js               | Apche 2.0           | Full version free                                   |



### Audio Only

| Library                | License          | Pricing             |
| ---------------------- | ---------------- | ------------------- |
| SoundJS                | MIT              | Full version free   |
| SoundManager 2         | BSD              | Full version free   | 



### Video Only

| Library                | License          | Pricing                                                            |
| ---------------------- | ---------------- | -------------------------------------------------------------------| 
| Codo Player            | GPL / Commercial | Standard version free with logo, commercial pro licenses available |
| flowplayer             | GPL / Commercial | Standard version free with logo, commercial pro licenses available |
| jMediaelement2         | MIT / GPL        | Full version free                                                  |
| JW Player              | CC / Commercial  | Standard version free with logo, commercial pro licenses available |
| Kaltura Player         | AGPL v3          | Full version free                                                  |
| Projekktor             | GPL              | Full version free                                                  |
| SublimeVideo           | remote hosted    | Free for personal use, commercial pro licenses available           |


Download and Repository
-----------------------

Once you've identified a potential library to go with you'll probably want to know how easy it is to download and install and what the repository looks like.

### Audio and Video

| Library                | Download              | Public Repo                                | Repo Stars* |
| ---------------------- | --------------------- | ------------------------------------------ | ----------- |
| Acorn Media Player     | Direct includes demos | https://github.com/ghinda/acornmediaplayer |          74 |
| jPlayer                | Direct includes demos | https://github.com/happyworm/jPlayer       |       2,588 |
| Leanback Player        | Direct                | unknown                                    |           - |
| mediaelement.js        | Direct includes demos | https://github.com/johndyer/mediaelement   |       2,849 |
| OSMPlayer              | Github                | https://github.com/mediafront/osmplayer    |         255 |
| Video.js               | Direct includes demo  | https://github.com/videojs/video.js        |       7,084 |



### Audio Only

| Library                | Download              | Public Repo                                    | Repo Stars* |
| ---------------------- | --------------------- | ---------------------------------------------- | ----------- |
| SoundJS                | Github includes demos | https://github.com/CreateJS/SoundJS            |       1,195 |
| SoundManager 2         | Direct includes demos | https://github.com/scottschiller/SoundManager2 |       1,984 |




### Video Only  

| Library                | Download              | Public Repo                               | Repo Stars* |
| ---------------------- | --------------------- | ----------------------------------------- | ----------- |
| Codo Player            | Direct includes demo  | unknown                                   |           - |
| flowplayer             | Registration required | https://github.com/flowplayer/flowplayer  |         787 |
| jMediaelement2         | Github includes demos | https://github.com/aFarkas/jMediaelement  |         147 |
| JW Player              | Registration required | https://github.com/jwplayer/jwplayer      |         126 |
| Kaltura Player         | Registration required | https://github.com/kaltura/mwEmbed        |          93 |
| Projekktor             | Direct includes demo  | https://github.com/frankyghost/projekktor |         111 |
| SublimeVideo           | Registration required | unknown                                   |           - |
   
   
*Information correct on October 30th, 2014

Dependencies, Size, Flexibility and Fallbacks
---------------------------------------------

Another key consideration are the library's dependencies - does it rely on jQuery or other libraries or is it standalone?

Dependencies and size of the source code can give you an idea of how long the player will take to load. 

> Note some libraries allow you to create a custom build.

There are two parts to important parts to the flexibility of your player:

- Can you customise it, how and to what extent?
- Can you easily swap a different player in (including the browsers native player) should you decide to change?

We looked at the mechanisms used to theme the player and whether the syntax for defining the player was standard enough that it could be used as a polyfill - in other words does is it defined using standard audio video tags?

Finally we looked at whether the player included a Flash fallback. This not only indicates that it will work on browsers that haven't implemented the audio and video tag, it can be used to provide support for formats that some browsers may not support and be used for RTMP streaming.

### Audio and Video

| Library                | Dependencies                | Library Size | Flash Fallback | Custom Build | Theming  |
| ---------------------- | --------------------------- | ------------ |--------------- | ------------ | -------- |
| Acorn Media Player     | jQuery, jQueryUI, modernizr |  34.6 kB **  | No             |              | CSS      |
| jPlayer                | jQuery or Zepto             |  55.7 kB     | Yes (14.2 kB)  |              | CSS/HTML |
| Leanback Player        | None                        |  62.9 kB     | No             |              | CSS      |
| mediaelement.js        | None                        |  27.6 kB     | Yes (57.1 kB)  |              | CSS      |
| OSMPlayer              | jQuery, jQueryUI            | 105.5 kB     | No             | Yes          | unknown  |
| Video.js               | None                        |  68.8 kB     | Yes (17.0 kB)  | Yes          | CSS      |


### Audio Only

| Library                | Dependencies | Library Size | Flash Fallback            | Custom Build | Theming      |
| ---------------------- | ------------ | -------------|-------------------------- | ------------ | ------------ |
| SoundJS                | None         | 34.3 kB      | Yes (18.9 kB) + SWFObject | No           | CSS/JSON     |
| SoundManager 2         | None         | 51.3 kB      | Yes (8.7 kB)              | Debug build  | CSS/Templates|


### Video Only

| Library                | Dependencies | Library Size | Flash Fallback | Custom Build | Theming        |
| ---------------------- | ------------ | ------------ | -------------- | ------------ | -------------- |
| Codo Player            | None         | 57.2 kB      | Yes (3.6 kB)   | No           | CSS            |
| flowplayer             | jQuery       | 36.4 kB      | Yes (5.8 kB)   | No           | CSS            |
| jMediaelement2         | None         | 26.8 kB      | No             | Yes          | CSS            |
| JW Player              | jQuery       | 162.7 kB     | Yes (165.4 kB) | No           | XML config     |
| Kaltura Player         | unknown      | unknown      | Yes            | Yes          | CSS/JSON       |
| Projekktor             | jQuery       | 136.5 kB     | Yes (15.5 kB)  | Yes          | CSS/Parameters |
| SublimeVideo           | unknown      | unknown      | Yes            | Online       | Online         |


The feature-set of audio and video on the web is comprehensive and constantly growing and evolving.
