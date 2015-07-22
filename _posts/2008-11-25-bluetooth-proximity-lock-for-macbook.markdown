---
layout: post
status: publish
published: true
title: Bluetooth Proximity Lock for Macbook
author:
  display_name: Chris Haynie
  login: admin
  email: chris@chrishaynie.com
  url: http://www.chrishaynie.com/
author_login: admin
author_email: chris@chrishaynie.com
author_url: http://www.chrishaynie.com/
wordpress_id: 26
wordpress_url: http://www.chrishaynie.com/?p=26
date: '2008-11-25 18:34:43 +0000'
date_gmt: '2008-11-26 00:34:43 +0000'
categories:
- code
tags:
- bluetooth
- mac
- osx
- hack
- script
comments:
- id: 18
  author: lenPremsges
  author_email: enelsbiossign@gmail.com
  author_url: http://iloveyandex.com
  date: '2009-03-12 14:21:28 +0000'
  date_gmt: '2009-03-12 20:21:28 +0000'
  content: Thank you!
- id: 975
  author: uber
  author_email: ryan@blankbmx.com
  author_url: http://ryanuber.com
  date: '2010-05-08 01:03:35 +0000'
  date_gmt: '2010-05-08 07:03:35 +0000'
  content: werd
---
<p>I wanted to have my macbook automatically lock and unlock based on if I am in the room or not automatically. Best way to do that I figured would be bluetooth, since I always have my phone on me, and it has bluetooth. From there I proceeded to google.</p>
<p><a title="http://web.mac.com/jhollington/technocrat/The_Technocrat/Entries/2007/3/18_Bluetooth_Proximity_Detection_on_OS_X.html" href="http://web.mac.com/jhollington/technocrat/The_Technocrat/Entries/2007/3/18_Bluetooth_Proximity_Detection_on_OS_X.html">http://web.mac.com/jhollington/technocrat/The_Technocrat/Entries/2007/3/18_Bluetooth_Proximity_Detection_on_OS_X.html</a></p>
<p>Pointed me to</p>
<p><a title="http://www.apple.com/downloads/macosx/system_disk_utilities/proximity.html" href="http://www.apple.com/downloads/macosx/system_disk_utilities/proximity.html">http://www.apple.com/downloads/macosx/system_disk_utilities/proximity.html</a></p>
<p>he also includes a script to automate syncing your phone book and such too</p>
<p>Another utility in my searching, is called JackSMS</p>
<p><a title="http://www.macupdate.com/info.php/id/21860" href="http://www.macupdate.com/info.php/id/21860">http://www.macupdate.com/info.php/id/21860</a></p>
<p>JackSMS does some cool stuff like, say someone tries to access your laptop while you're not there - the built in isight camera will take their picture and email it then, so I wanted to use JackSMS as well.</p>
<p>Proximity allows you to set scripts for when you arrive or when you leave, so here are the applescripts I used:</p>
<p>lock.scpt:</p>
<pre lang="applescript">﻿tell application "JackSMS" to set lock screen to true</pre>
unlock.scpt:</p>
<pre lang="applescript">﻿tell application "JackSMS" to set lock screen to false</pre>
courtesy of: <a title="http://www.macosxhints.com/article.php?story=2006061914363693" href="http://www.macosxhints.com/article.php?story=2006061914363693">http://www.macosxhints.com/article.php?story=2006061914363693</a></p>
<p>update: a better proximity script, copied below, courtesy of:
<a title="http://pixelignition.net/better-proximity-applescript" href="http://pixelignition.net/better-proximity-applescript"> http://pixelignition.net/better-proximity-applescript</a>
lock.scpt:
</p>
<pre lang="applescript">
global okflag
set okflag to false
set front_app to (path to frontmost application as Unicode text) -- So we can switch back to this after running the fade
-- check if iTunes is running
tell application "System Events"
        if process "iTunes" exists then
                set okflag to true --iTunes is running
        end if
end tell
if okflag is true then
        try
                tell application "iTunes"
                        set currentvolume to the sound volume
                        if (player state is playing) then
                                repeat
                                        --Fade down
                                        repeat with i from currentvolume to 0 by -1 --try by -4 on slower Macs
                                                set the sound volume to i
                                                delay 0.01 -- Adjust this to change fadeout duration (delete this line on slower Macs)
                                        end repeat
                                        pause
                                        --Restore original volume
                                        set the sound volume to currentvolume
                                        exit repeat
                                end repeat
                                set comment of current track to "Proximity Paused"
                        end if
                end tell
                tell application front_app
                        activate
                end tell
        on error
                beep
        end try
end if
tell application "JackSMS" to set jack status to "on"
delay 1
tell application "DeskShade"
        lock
end tell
end run
</pre>
unlock.scpt:
</p>
<pre lang="applescript">
tell application "ScreenSaverEngine" to quit
tell application "DeskShade"
        unlock
        quit
end tell
tell application "JackSMS"
        quit
end tell
global okflag
set okflag to false
set front_app to (path to frontmost application as Unicode text) -- So we can switch back to this after running the fade</p>
- check if iTunes is running
tell application "System Events"
        if process "iTunes" exists then
                set okflag to true --iTunes is running
        end if
end tell
if okflag is true then
        try
                tell application "iTunes"
                        set currentvolume to the sound volume
                        if comment of current track is "Proximity Paused" then
                                set comment of current track to ""
                                play
                                repeat with j from 0 to currentvolume by 2 --try by 4 on slower Macs
                                        set the sound volume to j
                                end repeat
                        end if
                end tell
                tell application front_app
                        activate
                end tell
        on error
                beep
        end try
end if
</pre>
