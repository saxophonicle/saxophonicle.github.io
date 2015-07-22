---
layout: post
status: publish
published: true
title: Python Threading
author:
  display_name: Chris Haynie
  login: admin
  email: chris@chrishaynie.com
  url: http://www.chrishaynie.com/
author_login: admin
author_email: chris@chrishaynie.com
author_url: http://www.chrishaynie.com/
wordpress_id: 94
wordpress_url: http://chrishaynie.com/?p=94
date: '2009-06-29 15:14:31 +0000'
date_gmt: '2009-06-29 21:14:31 +0000'
categories:
- code
tags:
- code
- script
- python
comments: []
---
<p>In order to speed up some of my scripts I've implemented threading using python. Some scripts now run in 1/10th the time they used to as a result.&nbsp; Here I will try to illustrate some of the key points, hopefully it will be useful to anyone.</p>
<pre lang="python">
#!/usr/bin/python<br />
import sys<br />
import subprocess<br />
from threading import Thread</p>
<p>class MyThread(Thread):<br />
    def __init__(self,server,cmd):<br />
        Thread.__init__(self)<br />
        self.server = server<br />
        self.cmd = cmd<br />
        self.status = -1<br />
    def run(self):<br />
        proc = subprocess.Popen('ssh %s %s' % (self.server,self.cmd),<br />
                       shell=True,<br />
                       stdin=subprocess.PIPE,<br />
                       stdout=subprocess.PIPE,<br />
                       stderr=subprocess.STDOUT,<br />
                       )<br />
        self.stdout, self.stderr = proc.communicate()</p>
<p>servers = ['shell1.example.net','ssh2.mhost.com','examplebox.net']<br />
command = 'uptime'<br />
cmdlist = []<br />
for srv in servers:<br />
    run = MyThread(srv,command)<br />
    cmdlist.append(run)<br />
    run.start()</p>
<p>for c in cmdlist:<br />
    c.join()<br />
    print "### %s:n%s" % (c.server,c.stdout),<br />
print "## Done"<br />
</pre></p>
<p>In my example we loop through servers[] and for each server[] we run the specified command on them, in their own thread! 3 servers in the above example, not a big deal, what if you have 20 servers? 40 servers? huge speed improvement.</p>
<p>The script can be easily extended to say... read the servers list in from a configuration file, or accept the 'command' from the command line arguments, (argv[1:]) etc.</p>
<p>Enjoy.</p>
