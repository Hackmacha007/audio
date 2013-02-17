* **tmrpcm.play("filename");** plays a file
* **tmrpcm.speakerPin = 11;** set to 5,6,11 or 46 for Mega, 9 for Uno, Nano, etc
* **tmrpcm.disable();** disables the timer on output pin and stops the music
* **tmrpcm.stopPlayback();** stops the music, but leaves the timer running
* **tmrpcm.isPlaying();**  returns 1 if music playing, 0 if not
* **tmrpcm.pause();**  pauses/unpauses playback
* **tmrpcm.pwmMode = 1;** set to 1 for phase/frequency correct mode, 0 for fast pwm
* **tmrpcm.volume(0);** CHANGED from prev version, now uses either a 1(up) or 0(down)