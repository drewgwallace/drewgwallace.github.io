---
layout: post
title:  "Postfix - Null Client configuration"
date:   2018-04-04 11:42:58 -0800
categories: guides linux postfix
---

<html lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title>Playbook - Null Client Configuration</title>

    </head>
<body>
<div class="container" id="page-show">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <div class="page-content">

                <div ng-non-bindable>

    <h1 id="bkmrk-page-title" class="float left">Playbook - Null Client Configuration</h1>

    <div style="clear:left;"></div>

            <pre id="bkmrk-sudo-apt-get-install"><code>sudo apt-get install postfix mailutils<br></code>#Select no configuration</pre>
<pre id="bkmrk-sudo-vi-%2Fetc%2Fpostfix"><code>sudo vi /etc/postfix/main.cf</code><br><br>    inet_interfaces =  loopback-only<br>    #mydestination=<br>    #myhostname = <br>    mynetworks=127.0.0.0/8 [::1]/128 10.0.0.0/8<br>    #myorigin = $mydomain<br>    relayhost = postfixhost.example.com<br>    local_transport=error: local delivery disabled<br><br></pre>
<p id="bkmrk-%C2%A0"> </p>
<p id="bkmrk-test-with-thisecho-%22">Test with this<br><code>echo "Testing" | mail -s "test email" example@example.com</code></p>
    </div>
                <hr>

            </div>
        </div>
    </div>
</div>
</body>
</html>
