---
layout: post
title:  "Process - Install Bacula Client from apt-get - Ubuntu/Debian"
date:   2018-04-15
last_updated: 2018-04-15
---

<h2 id="bkmrk-ubuntu" class="sectionedit8">Ubuntu 16.04</h2>
<pre class="code" id="bkmrk-sudo-apt-get-install">  sudo apt-get install bacula-fd</pre>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Fbacula%2F">sudo vi /etc/bacula/bacula-fd.conf</pre>
<p id="bkmrk-sample-bacula-fd.con">Sample Bacula-fd.conf</p>
<pre class="code" id="bkmrk-%23-%23-default-bacula-f">#
# Default  Bacula File Daemon Configuration file
#
#  For Bacula release 7.0.5 (28 July 2014) -- ubuntu 16.04
#
# There is not much to change here except perhaps the
# File daemon Name to
#

#
# List Directors who are permitted to contact this File daemon
#
Director {
  Name = bacula-base-dir #change this
  Password = "&lt;unique pw&gt;"
}

#
# Restricted Director, used by tray-monitor to get the
#   status of the file daemon
#
Director {
  Name = ubiquiti-controller-mon
  Password = "&lt;unique pw&gt;"
  Monitor = yes
}

#
# "Global" File daemon configuration specifications
#
FileDaemon {                          # this is me
  Name = ubiquiti-controller-fd
  FDport = 9102                  # where we listen for the director
  WorkingDirectory = /var/lib/bacula
  Pid Directory = /var/run/bacula
  Maximum Concurrent Jobs = 20
# Plugin Directory = /usr/lib/bacula
  FDAddress = &lt;FQDN&gt;
}

# Send all messages except skipped files back to Director
Messages {
  Name = Standard
  director = bacula-base-dir = all, !skipped, !restored
}</pre>
<div id="bkmrk-%C2%A0-8" class="secedit editbutton_section editbutton_9"> </div>
