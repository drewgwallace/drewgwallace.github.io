---
layout: post
title:  "Process - Install Grafana InfluxDB"
date:   2018-04-07
---



<p id="bkmrk-wget-sources-from-th">wget sources from these locations<br><a href="https://grafana.com/grafana/download">https://grafana.com/grafana/download<br>https://portal.influxdata.com/downloads</a></p>
<p id="bkmrk-better-yet%2C-add-thes">Better yet, add these to /etc/apt/sources.list as root by running</p>
<pre id="bkmrk-source-%2Fetc%2Flsb-rele"><code>source /etc/lsb-release<br>echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | tee -a /etc/apt/sources.list
echo "deb https://packagecloud.io/grafana/stable/debian/ jessie main" | tee -a /etc/apt/sources.list</code></pre>
<p id="bkmrk-then-add-the-signed-">Then add the signed key</p>
<pre id="bkmrk-curl-https%3A%2F%2Fpackage"><code>curl https://packagecloud.io/gpg.key <span class="token function">| </span>apt-key add -
curl -sL https://repos.influxdata.com/influxdb.key | apt-key add -</code></pre>
<p id="bkmrk-and-install">And install</p>
<pre id="bkmrk-sudo-apt-get-updates"><code>sudo apt-get update<br>sudo apt-get install grafana python3 python3-influxdb influxdb<br></code></pre>
<p id="bkmrk-create-self-signed-c">Create Self-signed certificates <br><a href="https://www.akadia.com/services/ssh_test_certificate.html">https://www.akadia.com/services/ssh_test_certificate.html</a></p>
<pre id="bkmrk-openssl-genrsa--des3">openssl genrsa -des3 -out server.key 1024<br>openssl req -new -key server.key -out server.csr<br>cp server.key server.key.org<br>openssl rsa -in server.key.org -out server.key<br>openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt<br>cp server.crt /etc/grafana/ssl.crt<br>cp server.key /etc/grafana/ssl.key</pre>
<p id="bkmrk-add-certificates-to-">Add certificates to Grafana<br>sudo vi /etc/grafana/grafana.ini</p>
<pre id="bkmrk-%23-https-certs-%26-key-"># https certs &amp; key file<br>cert_file = /home/grafana/server.crt<br>cert_key = /home/grafana/server.key</pre>
