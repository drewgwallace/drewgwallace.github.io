---
layout: post
title:  "Process - Install Moonlite on RetroPi"
date:   2018-04-15
---

<p id="bkmrk-edit-%2Fetc%2Fapt%2Fsource">Edit /etc/apt/sources.list</p>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Fapt%2Fsou">  sudo vi /etc/apt/sources.list
  deb http://archive.itimmer.nl/raspbian/moonlight jessie main</pre>
<p id="bkmrk-install-moonlite">Install Moonlite</p>
<pre class="code" id="bkmrk-sudo-apt-get-update-">  sudo apt-get update
  sudo apt-get install moonlite-embedded</pre>
<p id="bkmrk-create-folder-%2Fhome%2F">Create folder /home/pi/RetroPie/roms/moonlite</p>
<p id="bkmrk-create-script-like-t">In the folder, create script such as The_Witcher.sh</p>
<pre class="code" id="bkmrk-%23%21%2Fbin%2Fbash-moonligh">  #!/bin/bash
  
  moonlight  -mapping &lt;CONTROLLER MAPPING&gt; -app "The Witcher 3: Wild Hunt" stream &lt;IP ADDRESS&gt;</pre>
<p id="bkmrk-script-steam.sh">Script Steam.sh</p>
<pre class="code" id="bkmrk-%23%21%2Fbin%2Fbash-moonligh-0">  #!/bin/bash
  
  moonlight -mapping  &lt;CONTROLLER MAPPING&gt; stream &lt;IP ADDRESS&gt;</pre>
<p id="bkmrk-%C2%A0"> </p>
<p id="bkmrk-edit-this-file-to-cr">Edit this file to create the system in EmulationStation</p>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Femulati">  sudo vi /etc/emulationstation/es_systems.cfg
  
  &lt;system&gt;
    &lt;name&gt;Moonlight&lt;/name&gt;
    &lt;path&gt;/home/pi/RetroPie/roms/moonlight/&lt;/path&gt;
    &lt;extension&gt;.sh&lt;/extension&gt;
    &lt;command&gt;%ROM%&lt;/command&gt;
  &lt;/system&gt;</pre>
<p id="bkmrk-pair-the-system">Pair the system</p>
<pre class="code" id="bkmrk-moonlight-pair-x.x.x">  moonlight pair X.X.X.X
  #Type the 4 digit code</pre>
<p id="bkmrk-restart-and-play%21">Restart and play!</p>
