Advanced features of the library

### **User Defines**
    The following options are configurable in TMRpcm.h:
    #define buffSize 128   Control the size of the 2 buffers (4 in MULTI mode)
    #define ENABLE_MULTI   Enable multi track playback mode (Default single timer)
        #define MODE2      Enable dual timer multi track playback mode
    #define ENABLE_RF      Enable streaming of audio over radio link (NRF24L01+)

### **Multi Mode**
 Multi Mode is new and still being tested. It enables simultaneous playback of two audio tracks.

    Functions:
    The functions in multi mode are slightly different than standard mode:
    wav.speakerPin2 = 5;     Same functionality as standard mode
    wav.play("sound.wav",0); play(WaveFile,Output 0 or 1) Defined by speakerpin and speakerpin2
    wav.stopPlayback(0);     Can include output number 0 or 1
    wav.isPlaying(0);        Can include output number 0 or 1

    Modes:
    Default: Uses the same timer and pins as regular mode with complimentary output
    MODE2: Uses two 16-bit timers and up to 4 pins

    Usage:
    16kHz-20kHz sample rate recommended
    Memory buffer can be increased for improved performance
    Sounds played together should be of the same sample rate
    Uncomment the define in TMRpcm.h to enable multi mode

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
    1. Open TMRpcm.h and uncomment the #define ENABLE_RF line
    2. Open up and run one of the two example files in streamingExamples folder

Note: The ENABLE_RF line MUST be commented if not using RF features

The transmitter/streamer was tested using an Arduino Mega, and uses interrupt 5 (Pin 18) connected to the radio module interrupt pin.
         
