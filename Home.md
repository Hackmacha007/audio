Arduino library for asynchronous playback of PCM/WAV files direct from SD card

Utilizes standard Arduino SD library, SD card and output device (Speaker, Headphones, Amplifier, etc)
  
###**Supported Boards**
All 328 based boards: Arduino Uno, Nano, Duemilanove, etc  
Mega Types: 1280, 2560, etc  
No Due support currently. See [Auto Analog Audio](http://tmrh20.github.io/AAAudio/)
  
###**Advanced Features**
See the [Advanced Features wiki page](https://github.com/TMRh20/TMRpcm/wiki/Advanced-Features)

### **Features**

   * PCM/WAV playback direct from SD card  
   * Main formats: WAV files, 8-bit, 8-32khz Sample Rate, mono.  
   * Asynchronous Playback: Allows code in main loop to run while audio playback occurs.  
   * Single timer operation: TIMER1 (Uno,Mega) or TIMER3,4 or 5 (Mega)
   * Complimentary output or dual speakers  
   * 2x Oversampling  
   * Supported devices: Arduino Uno, Nano, Mega, etc.  
   * More! See the [Advanced Features wiki page](https://github.com/TMRh20/TMRpcm/wiki/Advanced-Features)
   for additional features  
  
Files easily converted:  
Using iTunes:

    Click _> Edit > Preferences > Import Settings_
    Change the dropdown to _WAV Encoder_ and Setting: _Custom > 16.000kHz to 32kHz, 8-bit, Mono_
    Right click any file in iTunes, and select _"Create WAV Version"_

Using Audacity:
	
    Tracks > Stereo Track to Mono    
    Project Rate (HZ) > set to 32000, 22050, 16000 or 11025    
    File > Export > Save as type: Other uncompressed files > Options...    
    Select WAV, Unsigned 8 bit PCM    
    
Then copy file to SD card using computer

### **Connecting Everything**
The SD card should be connected as usual per https://www.arduino.cc/en/reference/SD 

The speaker can be connected directly to the output pins. On ATMega328 devices (Uno,Nano,etc) pins 9,10. See your board info for the main 16-bit timer/PWM output pins of your board.

If using an amplifier, consider that the output is a choppy digital PWM signal, not a smooth analog signal.

Note: Advice is often given NOT to connect a speaker directly to the digital pins on an Arduino. This is typically true, but TMRpcm is designed and tested with this configuration:
- By program design, there should never be a DC current flowing from PIN->GND or PIN->PIN, it will always be either a LOW-LOW state or a high frequency PWM signal.
- Standard rules of DC current (ie voltage/resistance=current) do NOT apply because this is more comparable to a HF A/C waveform
- The induced resistance (impedance) protects the MCU from an over-current situation

Disclaimer: Take advice at your own risk, I have never had a problem over years of usage with small 8-ohm speakers.

### **Functions**
    TMRpcm audio;
    audio.play("filename");    plays a file
    audio.play("filename",30); plays a file starting at 30 seconds into the track
    audio.play(F("filename")); plays a file storing the string in program memory instead of RAM
    audio.speakerPin = 11;     set to 5,6,11 or 46 for Mega, 9 for Uno, Nano, etc.
    audio.disable();           disables the timer on output pin and stops the music
    audio.stopPlayback();      stops the music, but leaves the timer running
    audio.isPlaying();         returns 1 if music playing, 0 if not
    audio.pause();             pauses/unpauses playback
    audio.quality(1);          Set 1 for 2x oversampling
    audio.volume(0);           1(up) or 0(down) to control volume
    audio.setVolume(0);        0 to 7. Set volume level
    audio.loop(1);             0 or 1. Can be changed during playback for full control of looping. 
  
 
**Contributed HowTo:** http://maxoffsky.com/maxoffsky-blog/how-to-play-wav-audio-files-with-arduino-uno-and-microsd-card/  
**Details at:** http://tmrh20.blogspot.com

### **Known Limitations**

    This library can be very processor intensive, and code execution during playback will be slower than normal
    Processing load can be reduced by using lower quality sounds encoded at a lower sample rate (8khz minimum)
    May interfere with other libraries that rely on interrupts. The isPlaying() disable() or noInterrupts()
     functions can be used to prevent parallel code execution.
    Volume control allows good range in volume control, but will distort if volume too high

### **Installation**

 1. Download current package: <https://github.com/TMRh20/TMRpcm/archive/master.zip>
 2. See the Manual Installation section at: <http://arduino.cc/en/Guide/Libraries>
  
### **Common Issues**

   See the video and comments at https://www.youtube.com/watch?v=LbqPgJe3Qd4 for some tips  
   Also http://www.instructables.com/id/Playing-Wave-file-using-arduino/ 

    1. Pop or click when playback is started or stopped:
       Ramps are built into the library to prevent popping when PWM is engaged, disabled, and between music tracks of the same
       sample rate. See the [Advanced Features wiki page](https://github.com/TMRh20/TMRpcm/wiki/Advanced-Features) for causes and fixes.
    2. Popping or clicking when music is playing
       If pops or clicks are heard during playback, it is most likely that buffer underruns are occurring or the volume is just too high.
       Ensure that #define SD_FULLSPEED is uncommented in pcmConfig.h. The value in #define buffSize 128 can be increased to provide
       additional memory for playback, which will reduce these issues. Audio can be encoded at a lower sample rate otherwise.
    3. The library works fine on its own, but doesn't play when library <name> is also included.
       The first thing to check is memory usage, since nothing will work if out of memory.         
       The library uses two timer pins by default. This may interfere with other libraries that use it. (pin 10 on Arduino Uno) 
       Disable the second pin by uncommenting the line #define DISABLE_SPEAKER2 in pcmConfig.h
       Boards like Uno only have one 16-bit timer. #define USE_TIMER2 can be uncommented in pcmConfig.h if TIMER1 is required for
       something else. See the [Advanced Features wiki page](https://github.com/TMRh20/TMRpcm/wiki/Advanced-Features)
    4. Error message when compiling: "Has no member named..." or "no matching function..."
       These errors usually indicate that commands are being run which are not available in the current configuration. Check the
       #defines in pcmConfig.h to ensure you are using the correct mode(s), and ensure your commands are correct.
    5. When using an SD card, (fat16/fat32) filenames are limited to 8 characters & the file extension.
 