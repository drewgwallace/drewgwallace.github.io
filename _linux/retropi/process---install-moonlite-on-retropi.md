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
<p id="bkmrk-create-script-like-t">Create script like The_Witcher.sh</p>
<pre class="code" id="bkmrk-%23%21%2Fbin%2Fbash-moonligh">  #!/bin/bash
  
  moonlight  -mapping ps3bluetooth -app "The Witcher 3: Wild Hunt" stream 10.0.0.2</pre>
<p id="bkmrk-script-steam.sh">Script Steam.sh</p>
<pre class="code" id="bkmrk-%23%21%2Fbin%2Fbash-moonligh-0">  #!/bin/bash
  
  moonlight -mapping  ps3bluetooth stream 10.0.0.2</pre>
<p id="bkmrk-add-this-mapping-to-">Add this mapping to /retropie/home/pi ps3bluetooth</p>
<pre class="code" id="bkmrk-abs_x-%3D-0-abs_y-%3D-1-">abs_x = 0
abs_y = 1
abs_z = -1
reverse_x = false
reverse_y = true
abs_rx = 2
abs_ry = 3
abs_rz = -1
reverse_rx = false
reverse_ry = true
abs_deadzone = -696
abs_dpad_x = -1
abs_dpad_y = -1
reverse_dpad_x = false
reverse_dpad_y = false
btn_north = 300
btn_east = 301
btn_south = 302
btn_west = 303
btn_select = 288
btn_start = 291
btn_mode = 304
btn_thumbl = 289
btn_thumbr = 290
btn_tl = 298
btn_tr = 299
btn_tl2 = 296
btn_tr2 = 297
btn_dpad_up = 292
btn_dpad_down = 294
btn_dpad_left = 295
btn_dpad_right = 293</pre>
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
