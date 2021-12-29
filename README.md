# LTCtoOSC + Touch Designer Template
This is a **M4L plugin** with a **companion touchdesigner project** that you can use to more reliably time synchronize video in **Ableton Live** on **Windows**.

This is based on a Max for Live LTC decoder (https://github.com/ArtScienceLab/M4L_LTC_Reader), which itself is based on code provided by David Butler / The Impersonal Stereo (https://github.com/impsnldavid/imp.ltc/tree/develop)

![Plugin 1](https://cdn.discordapp.com/attachments/422835897332137984/925476063746867220/unknown.png)

## So.. Why should I use this?
The original [Ableton Live Guide for using video on windows](https://help.ableton.com/hc/en-us/articles/209773125-Using-Video) recommends Windows users to download and install  Demuxer software that hasn't been updated since 2013. When this doesn't work, they ask users to convert videos into the right format using a piece of software from 2003. 

From a pro user standpoint, this workflow is frankly unacceptable. 
Becuase of the Ableton's neglect of video on windows over the years, this implementation has become highly unstable.
Touch Designer's video playback can be completely GPU accelerated and should run super quick alongside most Live projects.

Syncing Video using third party tools has been tricky because Ableton Live does not keep it's own timecode (this is, the playback time of the playhead inside the arrangement view in milliseconds). 
The Live Object model exposes play back time; but this is midi timecode, which becomes completely inaccurate when the project tempo is modulated. 

Then there are VST solutions like [VidplayVST](https://vidplayvst.com/index.htm), but even this cannot sync properly with a tempo modulated live timeline. 
The author of this plugin writes that this cannot be fixed "due to a limitation of the plugin-in interface provided by this daw", but it's actually because live does not keep SMPTE internally *at all*.

The setup in this github serves to alleviate all of these issues.

## Disadvantages of using this over the Default implementation.
- The audio of the video is not streamed from touch designer into Ableton Live, so you need to manually extract and import the video audio if you want to edit it in your project.
- You cannot cut/edit or export the video inside live.
- You need to save the touch designer project alongside the live project, and then open it manually when you get to work.

## How to use
### Ableton LTC to OSC
1. Get an LTC audio file from https://elteesee.pehrhovey.net/. The sample rate should match the sample rate you use in your live project, so if you produce at 48k; you will get the best results also using a 48k audio file. The length of the LTC audio needs to at least exceed the length of your video. Other than that, make sure the timecode audio starts at 1 Hour, which is the industry standard. A bit depth of 8 works perfectly and saves storage space.
Even though the video feed is synchronized at 30fps, it does not need to be a 30fps video. The touch designer project will automatically interpolate the OSC signal and support videos with any frame rate.

2. Inside Live, create an audio track that has the LTC audio file in it. Make sure the sample is unwarped.
3. Put this device on that audio track and turn down the track volume to -inf.

The M4L device will now keep track of the time in your project regardless of tempo modulations and send this time info to port 42169.

In order to play back video to this time, we use a free node-based vfx creation tool called touch designer, which you can get [here](https://derivative.ca/)

### OSC timecode for video playback using TouchDesigner
1. When you load up the TouchDesigner project the "oscin1" CHOP / node should automatically be activated, if not you can turn it on by clicking on it and toggling the "Active" button.
2. Load your video into the "VIDEO" module TOP, make sure not to change any of the other settings.
3. To open your video in a separate window, go to the "window1" OP and click "Open as Separate window" in the popup menu.

You can change some settings in the "SETTINGS" CHOP.
- "framerate" should be the same as the framerate in your LTC file (keep at 30, it's hardcoded in M4L for now)
- "offset" offsets the video from the incoming OSC timecode, use this if your audio is not in sync with the video.
- "error" changes the error margin for the internal framerate counter. If the difference in time between the OSC values and the internal counter go above this threshold, the internal counter snaps to the current OSC value.

If you're unfamiliar with touch designer - here is a visual guide about which notes you should interact with to get what you need:

![Touch Designer Guide](https://cdn.discordapp.com/attachments/202817364264222720/925543265757970462/touch_desinger.png)

Once set up the video synchronization is super fast and consistent. You can even use the touch designer video output as your webcam, and stream live and the video feed over discord simultaniously.

## TODO

Maybe instead of requiring people to install touch designer, we can make a version that can run in Touch Player, which would be more light weight.
Maybe instead of using OSC and Touch Designer, perhaps in the future we can make M4L play video alongside the LTC decoder directly.. but that's a project for someone else... or I'll pick it up...... one day........
