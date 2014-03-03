Advanced features of the library

This library was intended to be a simple and user friendly wav audio player using standard Arduino libraries, playing bare-bones standard format WAV files.Many of the extra features have been added due to user request, and are enabled optionally in pcmConfig.h to preserve the out of the box simplicity initially intended.

**Code/Updates:** https://github.com/TMRh20/TMRpcm  
**Wiki:** https://github.com/TMRh20/TMRpcm/wiki  
**Blog:** https://tmrh20.blogspot.com/  
  
Most of the extra features require more memory, more program space, and in some cases, more processing power for playback
Some of them are still being fine tuned. Please keep this in mind when enabling these features.  
See pcmConfig.h to configure the following options:  

### **User Defines**
    The following options are configurable in pcmConfig.h:  
    #define buffSize 128     Control the size of the 2 buffers (4 in MULTI mode)  
    #define DISABLE_SPEAKER2 Disables default second speaker pin for compatibility with other libs. (pin 10 on Uno)
    #define ENABLE_MULTI     Enable multi track playback mode (Default single timer)  
    #define STEREO_OR_16BIT  Enable playback of stereo or 16-bit files  
        #define MODE2        Enable dual timer multi track playback mode. Not available with TIMER2  
    #define SDFAT            SdFat library uses less prog space and memory. See SDFAT example included with lib.  
    #define HANDLE_TAGS	     Skip tags for WAV files that contain metadata  
    #define USE_TIMER2	     Use 8-bit TIMER2 instead of 16-bit timer(s)  
    #define rampMega	     Force manual selection of PWM enable/disable ramping method.  
    #define ENABLE_RF        Enable streaming of audio over radio link (NRF24L01+)      

### **Speaker2/Complimentary Mode**
This library outputs WAV data to two timer pins by default, although only one is fully enabled by default.
    
    The complimentary pin must be set as an output to enable:  
    eg:  
    Arduino Uno (Single Output): audio.speakerPin = 9;  
    Arduino Uno (Complimentary Output): audio.speakerPin = 9; pinMode(10,OUTPUT);  
      
    Uncomment the line #define DISABLE_SPEAKER2 in pcmConfig.h to fully disable the second pin.

### **Simple Digital WAV File generation**
These features will generate standards compliant WAV files. Raw data from analog inputs or other sensors or information sources
can be written to the file to generate digital audio that can be played on any device that supports WAV files, or easily converted
to other formats.  
Note: More wav formats etc. will be added to this eventually.  

    Syntax: audio.createWavTemplate(<Song Name>,<Sample Rate>);
            audio.finalizeWavTemplate(<Song Name>);
    
    Usage: See the example included with the library. Generate the template file, then write data to it starting at 44 bytes.
    	   Use the finalizeWavTemplate comand to add the file size details to the file prior to playback.. 
    
    Notes: If the specified file exists, it will be overwritten for creation, but only updated for finalization.

### **Multi Mode**
 Multi Mode enables simultaneous playback of two audio tracks.

    Functions:
    The functions in multi mode are slightly different than standard mode:
    audio.speakerPin2 = 5;        Same functionality as standard mode, only used with 4 pin output
    audio.play("sound.wav");      Play a file on output 0
    audio.play("sound.wav",30,1); Play a file starting at 30 seconds, on output 1
    audio.play("sound.wav",0);    Play(WaveFile,Output 0 or 1) Defined by speakerpin and speakerpin2
    audio.stopPlayback(0);     Can include output number 0 or 1
    audio.isPlaying(0);        Can include output number 0 or 1

    Modes:
    Default: Uses the same timer and pins as regular mode with complimentary output  
    MODE2: Uses two 16-bit timers and up to 4 pins  

    Usage:
    16kHz-20kHz sample rate recommended
    Memory buffer can be increased for improved performance
    Sounds played together should be of the same sample rate
    Uncomment the define in pcmConfig.h to enable multi mode
    The audio.speakerPin2 variable must be set in 4-pin mode to select the additional timer/pins to be used.

    Modes Simplified:
    
    Normal mode ( 2 pins, one track)	    ( 1 or 2 speakers)
    Normal mode Stereo ( 2 pins, one track) ( 2 Speakers, non complimentary (pin to ground) )
    Normal MODE2 Stereo (4 pins, one track) ( 4 Speakers OR Complimentary output (pin-to-pin) with 2 Speakers)
    
    Multi mode ( 2 pins, two tracks )	    ( 1 or 2 speakers)
    Multi mode Stereo ( 4 pins, 2 tracks)   ( 2 or 4 speakers, non complimentary)  
    Multi MODE2 (4 pins, 2 tracks)          ( 2 complimentary, or 4 non complimentary)
  
    Note: All 4-pin modes require a board with two or more 16-bit timers.

