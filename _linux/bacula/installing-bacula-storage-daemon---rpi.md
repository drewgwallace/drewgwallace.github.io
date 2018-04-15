---
layout: post
title:  "Process - Install Bacula Storage Daemon - Raspberry Pi / Debian based systems"
date:   2018-04-15
last_updated: 2018-04-15
---

<h1 id="bkmrk-installing-storage-d" class="sectionedit11">Installing Storage Daemon</h1>
<p id="bkmrk-wget-http%3A%2F%2Fdownload-0">wget <a class="urlextern" title="http://downloads.sourceforge.net/project/bacula/bacula/7.4.1/bacula-7.4.1.tar.gz" href="http://downloads.sourceforge.net/project/bacula/bacula/7.4.1/bacula-7.4.1.tar.gz" rel="nofollow">http://downloads.sourceforge.net/project/bacula/bacula/7.4.1/bacula-7.4.1.tar.gz</a> <br> tar -xf bacula-7.4.1.tar.gz</p>
<pre class="code" id="bkmrk-%23for-postgres-sudo-a">#for Postgres
sudo apt-get install build-essential gcc postgresql postgresql-server-dev-all

sudo vi ~/bacula-7.4.5.3/compiler-script
      #!/bin/sh
      #PREFIX=/opt/bacula
      CFLAGS="-g -O2 -Wall" \
      #CFLAGS -g is for built-in debugging, -O2 is preferred compiler optimazation
      ./configure \
            --sbindir=/bin \
            --sysconfdir=/etc/bacula \
            --docdir=/html/bacula \
            --htmldir=/html/bacula \
            --with-working-dir=/var/bacula \
            --with-pid-dir=/var/run \
            --with-scriptdir=/home/pi/bacula/scripts \
            --with-plugindir=/etc/bacula/plugins \
            --libdir=/var/lib \
            --with-postgresql \
            --enable-smartalloc \
            --disable-conio \
            --disable-build-dird \
            --enable-readline \
            --enable-thread-safety
    exit 0

sudo chmod u+x compiler-script
  
./compiler-script

make
  
sudo make install
  
sudo make install-autostart
#this will perform an update-rc.d and consequenty symbolic links to /etc/init.d
#what I found though, was the symlinks didn't have execute permissions
sudo chmod 755 /etc/init.d/bacula*
</pre>
<p id="bkmrk-give-the-bacula-%E2%80%9Cr-0">Give the Bacula “role” Create Database permissions</p>
<pre class="code" id="bkmrk-sudo--i-su-postgres--0">  sudo -i
  su postgres
  psql
  ALTER ROLE bacula CREATEDB;
  #if this give you error, CREATE ROLE bacula;
  #I also had to give login permissions ALTER ROLE bacula LOGIN;
  \du bacula
  #check here that bacula got Create DB permissions
  \q
  exit #exit back to standard user
  cd /opt/bacula/scripts
  </pre>
<p id="bkmrk-create-bacula-user-0">Create bacula user</p>
<pre class="code" id="bkmrk-sudo-adduser-bacula--0">  sudo adduser bacula
  #enter password
  </pre>
<p id="bkmrk-allow-postgres-conne-0">Allow postgres connection</p>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Fpostgre-0">  sudo vi /etc/postgresql/9.5/main/pg_hba.conf
  #add this to the top of connection options
  local   all             all                                     trust
  #exit
  sudo /etc/init.d/postgresql restart
  
  </pre>
<p id="bkmrk-run-database-scripts-0">Run Database scripts as bacula user</p>
<pre class="code" id="bkmrk-sudo-chmod-%2Brx-%2Fopt%2F-1">  sudo chmod +rx /opt/bacula/scripts/*
  sudo -i
  su bacula
  ./create_bacula_database 
  ./make_bacula_tables
  ./grant_bacula_privileges </pre>
<pre class="code" id="bkmrk-%23test-the-postgres-c-0">  #test the postgres connection with the bacula user
  psql -Ubacula bacula
  #this should put you into a postgres command prompt 
  #bacula=&gt; \q #quit the prompt</pre>
<p id="bkmrk-go-on-to-edit-the-%2Fe">Go on to edit the /etc/bacula/*.conf files as usual</p>
