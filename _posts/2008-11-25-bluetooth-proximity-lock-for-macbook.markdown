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
<p><a title="http:&#47;&#47;web.mac.com&#47;jhollington&#47;technocrat&#47;The_Technocrat&#47;Entries&#47;2007&#47;3&#47;18_Bluetooth_Proximity_Detection_on_OS_X.html" href="http:&#47;&#47;web.mac.com&#47;jhollington&#47;technocrat&#47;The_Technocrat&#47;Entries&#47;2007&#47;3&#47;18_Bluetooth_Proximity_Detection_on_OS_X.html">http:&#47;&#47;web.mac.com&#47;jhollington&#47;technocrat&#47;The_Technocrat&#47;Entries&#47;2007&#47;3&#47;18_Bluetooth_Proximity_Detection_on_OS_X.html<&#47;a></p>
<p>Pointed me to</p>
<p><a title="http:&#47;&#47;www.apple.com&#47;downloads&#47;macosx&#47;system_disk_utilities&#47;proximity.html" href="http:&#47;&#47;www.apple.com&#47;downloads&#47;macosx&#47;system_disk_utilities&#47;proximity.html">http:&#47;&#47;www.apple.com&#47;downloads&#47;macosx&#47;system_disk_utilities&#47;proximity.html<&#47;a></p>
<p>he also includes a script to automate syncing your phone book and such too</p>
<p>Another utility in my searching, is called JackSMS</p>
<p><a title="http:&#47;&#47;www.macupdate.com&#47;info.php&#47;id&#47;21860" href="http:&#47;&#47;www.macupdate.com&#47;info.php&#47;id&#47;21860">http:&#47;&#47;www.macupdate.com&#47;info.php&#47;id&#47;21860<&#47;a></p>
<p>JackSMS does some cool stuff like, say someone tries to access your laptop while you're not there - the built in isight camera will take their picture and email it then, so I wanted to use JackSMS as well.</p>
<p>Proximity allows you to set scripts for when you arrive or when you leave, so here are the applescripts I used:</p>
<p>lock.scpt:</p>
<pre lang="applescript">﻿tell application "JackSMS" to set lock screen to true<&#47;pre><br />
unlock.scpt:</p>
<pre lang="applescript">﻿tell application "JackSMS" to set lock screen to false<&#47;pre><br />
courtesy of: <a title="http:&#47;&#47;www.macosxhints.com&#47;article.php?story=2006061914363693" href="http:&#47;&#47;www.macosxhints.com&#47;article.php?story=2006061914363693">http:&#47;&#47;www.macosxhints.com&#47;article.php?story=2006061914363693<&#47;a></p>
<p>update: a better proximity script, copied below, courtesy of:<br />
<a title="http:&#47;&#47;pixelignition.net&#47;better-proximity-applescript" href="http:&#47;&#47;pixelignition.net&#47;better-proximity-applescript"> http:&#47;&#47;pixelignition.net&#47;better-proximity-applescript<&#47;a><br />
lock.scpt:<br />
<spoiler></p>
<pre lang="applescript">
global okflag<br />
set okflag to false<br />
set front_app to (path to frontmost application as Unicode text) -- So we can switch back to this after running the fade</p>
<p>-- check if iTunes is running<br />
tell application "System Events"<br />
        if process "iTunes" exists then<br />
                set okflag to true --iTunes is running<br />
        end if<br />
end tell</p>
<p>if okflag is true then<br />
        try<br />
                tell application "iTunes"<br />
                        set currentvolume to the sound volume<br />
                        if (player state is playing) then<br />
                                repeat<br />
                                        --Fade down<br />
                                        repeat with i from currentvolume to 0 by -1 --try by -4 on slower Macs<br />
                                                set the sound volume to i<br />
                                                delay 0.01 -- Adjust this to change fadeout duration (delete this line on slower Macs)<br />
                                        end repeat<br />
                                        pause<br />
                                        --Restore original volume<br />
                                        set the sound volume to currentvolume<br />
                                        exit repeat<br />
                                end repeat<br />
                                set comment of current track to "Proximity Paused"<br />
                        end if<br />
                end tell<br />
                tell application front_app<br />
                        activate<br />
                end tell<br />
        on error<br />
                beep<br />
        end try<br />
end if</p>
<p>tell application "JackSMS" to set jack status to "on"<br />
delay 1<br />
tell application "DeskShade"<br />
        lock<br />
end tell<br />
end run<br />
<&#47;pre><br />
<&#47;spoiler><br />
unlock.scpt:<br />
<spoiler></p>
<pre lang="applescript">
tell application "ScreenSaverEngine" to quit<br />
tell application "DeskShade"<br />
        unlock<br />
        quit<br />
end tell<br />
tell application "JackSMS"<br />
        quit<br />
end tell</p>
<p>global okflag<br />
set okflag to false<br />
set front_app to (path to frontmost application as Unicode text) -- So we can switch back to this after running the fade</p>
<p>-- check if iTunes is running<br />
tell application "System Events"<br />
        if process "iTunes" exists then<br />
                set okflag to true --iTunes is running<br />
        end if<br />
end tell</p>
<p>if okflag is true then<br />
        try<br />
                tell application "iTunes"<br />
                        set currentvolume to the sound volume<br />
                        if comment of current track is "Proximity Paused" then<br />
                                set comment of current track to ""<br />
                                play<br />
                                repeat with j from 0 to currentvolume by 2 --try by 4 on slower Macs<br />
                                        set the sound volume to j<br />
                                end repeat<br />
                        end if<br />
                end tell<br />
                tell application front_app<br />
                        activate<br />
                end tell<br />
        on error<br />
                beep<br />
        end try<br />
end if<br />
<&#47;pre><br />
<&#47;spoiler></p>
