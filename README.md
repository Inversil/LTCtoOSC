# LTCtoOSC + Touch Designer
This is a Max 4 live plugin with a companion touchdesigner project that you can use to more reliably time synchronize GPU accelerated video in Ableton Live on Windows.

This is based on a Max for Live LTC decoder (https://github.com/ArtScienceLab/M4L_LTC_Reader), which itself is based on code provided by David Butler / The Impersonal Stereo (https://github.com/impsnldavid/imp.ltc/tree/develop)

![Plugin 1](https://cdn.discordapp.com/attachments/422835897332137984/925476063746867220/unknown.png)

## How to use
1. Get an LTC audio file from https://elteesee.pehrhovey.net/. The sample rate should match the sample rate you use in your live project, so if you produce at 48k; you will get the best results also using a 48k audio file. Make sure the timecode audio starts at 1 Hour, which is the industry standard.
2. Make an audio track that has the LTC audio, and make sure it is un-warped.
3. Put this device on that audio track and pute the output audio

The M4L device will now keep track of the time in your project regardless of tempo modulations and send this time info to port 42169.

In order to play back video to this time, we use a free node-based vfx creation tool called touch designer, which you can get [here](https://derivative.ca/)
