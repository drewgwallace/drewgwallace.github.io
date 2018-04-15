---
layout: post
title:  "PS3 Controller Map for Moonlite"
last_updated: 2018-04-15
date:   2018-04-13
---

<p id="bkmrk-when-starting-games-">When starting games through moonlite it will need it's own mapping that is outside of EmulationStation / RetroPi,<br>to do this we create a mapping and reference the file in our moonlite execution like this:</p>
<pre class="code hljs nginx" id="bkmrk-%23%21%2Fbin%2Fbash-moonligh"> <span class="hljs-title">moonlight</span>  -mapping <span style="text-decoration: underline;"><strong>&lt;CONTROLLER MAPPING&gt;</strong></span> -app <span class="hljs-string">"The Witcher 3: Wild Hunt"</span> stream &lt;IP ADDRESS&gt;</pre>
<p id="bkmrk-add-this-mapping-to-">To get started create file and add this mapping to /retropie/home/pi/ps3bluetooth</p>
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
