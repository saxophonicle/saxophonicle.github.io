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
<p>In order to speed up some of my scripts I've implemented threading using python. Some scripts now run in 1/10th the time they used to as a result. Here I will try to illustrate some of the key points, hopefully it will be useful to anyone.</p>
<pre lang="python">
#!/usr/bin/python
import sys
import subprocess
from threading import Thread
class MyThread(Thread):
    def __init__(self,server,cmd):
        Thread.__init__(self)
        self.server = server
        self.cmd = cmd
        self.status = -1
    def run(self):
        proc = subprocess.Popen('ssh %s %s' % (self.server,self.cmd),
                       shell=True,
                       stdin=subprocess.PIPE,
                       stdout=subprocess.PIPE,
                       stderr=subprocess.STDOUT,
                       )
        self.stdout, self.stderr = proc.communicate()</p>
servers = ['shell1.example.net','ssh2.mhost.com','examplebox.net']
command = 'uptime'
cmdlist = []
for srv in servers:
    run = MyThread(srv,command)
    cmdlist.append(run)
    run.start()</p>
for c in cmdlist:
    c.join()
    print "### %s:n%s" % (c.server,c.stdout),
print "## Done"
</pre>
<p>In my example we loop through servers[] and for each server[] we run the specified command on them, in their own thread! 3 servers in the above example, not a big deal, what if you have 20 servers? 40 servers? huge speed improvement.</p>
<p>The script can be easily extended to say... read the servers list in from a configuration file, or accept the 'command' from the command line arguments, (argv[1:]) etc.</p>
<p>Enjoy.</p>
