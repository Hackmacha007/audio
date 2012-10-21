* **tmrpcm.play("filename");** plays a file
* **tmrpcm.speakerPin = 11;** set to 11 for Mega, 9 for Uno, Nano, etc
* **tmrpcm.volume(1);** raises or lowers the volume: 1 or -1
* **tmrpcm.disable();** disables the timer on output pin and stops the music
* **tmrpcm.stopPlayback();** stops the music, but leaves the timer running
* **tmrpcm.isPlaying();**  returns 1 if music playing, 0 if not
* **tmrpcm.pause();**  pauses/unpauses playback
* **tmrpcm.pwmMode = 1;** set to 1 for phase/frequency correct mode, 0 for fast pwm
* **tmrpcm.volume(0);** CHANGED from prev version, now uses either a 1(up) or 0(down)