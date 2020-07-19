---
layout: post
title:  Track sleep using an IP camera with infrared
author: nickstugr
---

I live alone with my cat, and often wake up on strange angles with her sleeping on the other side of my legs than she was when I went to sleep.

I've been wondering what the quality of my sleep is like, and how much sharing a bed with a cat (or my partner) affects it, so I borrowed an IP camera with infrared from a friend.

This camera is _old_ and so can't do motion detection or write footage to a remote location (only SD card).

Below is the process by which I am recording the footage via an RTSP stream, analysing it for motion, then speeding it up so that I can watch a 2 minute summary.

Some of my notes after a few nights:

- My cat cleans herself _a lot_
- My partner grinds her teeth in her sleep
- I might have [PLMS](http://sleepeducation.org/sleep-disorders-by-category/sleep-movement-disorders/periodic-limb-movements/overview-facts)
- I am going to try locking my cat out for a few nights to see the difference this makes
- Play with `dvr-scan -roi` to specify region of interest to just detect motion of just me (head or feet), or cat
- Haven't done anything with audio yet
- Try graphing motion event timeline using timestamps outputted by `dvr-scan`

Example output (roughly cropped to just my cat, you don't need to watch me sleep ya weirdo):
{% include youtube.html id='ZCUUOapwfEI' %}
<br />
# Prerequisites

You will need a copy of [ffmpeg](https://ffmpeg.org/) and [dvr-scan](https://dvr-scan.readthedocs.io/) (which you'll also need Python for). I'm running this on Windows and have my utils in my `%path%`

# Before you go to sleep

1. Change working dir to a new folder for each night
1. Start capturing RTSP stream into 5 minute long video files split using clocktime (so 1:00-1:05 etc). Each file is labeled `capture-xxxxx.mp4` where xxxxx is an incrementing 5 digit number.

    ```
    ffmpeg -i rtsp://user:pass@ip:port/11 -c:v copy -c:a mp2 -ar 44100 -aq 0 -ac 2 -map 0 -f segment -segment_atclocktime 1 -segment_time 300 -segment_format mp4 "capture-%05d.mp4"
    ```

# The next morning

1. Manually delete videos at the start and end that aren't relevant (like reading in bed, getting out of bed etc)
1. Generate a file called `scan.txt` that contains the command for the next step that includes all remaining videos as parameters
   
    ```
    powershell "'dvr-scan', (((Get-ChildItem capture*.mp4).name) -join ' -i ') -join ' -i ' | Set-Content scan.txt"
    ```
    Example `scan.txt` content:
    ```
    dvr-scan -i capture-00009.mp4 -i capture-00010.mp4 -i capture-00011.mp4
    ```
1. Open `scan.txt` and copy the contents into your clipboard
1. Run `dvr-scan` command using the contents of your clipboard.\
    This outputs one file per motion event, auto-naming all files using the filename of the first provided input file with `DSME_xxxx` appended on the end eg. `capture-00009.DSME_0000.avi`, `capture-00009.DSME_0001.avi` etc.\
    _NB. dvr-scan outputs .avi and my attempts at using -c to specify a different codec were unsuccessful_
1. Manually delete videos of motion at the start and end that aren't relevant
1. Output the filenames of videos of motion in the format that ffmpeg wants into a file called `files.txt`

    ```
    powershell "(Get-ChildItem *DSME* | ForEach-Object { 'file ''{0}''' -f $_.name }) | Set-Content files.txt"
    ```
    Example `files.txt` content:
    ```
    file 'capture-00009.DSME_0000.avi'
    file 'capture-00009.DSME_0001.avi'
    file 'capture-00009.DSME_0002.avi'
    ```
1. Run `ffmpeg` to stitch the videos together at 20x speed (for 10x change `0.05` to `0.1`)

    ```
    ffmpeg -f concat -safe 0 -i files.txt -filter:v "setpts=0.05*PTS" output.avi
    ```
1. Watch `output.avi` to see how badly you sleep