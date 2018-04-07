---
layout: post
title:  "Process - Install and configure Telegraf for InfluxDB"
date:   2018-04-07
---


<div class="container" id="page-show">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <div class="page-content">

                <div ng-non-bindable>

    <h1 id="bkmrk-page-title" class="float left">Process - Install and configure Telegraf for InfluxDB</h1>

    <div style="clear:left;"></div>

            <p id="bkmrk-this-is-to-describe-">This is to describe configuring telegraf output plugins to communicate with an InfluxDB database</p>
<p id="bkmrk-firstly-add-the-pack">Firstly add the package repository and install telegraf</p>
<pre id="bkmrk-sudo-apt-get-install">source /etc/lsb-release<br>echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | tee -a /etc/apt/sources.list<br>curl -sL https://repos.influxdata.com/influxdb.key | apt-key add -<br>sudo apt-get update<br>sudo apt-get install telegraf</pre>
<p id="bkmrk-for-a-basic-setup-on">For a basic setup only a few lines will need to be modified in /etc/telegraf/telegraf.conf , you will find them in the "output plugins" section, most other options are optional</p>
<pre id="bkmrk-%23-configuration-for-"># Configuration for influxdb server to send metrics to<br>database = "<strong>&lt;database name to use in InfluxDB&gt;</strong>" # required<br>urls = ["http://<strong>&lt;InfluxDB Server FQDN&gt;</strong>:8086"] # required<br><br>#and optionally add username, password, and encryption settings<br>username = "<strong>&lt;username&gt;</strong>" # optional<br>password = "<strong>&lt;password&gt;</strong>" # optional<br>ssl_ca = "/etc/telegraf/ca.pem"<br>ssl_cert = "/etc/telegraf/cert.pem"<br>ssl_key = "/etc/telegraf/key.pem"</pre>
<p id="bkmrk-now-start-the-servic">Now start the service and configure the service to start automatically</p>
<pre id="bkmrk-%C2%A0sudo-systemctl-sta"> sudo systemctl start telegraf.service<br> sudo systemctl enable telegraf.service</pre>
<p id="bkmrk-by-default-the-syste">By default the system is going to send metrics to the InfluxDB database, inspect the log files on both the metrics originating machine, the destination database machine for any errors</p>
<pre id="bkmrk-less-%2Fvar%2Flog%2Fsyslog">less /var/log/syslog # Debian/Ubuntu command<br>less /var/log/messages #RHEL/CentOS</pre>
<p id="bkmrk-also%2C-inspect-the-in">Also, inspect the InfluxDB database, an example command to check that values are being recorded<br><code><a href="mailto:host@host:~%24">host@host:~$</a> influx #enter the database</code><br><code>&gt; use &lt;<strong>database name</strong>&gt; #select the database to use</code></p>
<pre id="bkmrk-%3E-select-last%28uptime">&gt; select last(uptime_format) from system where host =~ /^<strong>&lt;host originating telegraf metrics&gt;</strong>$/</pre>
    </div>
                <hr>
            </div>
        </div>
    </div>
</div>
