---
layout: post
title:  "Process - InfluxDB Authentication and Authorization"
date:   2018-04-07
---


<p id="bkmrk-https%3A%2F%2Fdocs.influxd"><a href="https://docs.influxdata.com/influxdb/v1.3/query_language/authentication_and_authorization/">https://docs.influxdata.com/influxdb/v1.3/query_language/authentication_and_authorization/</a></p>
<p id="bkmrk-from-the-influx-cli-">From the influx cli use these commands to create a new admin user</p>
<pre id="bkmrk-create-user-%3Cusernam"><code>CREATE USER &lt;username&gt; WITH PASSWORD '&lt;password&gt;' WITH ALL PRIVILEGES<br>#or<br>GRANT ALL PRIVILEGES TO &lt;username&gt;<br></code></pre>
<p id="bkmrk-%C2%A0for-a-standard-use"> For a standard user</p>
<pre id="bkmrk-create-user-%3Cusernam-0"><code>CREATE USER &lt;username&gt; WITH PASSWORD '&lt;password&gt;'<br>GRANT [READ,WRITE,ALL] ON &lt;database_name&gt; TO &lt;username&gt;</code></pre>
<p id="bkmrk-%C2%A0"> Authenticate to the influx CLI with</p>
<pre class=" language-bash" id="bkmrk-%3E-auth-username%3A-%3Cus"><code class=" language-bash">&gt; auth
username: &lt;username&gt;
password: &lt;password&gt;</code></pre>
<pre class=" language-bash" id="bkmrk-curl--g-http%3A%2F%2Flocal"><code class=" language-bash">curl -G http://localhost:8086/query -u todd:influxdb4ever --data-urlencode "q=SHOW DATABASES"<br></code></pre>
