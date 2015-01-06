Video.js Record
===============

A Video.js plugin for recording audio/video files.

![Screenshot](examples/img/screenshot.png?raw=true "Screenshot")

Installation
------------

You can use [bower](http://bower.io) (`bower install videojs-record`) or
[npm](https://www.npmjs.org) (`npm install videojs-record`) to install the
plugin, or download and include `videojs.record.js` in your project.

The plugin has the following mandatory dependencies:

- [Video.js](http://www.videojs.com) - HTML5 media player that provides the user interface.
- [RecordRTC.js](http://recordrtc.org) - Provides support for audio/video recording.

If you're going to record audio-only, you'll also need these dependencies:

- [wavesurfer.js](https://github.com/katspaugh/wavesurfer.js) - Adds navigable waveform for audio files. Also comes with a [microphone plugin](http://www.wavesurfer.fm/example/microphone) used for realtime visualization of the microphone.
- [videojs-wavesurfer](https://github.com/collab-project/videojs-wavesurfer) - Turns Video.js into an audio-player.

Using the Plugin
----------------

Whether you're going to record audio or video, or both, you'll always need
video.js and recordrtc.js. Start by including these on your page:

```html
<link href="http://vjs.zencdn.net/4.11.3/video-js.css" rel="stylesheet">

<script src="http://vjs.zencdn.net/4.11.3/video.js"></script>
<script src="http://recordrtc.org/latest.js"></script>
```

The videojs-record plugin automatically registers itself when you include the script
on your page:

```html
<script src="videojs.record.js"></script>
```

You also need to include an extra stylesheet that includes a
[custom font](src/css/font) with additional icons:

```html
<link href="videojs.record.css" rel="stylesheet">
```

### Audio/video

If you want to record both audio/video, or video-only, then include a
`video` element on your page that will display a live camera preview:

```html
<video id="myVideo" class="video-js vjs-default-skin"></video>
```

Check out the full [audio/video](examples/audio-video.html "audio/video example") or
[video-only](examples/video-only.html "video-only example") examples.

### Audio-only

![Audio-only screenshot](examples/img/audio-only.png?raw=true "Audio-only screenshot")

If you're only recording audio, you also need to include wavesurfer.js and
the videojs-wavesurfer and microphone plugins. Make sure to place them before
the `videojs.record.js` script.

```html
<script src="http://wavesurfer.fm/build/wavesurfer.min.js"></script>
<script src="http://wavesurfer.fm/plugin/wavesurfer.microphone.js"></script>
<script src="videojs.wavesurfer.js"></script>
```

And define an `audio` element:

```html
<audio id="myAudio" class="video-js vjs-default-skin"></audio>
```

Check out the full [audio-only](examples/audio-only.html "audio-only example")
example.

Options
-------

Configure the player using the video.js
[options](https://github.com/videojs/video.js/blob/master/docs/guides/options.md),
and enable the plugin by adding a `record` entry. For example:

```javascript
var player = videojs("myVideo",
{
    controls: true,
    loop: false,
    width: 320,
    height: 240,
    plugins: {
        record: {
            audio: false,
            video: true,
            recordTimeMax: 5
        }
    }
});
```

Plugin options
--------------

Available options for this plugin:

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `audio` | boolean | `false` | Set to `true` for recording audio. |
| `video` | boolean | `true` | Set to `true` for recording video. |
| `recordTimeMax` | float | `10` | Maximum length of a recorded clip. |

Plugin events
-------------

Events for this plugin that are available on the video.js player instance:

| Event | Description |
| --- | --- |
| `deviceError` | Triggered when the user doesn't allow the browser to access the microphone. Check `player.deviceErrorCode` for the specific [error code](https://developer.mozilla.org/en-US/docs/NavigatorUserMedia.getUserMedia#errorCallback). |
| `startRecord` | Triggered when the user clicked the record button to start recording. |
| `stopRecord` | Triggered when the user clicked the stop button to stop recording. |
| `finishRecord` | Triggered once the recorded stream is available. Check `player.recordedData` for the Blob data of your recorded clip. |

Get recorded data
-----------------

Listen for the `finishRecord` event and obtain the clip data from the
`player.recordedData` attribute for further processing:

```javascript
// user completed recording and stream is available
player.on('finishRecord', function()
{
    // the blob object contains the recorded data that
    // can be downloaded by the user, stored on server etc.
    console.log('finished recording: ', player.recordedData);
});
```

Customizing controls
--------------------

If you want to disable and hide specific controls, use the video.js `children`
option:

```javascript
// user completed recording and stream is available
player.on('finishRecord', function()
{
    // the blob object contains the recorded data that
    // can be downloaded by the user, stored on server etc.
    console.log('finished recording: ', player.recordedData);
});
```

Development
-----------

Install `grunt-cli`:

```
sudo npm install -g grunt-cli
```

Install dependencies using npm:

```
npm install
```

Or using bower:

```
bower install
```

Build a minified version:

```
grunt
```

Generated files are placed in the `dist` directory.

License
-------

This work is licensed under the [MIT License](LICENSE).
