Clap detection module for node js
===

## Synopsis

At the top of the file there should be a short introduction and/ or overview that explains **what** the project is. This description should match descriptions added for package managers (Gemspec, package.json, etc.)
I created this module for my project of personal assistant on Raspberry Pi (raspbian). The clap detection allows me to activate the assistant whenever I need it (and prevent it from continuously being listening for instructions or interpreting random noises as instructions)

## Requirements
This module works on a linux based OS (raspbian, Ubuntu, Debian...) using alsa for audio and having a working microphone.

## Installation

This module requires sox, "the Swiss Army knife of sound processing programs" (http://sox.sourceforge.net/) to be installed
```bash
sudo apt-get install sox
```
You can simply add this module to your node.js project with
```bash
// sudo might be required depending on your system
npm install --save clap-detector
```

## Usage

There are thee public methods you can use:
- clapDetector.start(clapConfig);
=> this needs to be called to initialize clap detection. The clapConfig object is not mandatory but you can use it to overwrite the default configuration (see next section)
- clapDetector.onClap(yourcallbackfunctionhere)
=> register a callback that will be triggered whenever a clap of hand is detected
- clapDetector.onClaps(numberOfClaps, delay, yourcallbackfunctionhere)
=> register a callback that will be triggered whenever a serie of claps (determined by the number of claps) is detected within the period of time you've specified (delay).

```bash
// Require the module
var clapDetector = require('clap-detector');

// Define configuration
var clapConfig = {
   AUDIO_SOURCE: 'hw:1,0'
};

// Start clap detection
clapDetector.start(clapConfig);

// Register on clap event
clapDetector.onClap(function() {
    //console.log('your callback code here ');
}.bind(this));

// Register to a serie of 3 claps occuring within 3 seconds
clapDetector.onClaps(3, 2000, function(delay) {
    //console.log('your callback code here ');
}.bind(this));
```

## Configuration

You can pass a configuration object at initialisation time (clapDetector.init(yourConfObject)). If you don't the following config will be used. You should at least provide the audio input (if different from the default config).

```bash
/* DEFAULT CONFIG */
    var CONFIG = {
        AUDIO_SOURCE: 'hw:1,0', // this is your microphone input. if you don't know it you can refer to this thread (http://www.voxforge.org/home/docs/faq/faq/linux-how-to-determine-your-audio-cards-or-usb-mics-maximum-sampling-rate)
        DETECTION_PERCENTAGE_START : '5%', // minimum noise percentage threshold necessary to start recording sound
        DETECTION_PERCENTAGE_END: '5%',  // minimum noise percentage threshold necessary to stop recording sound
        CLEANING: {
            PERFORM: false, // set to true if you want to clean the file from noise before analyzing it. It requires a sox noise profile
            NOISE_PROFILE: 'noise.prof' // path to the sox noise profile
        },
        SOUND_FILE : "input.wav", // name of wav file created while recording
        SOUND_FILE_CLEAN  : "input-clean.wav", // name of cleaned (if you activated cleaning) file
        CLAP_AMPLITUDE_THRESHOLD: 0.7, // minimum amplitude threshold to be considered as clap
        CLAP_ENERGY_THRESHOLD: 0.3,  // minimum energy threshold to be considered as clap
        MAX_HISTORY_LENGTH: 10 // all claps are stored in history, this is its max length
    };
```

If you wish to improve the clap detection you can fiddle with the CLAP_AMPLITUDE_THRESHOLD and CLAP_ENERGY_THRESHOLD values. Depending on your microphone these might need to be modified.
You can also activate cleaning (CLEANING.perform) of the noise for better results. In order to do that record a wav file (using your microphone) without talking and then generate the sox profile with the command:

```bash
sox -c 1 SILENCE.WAV -n trim 0 2 noiseprof noise.prof
```

## Tests

These will be added soon. Please do not hesitate to add some !

## About the Author

I am a full-stack Javascript developer based in Lyon, France.

[Check out my website](http://www.thomschell.com)

## License

clap-detector is dual licensed under the MIT license and GPL.
For more information click [here](https://opensource.org/licenses/MIT).