hotwordwrap

# Hotword detector wrapper
Now you can use sox or arecord in linux based systems
Written in [Node.js](https://nodejs.org/) using latest [Snowboy](https://snowboy.kitt.ai/) version by [Kitt.ai](https://kitt.ai/). Snowboy is an offline neural network driven hotword detection library, with compatibility only on MacOS (darwin) and most Linux distro's. This module aims to simplify and improve the standard [snowboy module](https://github.com/Kitt-AI/snowboy) by turning it into one single interface with build-in microphone recording and by adding start, stop, pause, and resume functionality.


## Installation
```
npm i --save hotwordwrapper
```

## Dependencies
This module uses snowboy, therefore it requires a some resources before hand please see the [official website](https://snowboy.kitt.ai/) for these resources.

The module also uses the [node-audiorecorder](https://github.com/RedKenrok/node-audiorecorder) module. It requires you to have [SoX](http://sox.sourceforge.net/) installed and it must be available in your $PATH. For more information see the [node-audiorecorder](https://github.com/RedKenrok/node-audiorecorder) module.

## Usage

### Constructor
```javascript
// Import module.
const HotwordDetector = require('hotwordwrapper');

// Detector data.
// See the 'snowboy' module for more information.
const detectorData = {
  resource: './node_modules/snowboy/resources/common.res'
};
// Array of data for each hotword model.
// See the 'snowboy' module for more information.
const modelData = [
  {
    file: './node_modules/snowboy/resources/snowboy.umdl',
    hotwords : 'snowboy',
    sensitivity: '0.5'
  }
];
// Optional parameter to select the recording options.
/*
options - JSON containing command line options. Following are valid options:
endian: big OR little, default: little
bitwidth: 8 OR 16 OR 24 OR anything valid supported by arecord OR sox, default: 16
encoding: signed-integer OR unsinged-integer (none of the other encoding formats are supported), default:signed-integer
rate: 8000 OR 16000 OR 44100 OR anything valid supported by arecord OR sox, default: 16000
channels: 1 OR 2 OR anything valid supported by arecord OR sox, default: 1 (mono)
device: hw:0,0 OR plughw:1, 0 OR anything valid supported by arecord. Ignored for sox on macOS.
debug: true OR false - can be used to aide in debugging
fileType: string defaults to 'raw', allows you to set a valid file type such as 'wav' (for sox only) to avoid the no header issue mentioned above, see a list of types here
*/
const recorderData = {
  rate: '16000',
    channels: '1',
    debug: true,
};
// Optional parameter intended for debugging.
// The object has to implement a log and warn function.
const logger = console;

// Create an instance.
let hotwordDetector = new HotwordDetector(detectorData, modelData, recorderData, logger);
```

> More information about [mic](https://github.com/ashishbajaj99/mic#readme).

### Methods
```javascript
// Creates the detector and starts the recording process.
hotwordDetector.Start();
// Stops the detection process and removes the recording process.
hotwordDetector.Stop();
```

### Events
```javascript
// Triggered when an error is encountered.
hotwordDetector.on('error', function(error) {
  console.error('hotwordDetector: ' + error);
});
// Triggered when a hotword has been detected.
hotwordDetector.on('hotword', function(index, hotword, buffer) {
  // Index is the associated index of the detected hotword.
  // Hotword is a string of which word has been detected.
  // Buffer is the most recent section from the audio buffer.
  console.log('hotwordDetector: Hotword detected: ' + hotword);
});
// Triggered when there is no audible sound being recorded.
hotwordDetector.on('silence', function() {
  console.log('hotwordDetector: silence');
});
// Triggered when there is audible sound being recorded.
hotwordDetector.on('sound', function(buffer) {
  // Buffer is the most recent section from the audio buffer.
  console.log('hotwordDetector: sound: ' + buffer);
});
