Arduino library for asynchronous playback of PCM/WAV files direct from SD card

Requires Arduino, SD card and output device (Speaker, Headphones, Amplifier, etc)


### **Features**

* NEW: Up to 32khz sample rate support:
	Supported formats: WAV files, 8-bit, 8-32khz Sample Rate, mono
* PCM/WAV playback direct from SD card
* Asynchronous Playback: Allows code in main loop to run while audio playback occurs.
* Playback uses a single timer: Timer 1 (Uno,Mega) or Timer 3,4 or 5 (Mega)
* Quality setting: Allows 2xSample rate operation for oversampling with low sample rates
* Supported devices: Arduino Uno, Nano, Mega, etc.
* Supports complimentary output or dual speakers
* Files easily converted:
Using iTunes:

    Click _> Edit > Preferences > Import Settings_  
    Change the dropdown to _WAV Encoder_ and Setting: _Custom > 16.000kHz, 8-bit, Mono_  
    Right click any file in iTunes, and select _"Create WAV Version"_      

Using Audacity:
	
    Tracks > Stereo Track to Mono    
    Project Rate (HZ) > set to 16000, 11025, or 8000    
    File > Export > Save as type: Other uncompressed files > Options...    
    Select WAV, Unsigned 8 bit PCM    
    
Then copy file to SD card using computer

### **Functions**
    TMRpcm audio;
    audio.play("filename"); plays a file
    audio.speakerPin = 11; set to 5,6,11 or 46 for Mega, 9 for Uno, Nano, etc
    audio.disable(); disables the timer on output pin and stops the music
    audio.stopPlayback(); stops the music, but leaves the timer running
    audio.isPlaying();  returns 1 if music playing, 0 if not
    audio.pause();  pauses/unpauses playback
    audio.quality(1); for use with low sample rates
    audio.volume(0); 1(up) or 0(down) to control volume
    audio.setVolume(0); 0 to 7. Set volume level

**Contributed HowTo:** http://maxoffsky.com/maxoffsky-blog/how-to-play-wav-audio-files-with-arduino-uno-and-microsd-card/

**Details at:** http://tmrh20.blogspot.com

### **Known Limitations**

* This library is very processor intensive, so code execution during playback will be slower than normal
* Processing load can be reduced by using lower quality sounds encoded at a lower sample rate (8khz minimum)
* May interfere with other libraries that rely on interrupts. The isPlaying() function can be used to
  prevent parallel code execution.
* Update to volume control allows greater range in low volume control, but will distort
  if volume too high

### **Installation**

1. Download current package: <https://github.com/TMRh20/TMRpcm/archive/master.zip>
2. See the Manual Installation section at: <http://arduino.cc/en/Guide/Libraries>

**Contributed How-To with Video:** <http://maxoffsky.com/maxoffsky-blog/how-to-play-wav-audio-files-with-arduino-uno-and-microsd-card/>

Details at [TMRh20.blogspot.com](http://tmrh20.blogspot.com)
