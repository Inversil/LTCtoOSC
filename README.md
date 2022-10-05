# LTCtoOSC + Touch Designer Template
This is a **M4L plugin** with a **companion touchdesigner project** that you can use to more reliably time synchronize video in **Ableton Live** on **Windows**.

This is based on a Max for Live LTC decoder (https://github.com/ArtScienceLab/M4L_LTC_Reader), which itself is based on code provided by David Butler / The Impersonal Stereo (https://github.com/impsnldavid/imp.ltc/tree/develop) (no longer an active repository)

This guide looks kind of long, but that's mostly because it's made for people not familiar with any of this software.
In reality this will probably take less than 10 minutes to get up and running.

![Plugin 1](https://user-images.githubusercontent.com/32222093/194161871-3de73bb1-d140-4859-8495-787ec406ff00.png)

## So.. Why should I use this?
The original [Ableton Live Guide for using video on windows](https://help.ableton.com/hc/en-us/articles/209773125-Using-Video) recommends Windows users to download and install  Demuxer software that hasn't been updated since 2013. When this doesn't work, they ask users to convert videos into the right format using a piece of software from 2003. 

From a pro user standpoint, this workflow is frankly unacceptable. 
Becuase of the Ableton's neglect of video on windows over the years, this implementation has become highly unstable.
Touch Designer's video playback can be completely GPU accelerated and should run super quick alongside most Live projects.

Syncing Video using third party tools has been tricky because Ableton Live does not keep it's own timecode (this is, the playback time of the playhead inside the arrangement view in milliseconds). 
The Live Object model exposes play back time; but this is midi timecode, which becomes completely inaccurate when the project tempo is modulated. 

Then there are VST solutions like [VidplayVST](https://vidplayvst.com/index.htm), but even this cannot sync properly with a tempo modulated live timeline. 
The author of this plugin writes that this cannot be fixed "due to a limitation of the plugin-in interface provided by this daw", but it's actually because live does not expose SMPTE *at all*.

The setup in this github serves as a *complete replacement* workflow, which alleviates most of the video issues found inside live.

## Disadvantages of using this over the Default implementation.
- The audio of the video is not streamed from touch designer into Ableton Live, so you need to manually extract and import the video audio if you want to edit it in your project. (If you have an MP4 video, extracting audio should only take a few clicks in Audacity, Audition, or Resonic Pro.)
- You cannot export the video inside live.

## How to use

To start, click the code button at the top of this page and hit "download as zip" or [download the latest release](https://github.com/Inversil/LTCtoOSC-TouchDesigner/releases)

### Ableton LTC to OSC
1. Get an LTC audio file from https://elteesee.pehrhovey.net/. 
- The **sample rate should match the sample rate you use in your live project**, so if you produce at 48k; you will get the best results also using a 48k audio file. 
- Regardless of your video frame rate, the frame rate of the LTC should be **30FPS**
- The length of the LTC needs to at least exceed the length of your video. 
- Make sure the timecode audio **starts at 1 Hour**
- **A bit depth of 8** works perfectly and saves storage space.

Even though the video feed is synchronized with the max device at 30fps, you do not have to use a 30fps video. The touch designer project will automatically interpolate the OSC signal and automatically play back video at their intended frame rate and resolution.

2. Inside Live, create an audio track that has the LTC audio file in it. Make sure the sample is unwarped.
3. Put this device on that audio track turn down the track volume all the way

The M4L device will now keep track of the time in your project regardless of tempo modulations and send this time info to port 42169.

### OSC timecode for video playback using TouchDesigner

In order to play back video alongside this companion device, we use a free node-based vfx creation tool called touch designer, which you can get [here](https://derivative.ca/).
You will probably need to sign up for an account to use the trial version of the program, but it should last a lifetime.

1. Open "OSC TIMECODE VIDEO.toe" in TouchDesigner.
2. Click the big button that says "Click Here" and the video should go full screen.
3. Drag and drop a video file onto the drag and drop field within the plugin to load it up.

### That's it! You're good to go

You can now play the video from within live. The LTC audio clip represents the time of the video in your touchdesigner window.
If the video doesn't play, that might be because it's corrupt/invalid format; or because touch designer is paused. Make sure it is playing!

For advanced tweaking, you can change some settings in the "SETTINGS" CHOP.
- "framerate" should be the same as the framerate in your LTC file (keep at 30, it's hardcoded in M4L for now)
- "offset" offsets the video from the incoming OSC timecode, use this if your audio is not in sync with the video.
- "error" changes the error margin for the internal framerate counter. If the difference in time between the OSC values and the internal counter go above this threshold, the internal counter snaps to the current OSC value.

~~If you're unfamiliar with touch designer - here is a visual guide about which nodes you should interact with to get what you need:~~
**This image is outdated and I need to update it.**

![Touch Designer Guide](https://cdn.discordapp.com/attachments/202817364264222720/925547803588063262/eeee.png)

Once set up the video synchronization is super fast and consistent. You can even use the touch designer video output as your webcam, and stream live and the video feed over discord simultaniously.

## TODO

Maybe instead of using OSC and Touch Designer, perhaps in the future we can make M4L play video alongside the LTC decoder directly.. but that's a project for someone else... or I'll pick it up...... one day........

-- made by Inverted Silence & Vlams