### **Stereo and 16-Bit Playback**
   These modes require additional resources and processing power since double the data must be read from the SD card.
    Mono tracks can be played in Stereo mode, not so in reverse.
    The operation can be a bit difficult to explain, but here goes:
    
    Normal Mode: 
    #define STEREO_OR_16BIT
    In this mode, stereo and 16-bit files are treated the same, with the first byte being read into one
    output, and the second byte into the other output. This will produce a stereo output for two speakers,
    connected from speaker pin(s) to ground, or a single 16-bit output using a resistor ladder.
    #define MODE2
    In Normal Mode, MODE2 will enable output STEREO or 16BIT audio using two additional timer pins. The timer and pins are
    specified by the speakerPin2 variable. Complimentary timer pins need to be set as output pins manually.
    
    In MULTI Mode:
    #define STEREO_OR_16BIT
    Enabling this option along with MULTI mode will allow two stereo or 16-bit tracks to be played on separate
    timer pins. This provides output for four speakers, connected from speaker pin(s) to ground or a single
    16-bit output for each track.
    #define MODE2
    Enabling this option with MULTI mode and STEREO_OR_16BIT both enabled will make no difference    
    
    See MULTI mode above. 
   
### **SDFAT Usage**

The SDFAT library can be used to reduce memory and program space usage, and increase performance. The files must be
included in the sketch. See the SDFAT example included with the library.


### **Metadata (ID3v2.3 and LIST tags)**
 Functions have been added to read Song, Artist, and Album info from ID3v2.3 and LIST tags in WAV files
 Note: Using these functions will increase program space and memory usage.
    
    Functions:
    listInfo - Reads LIST tags into a character buffer, returns the length of the tag
    id3Info -  Reads ID3 tags into a character buffer, returns the length of the tag
    getInfo -  Searches for both tags and reads into a character buffer, returns the length of the tag. ID3 is searched first.
    
    Tag To Request: 0 = Song Name, 1 = Artist Name, 2 = Album Name
    
    Syntax: byte length = audio.listInfo(<Song Name>,<buffer[]>,<tagToRequest>);
    	    byte length = audio.id3Info(<Song Name>,<buffer[]>,<tagToRequest>);
    	    byte length = audio.getInfo(<Song Name>,<buffer[]>,<tagToRequest>);
###
###
    Examples:
    1. Search LIST info only and print song name over serial:
    	char info[32];
    	audio.listInfo("song.wav",info,0);
    	Serial.print(info);Serial.println(":"); 
    
    2. Search ID3v2.3 and LIST info and print song, artist and album info over serial   	
    	char info[40];
    	char* titles[3] = {"Now Playing: ", "by: ", "Album: "};
    	
    	for(int i=0; i<3; i++){
    	  if(audio.getInfo("song1.wav",info,i) > 0){
    	  	Serial.print(titles[i]);
    	  	Serial.println(info);
    	  }
    	}
    	audio.play("song1.wav");    	


### **TIMER2 Usage**

Mainly for use with Uno, Nano etc with only one 16-bit timer. TIMER2 can be used for audio playback when TIMER1 is required for other purposes.  
SpeakerPins - Pin3 only on Uno,Nano,etc  
To enable usage of 8-bit TIMER2, uncomment #define USE_TIMER2 in the user defines section  

    Notes:
    1. This is usually not the best solution.
    2. Playback speed will be slightly different than with 16-bit timers.
    3. TIMER2 playback supports non-standard sample rates:
       31.4 kHz, 23.5 kHz, and 15.7 kHz    
    4. Oversampling is on by default and cannot be changed in this mode
    5. 24-32kHz sample rate with 128buffer size is recommended
    
### **PWM and rampMega option**    	
Trying to reduce all of the popping noises created from PWM has been a struggle, but I've identified and addressed 4 main sources:  

    Problem sources:      
    1. Enabling PWM/Timers on Arduino
    2. Disabling PWM/Timers on Arduino
    3. Difference in values between tracks
    4. Parsing of non-audio data
###
###  
    Solutions:  
    1 and 2. In testing on Arduino Duemilanove and Mega boards, I discovered a need for different
    ramping methods on startup and disabling. Hopefully this addresses the issue on most other boards.  
    3.The ramping code between tracks is solid. If playing tracks of different sample rates, use the
    disable() function to turn the timers off between changes. Also see #4.  
    4.An option HANDLE_TAGS has been included in pcmConfig.h to allow proper playback of wav files
    with included metadata (ID3 or LIST info)  
  
### **Audio streaming over NRF24L01+ radio link**
This optional receiver library is included to enable PCM/WAV streaming
using NRF24L01+ radio modules. The code is proof of concept only.

    Requirements: 
    1 Arduino Mega
    1 or more Arduino (Uno,Mega,Nano,etc)
    2 or more NRF24L01+ Radio Modules + RF24 Arduino Library
    1 SD card for controller
###
    Step 1: Connect everything and test it all individually

    To enable receiver:
    1. Copy the 'pcmRX' folder to your Arduino library folder.
    2. Open and run the RX example under the examples folder
    
    To enable controller:
    1. Open pcmConfig.h and uncomment the #define ENABLE_RF line
    2. Open up and run one of the two example files in streamingExamples folder

Note: The ENABLE_RF line MUST be commented if not using RF features

The transmitter/streamer was tested using an Arduino Mega, and uses interrupt 5 (Pin 18) connected to the radio module interrupt pin. It seems that this module must be running on 5v for the interrupt to function.
         
