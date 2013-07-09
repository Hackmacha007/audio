Arduino library for asynchronous playback of PCM/WAV files direct from SD card

Requires Arduino, SD card and output device (Speaker, Headphones, Amplifier, etc)


### **Features**

* PCM/WAV audio playback direct from SD card
* Asynchronous Playback: Allows code in main loop to run while audio playback occurs.
* Playback uses a single timer: Timer 1 (Uno,Mega) or Timer 3,4 or 5 (Mega)
* Supported formats: WAV files, 8-bit, 8-20khz Sample Rate, mono
* Complimentary or Dual Outputs - see examples
* Supported devices: Arduino Uno, Nano, Mega, etc.
* Files easily converted using iTunes or other software:  
    Click _> Edit > Preferences > Import Settings_  
    Change the dropdown to _WAV Encoder_ and Setting: _Custom > 16.000kHz, 8-bit, Mono_  
    Right click any file in iTunes, and select _"Create WAV Version"_  
    Copy file to SD card using computer


### **Functions**
    tmrpcm.play("filename"); plays a file
    tmrpcm.speakerPin = 11; set to 5,6,11 or 46 for Mega, 9 for Uno, Nano, etc
    tmrpcm.disable(); disables the timer on output pin and stops the music
    tmrpcm.stopPlayback(); stops the music, but leaves the timer running
    tmrpcm.isPlaying();  returns 1 if music playing, 0 if not
    tmrpcm.pause();  pauses/unpauses playback
    tmrpcm.pwmMode = 1; **REMOVED**
    tmrpcm.volume(0); 1(up) or 0(down) to control volume

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
