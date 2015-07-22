---
layout: post
status: publish
published: true
title: Clipboard Snippeteering
author:
  display_name: Chris Haynie
  login: admin
  email: chris@chrishaynie.com
  url: http://www.chrishaynie.com/
author_login: admin
author_email: chris@chrishaynie.com
author_url: http://www.chrishaynie.com/
wordpress_id: 114
wordpress_url: http://chrishaynie.com/?p=114
date: '2010-08-26 17:36:48 +0000'
date_gmt: '2010-08-26 23:36:48 +0000'
categories:
- code
tags:
- code
- bash
- linux
comments: []
---
<p>I work in IT and frequently you may need to send someone a screenshot. Maybe its the configuration screen you're looking at or the output of a command.  As many of you know this procedure usually involves hitting print-screen on your keyboard, pasting it into a photo editor like ms-paint, saving it as a jpeg and what... do you send it over IM with a direct connection? email it to them? I wanted a solution where I can easily take a screenshot of just an area of the screen, or the entire screen if I choose and have it automatically upload the screenshot to a webserver, and allow me to paste the URL to the screenshot instantly.  Ideally, I would have it even put the URL IN my clipboard as well, as it turns out this is all exactly what I accomplished.</p>
<p><a href="http://chrishaynie.com/wp-content/uploads/2010/08/66b1c38661258dc3b201c52f0035956b.jpg"><img src="http://http://b819c688147e0961fe05-3686d6c970b79fee78310b4e70aec21e.r18.cf1.rackcdn.com/wp-content/uploads/2010/08/66b1c38661258dc3b201c52f0035956b-300x183.jpg" alt="Screenshot Utility in CompizConfig Settings Manager" title="Screenshot Utility in CompizConfig Settings Manager" width="300" height="183" class="alignright size-medium wp-image-117" /></a></p>
<p>I run Ubuntu on my workstation with compiz. You will need to install compizconfig-settings-manager, xsel, and ImageMagick for this all to work. Below is the script to perform the screenshot auto-magic, when combined with the compiz screenshot plugin.  I press the windows key on my keyboard and then click+drag I have an instant screenshot. </p>
<pre lang="bash">
FNAME="/home/chrisha/Documents/snippets/$(date +%s|md5sum|awk '{print $1}').jpg"
/usr/bin/convert -bordercolor white -border 5 $1 $1
/usr/bin/convert -bordercolor black -border 1 $1 $1
/usr/bin/convert -quality 75 $1 $FNAME
scp $FNAME slice:public_html/dev.chrishaynie.com/html/snippets/
echo http://slice.chrishaynie.com/snippets/$(basename $FNAME)|xsel -i
rm -f $1
</pre>
<p>Now then, lets say you want to send a large block of text - maybe its a snippet of code, maybe its the output of a command. Taking a screenshot of text isn't verfy efficient, which we can agree. Traditional solutions have been just pasting it into the chat window and spamming the receiver, using sites like pastebin.com, but now your text is publicly available, maybe you're wanting to send a snippet of some proprietary code or personal conversation.</p>
<p>My buddy Tim over at http://h1tman.com/ advanced my above screenshot auto-upload to include text excerpts, which get uploaded as plain-text.  You bind his script to a keyboard shortcut and from there, you select the text, hit the keyboard shortcut and you're off and running. You can read his full howto in depth here http://www.h1tman.com/2010/08/clipboard-hacks-social-copypasta</p>
<p>Now we get to the good part, I've combined both my original "screenshotuploader" with Tim's "textshot" into what I'm going to call snippets. Use the same script in the screenshot plugin and in your keyboard shortcut for textshots, use the same url and same folder to store them all, in one combined script below.</p>

<pre lang="bash">
#!/bin/bash
DIR="/home/chrisha/Documents/snippets/"
HOST="slice.chrishaynie.com"
HASH="$(date +%s|md5sum|awk '{print $1}')"
FNAME="$DIR$HASH"
if [ -z $1 ]
then
	FNAME="$FNAME.txt"<br />
	xsel>$FNAME<br />
else
	FNAME="$FNAME.jpg"<br />
	/usr/bin/convert -bordercolor white -border 5 $1 $1<br />
	/usr/bin/convert -bordercolor black -border 1 $1 $1<br />
	/usr/bin/convert -quality 75 $1 $FNAME<br />
	rm -f $1<br />
fi
scp $FNAME $HOST:public_html/dev.chrishaynie.com/html/snippets/
echo http://slice.chrishaynie.com/snippets/$(basename $FNAME)|xsel -i
</pre>
