---
layout: post
title:  "Process - Install Bacula Client from yum - Centos/RHEL"
date:   2018-04-15
---

    <h1 id="bkmrk-page-title" class="float left">Installing Bacula Client - CentOS</h1>

    <div style="clear:left;"></div>

            <h2 id="bkmrk-centos" class="sectionedit9">CentOS</h2>
<pre class="code" id="bkmrk-yum-install-bacula-c">  yum install bacula-client.i686
  #edit the /etc/bacula/bacula-fd.conf
  /sbin/chkconfig bacula-fd on #enable the service at boot
  </pre>
<p id="bkmrk-you-will-also-need-t">You will also need to add the IPTables rules</p>
<pre class="code" id="bkmrk-iptables--i-input-1-">  iptables -I INPUT 1 -p tcp --dport 9102 -j ACCEPT
  /sbin/service iptables save</pre>
