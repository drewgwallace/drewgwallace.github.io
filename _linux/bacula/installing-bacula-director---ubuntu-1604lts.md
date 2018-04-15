---
layout: post
title:  "Process - Install Bacula Director from Source on Ubuntu 16.04"
date:   2018-04-15
---



            <p id="bkmrk-7.2.x-documentation"><a class="urlextern" title="http://www.bacula.org/7.2.x-manuals/en/main/index.html" href="http://www.bacula.org/7.2.x-manuals/en/main/index.html" rel="nofollow">7.2.X Documentation</a></p>
<div id="bkmrk-%C2%A0" class="secedit editbutton_section editbutton_1"> </div>
<h2 id="bkmrk-on-ubuntu-16.04lts-f" class="sectionedit2">On Ubuntu 16.04LTS from Source</h2>
<pre class="code" id="bkmrk-wget-http%3A%2F%2Fdownload">  wget http://downloads.sourceforge.net/project/bacula/bacula/7.4.1/bacula-7.4.1.tar.gz
  
  tar xzvf bacula-7.4.1.tar.gz
  
  
  #for MySQL
  sudo apt-get install build-essential mysql-server gcc zlib1g-dev mysql-client libmysqlclient-dev liblzo2-dev 
  #build-essential is for compiler programs
  #liblzo2-dev is for LZO compression
  #zlib1g-dev is for GZIP compression
  
  #for Postgres
  sudo apt-get install build-essential gcc postgresql postgresql-server-dev-all</pre>
<p id="bkmrk-compile-bacula">Compile bacula, note that the PREFIX installs to /opt/bacula</p>
<pre class="code" id="bkmrk-cd-bacula-7.4.1.tar.">  cd bacula-7.4.1.tar.gz
  
  #for MySQL
  vi compiler-script
      #!/bin/sh
      PREFIX=/opt/bacula
      CFLAGS="-g -O2 -Wall" \
      #CFLAGS -g is for built-in debugging, -O2 is preferred compiler optimazation
      ./configure \
            --sbindir=${PREFIX}/bin \
            --sysconfdir=${PREFIX}/etc \
            --docdir=${PREFIX}/html \
            --htmldir=${PREFIX}/html \
            --with-working-dir=${PREFIX}/working \
            --with-pid-dir=${PREFIX}/working \
            --with-scriptdir=${PREFIX}/scripts \
            --with-plugindir=${PREFIX}/plugins \
            --libdir=${PREFIX}/lib \
            --enable-smartalloc \
            --disable-conio \
            --enable-readline \
            --with-mysql \
            --with-baseport=9101
  
        exit 0 
        
  #for PostgreSQL
  vi compiler-script
      #!/bin/sh
      PREFIX=/opt/bacula
      CFLAGS="-g -O2 -Wall" \
      #CFLAGS -g is for built-in debugging, -O2 is preferred compiler optimazation
      ./configure \
            --sbindir=${PREFIX}/bin \
            --sysconfdir=${PREFIX}/etc \
            --docdir=${PREFIX}/html \
            --htmldir=${PREFIX}/html \
            --with-working-dir=${PREFIX}/working \
            --with-pid-dir=${PREFIX}/working \
            --with-scriptdir=${PREFIX}/scripts \
            --with-plugindir=${PREFIX}/plugins \
            --libdir=${PREFIX}/lib \
            --enable-smartalloc \
            --disable-conio \
            --enable-readline \
            --with-postgresql \
            --with-baseport=9101 \
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
<p id="bkmrk-add-the-%2Fopt%2Fbacula%2F">Add the /opt/bacula/bin directory to path</p>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Fenviron">  sudo vi /etc/environment
  sudo vi /etc/sudoers
      #edit the "secure path" adding /opt/bacula/bin
  sudo reboot</pre>
<p id="bkmrk-add-read-%26-execute-t">Add read &amp; execute to the /opt/bacula/etc directory</p>
<pre class="code" id="bkmrk-sudo-chmod-%2Brx-%2Fopt%2F">  sudo chmod +rx /opt/bacula/etc
  
  #ENTER CONFIG FILE HERE
  </pre>
<p id="bkmrk-give-the-bacula-%E2%80%9Cr">Give the Bacula “role” Create Database permissions</p>
<pre class="code" id="bkmrk-sudo--i-su-postgres-">  sudo -i
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
<p id="bkmrk-create-bacula-user">Create bacula user</p>
<pre class="code" id="bkmrk-sudo-adduser-bacula-">  sudo adduser bacula
  #enter password
  </pre>
<p id="bkmrk-allow-postgres-conne">Allow postgres connection</p>
<pre class="code" id="bkmrk-sudo-vi-%2Fetc%2Fpostgre">  sudo vi /etc/postgresql/9.5/main/pg_hba.conf
  #add this to the top of connection options
  local   all             all                                     trust
  #exit
  sudo /etc/init.d/postgresql restart
  
  </pre>
<p id="bkmrk-run-database-scripts">Run Database scripts as bacula user</p>
<pre class="code" id="bkmrk-sudo-chmod-%2Brx-%2Fopt%2F-0">  sudo chmod +rx /opt/bacula/scripts/*
  sudo -i
  su bacula
  ./create_bacula_database 
  ./make_bacula_tables
  ./grant_bacula_privileges </pre>
<pre class="code" id="bkmrk-%23test-the-postgres-c">  #test the postgres connection with the bacula user
  psql -Ubacula bacula
  #this should put you into a postgres command prompt 
  #bacula=&gt; \q #quit the prompt</pre>
<div id="bkmrk-%C2%A0-0" class="secedit editbutton_section editbutton_2"> </div>
<h3 id="bkmrk-sample-bacula-dir.co" class="sectionedit3">Sample bacula-dir.conf</h3>
<pre class="code" id="bkmrk-bacula-base%40bacula-b">bacula-base@bacula-base:/opt/bacula/etc$ sudo cat bacula-dir.conf
#
# Default Bacula Director Configuration file
#
#  The only thing that MUST be changed is to add one or more
#   file or directory names in the Include directive of the
#   FileSet resource.
#
#  For Bacula release 7.4.1 (02 May 2016) -- ubuntu 16.04
#
#  You might also want to change the default email address
#   from root to your address.  See the "mail" and "operator"
#   directives in the Messages resource.
#
# Copyright (C) 2000-2015 Kern Sibbald
# License: BSD 2-Clause; see file LICENSE-FOSS
#

Director {                            # define myself
  Name = bacula-base-dir
  DIRport = 9101                # where we listen for UA connections
  QueryFile = "/opt/bacula/scripts/query.sql"
  WorkingDirectory = "/opt/bacula/working"
  PidDirectory = "/opt/bacula/working"
  Maximum Concurrent Jobs = 20
  Password = "<unique pw>"         # Console password
  Messages = Daemon
}

JobDefs {
  Name = "DefaultJob"
  Type = Backup
  Level = Incremental
  Client = bacula-base-fd
  FileSet = "Full Set"
  Schedule = "WeeklyCycle"
  Storage = FileTEST
  Messages = Standard
  Pool = FilePoolTEST
  SpoolAttributes = yes
  Priority = 10
  Write Bootstrap = "/opt/bacula/working/%c.bsr"
}


#
# Define the main nightly save backup job
#   By default, this job will back up to disk in /tmp
Job {
  Name = "BackupLocalFilesTEST"
  JobDefs = "DefaultJob"
}

#Job {
#  Name = "BackupClient2"
#  Client = bacula-base2-fd
#  JobDefs = "DefaultJob"
#}

#Job {
#  Name = "BackupClient1-to-Tape"
#  JobDefs = "DefaultJob"
#  Storage = LTO-4
#  Spool Data = yes    # Avoid shoe-shine
#  Pool = Default
#}

#}

# Backup the catalog database (after the nightly save)
Job {
  Name = "BackupCatalog"
  JobDefs = "DefaultJob"
  Level = Full
  FileSet="Catalog"
  Schedule = "WeeklyCycleAfterBackup"
  # This creates an ASCII copy of the catalog
  # Arguments to make_catalog_backup.pl are:
  #  make_catalog_backup.pl &lt;catalog-name&gt;
  RunBeforeJob = "/opt/bacula/scripts/make_catalog_backup.pl MyCatalog"
  # This deletes the copy of the catalog
  RunAfterJob  = "/opt/bacula/scripts/delete_catalog_backup"
  Write Bootstrap = "/opt/bacula/working/%n.bsr"
  Priority = 11                   # run after main backup
}

#
# Standard Restore template, to be changed by Console program
#  Only one such job is needed for all Jobs/Clients/Storage ...
#
Job {
  Name = "RestoreLocalFilesTEST"
  Type = Restore
  Client=bacula-base-fd
  FileSet="Full Set"
  Storage = FileTEST
  Pool = FilePoolTEST
  Messages = Standard
  Where = /home/bacula-base/restore
}


# List of files to be backed up
FileSet {
  Name = "Full Set"
  Include {
    Options {
      signature = MD5
      compression = GZIP
    }
#
#  Put your list of files here, preceded by 'File =', one per line
#    or include an external list with:
#
#    File = &lt;file-name
#
#  Note: / backs up everything on the root partition.
#    if you have other partitions such as /usr or /home
#    you will probably want to add them too.
#
#  By default this is defined to point to the Bacula binary
#    directory to give a reasonable FileSet to backup to
#    disk storage during initial testing.
#
    File = /
  }

#
# If you backup the root directory, the following two excluded
#   files can be useful
#
  Exclude {
    File = /opt/bacula/working
    File = /tmp
    File = /proc
    File = /tmp
    File = /sys
    File = /.journal
    File = /.fsck
    File = /home/bacula-base/backup
    File = /home/bacula-base/restore
  }
}

#
# When to do the backups, full backup on first sunday of the month,
#  differential (i.e. incremental since full) every other sunday,
#  and incremental backups other days
Schedule {
  Name = "WeeklyCycle"
  Run = Full 1st sun at 23:05
  Run = Differential 2nd-5th sun at 23:05
  Run = Incremental mon-sat at 23:05
}

# This schedule does the catalog. It starts after the WeeklyCycle
Schedule {
  Name = "WeeklyCycleAfterBackup"
  Run = Full sun-sat at 23:10
}

# This is the backup of the catalog
FileSet {
  Name = "Catalog"
  Include {
    Options {
      signature = MD5
    }
    File = "/opt/bacula/working/bacula.sql"
  }
}

# Client (File Services) to backup
Client {
  Name = bacula-base-fd
  Address = bacula-base.example.com
  FDPort = 9102
  Catalog = MyCatalog
  Password = "<unique pw>"          # password for FileDaemon
  File Retention = 60 days            # 60 days
  Job Retention = 6 months            # six months
  AutoPrune = yes                     # Prune expired Jobs/Files
}

#
# Second Client (File Services) to backup
#  You should change Name, Address, and Password before using
#
#Client {
#  Name = bacula-base2-fd
#  Address = bacula-base2
#  FDPort = 9102
#  Catalog = MyCatalog
#  Password = "<unique pw>"        # password for FileDaemon 2
#  File Retention = 60 days           # 60 days
#  Job Retention = 6 months           # six months
#  AutoPrune = yes                    # Prune expired Jobs/Files
#}


# Definition of file Virtual Autochanger device
Storage {
  Name = FileTEST
# Do not use "localhost" here
  Address = bacula-base.example.com                # N.B. Use a fully qualified name here
  SDPort = 9103
  Password = ""
  Device = FileChgr1
  Media Type = FileTEST
  Maximum Concurrent Jobs = 10        # run up to 10 jobs a the same time
}

# Definition of a second file Virtual Autochanger device
#   Possibly pointing to a different disk drive
Storage {
  Name = File2
# Do not use "localhost" here
  Address = bacula-base.example.com                # N.B. Use a fully qualified name here
  SDPort = 9103
  Password = ""
  Device = FileChgr2
  Media Type = File2
  Maximum Concurrent Jobs = 10        # run up to 10 jobs a the same time
}

# Definition of LTO-4 tape Autochanger device
#Storage {
#  Name = LTO-4
#  Do not use "localhost" here
#  Address = bacula-base               # N.B. Use a fully qualified name here
#  SDPort = 9103
#  Password = "<unique pw>"         # password for Storage daemon
#  Device = LTO-4                     # must be same as Device in Storage daemon
#  Media Type = LTO-4                 # must be same as MediaType in Storage daemon
#  Maximum Concurrent Jobs = 10
#}

# Generic catalog service
Catalog {
  Name = MyCatalog
  dbname = "bacula"; dbuser = "bacula"; dbpassword = ""
}

# Reasonable message delivery -- send most everything to email address
#  and to the console
Messages {
  Name = Standard
#
# NOTE! If you send to two email or more email addresses, you will need
#  to replace the %r in the from field (-f part) with a single valid
#  email address in both the mailcommand and the operatorcommand.
#  What this does is, it sets the email address that emails would display
#  in the FROM field, which is by default the same email as they're being
#  sent to.  However, if you send email to more than one address, then
#  you'll have to set the FROM address manually, to a single address.
#  for example, a 'no-reply@mydomain.com', is better since that tends to
#  tell (most) people that its coming from an automated source.

#
  mailcommand = "/opt/bacula/bin/bsmtp -h localhost -f \"\(Bacula\) \&lt;%r\&gt;\" -s \"Bacula: %t %e of %c %l\" %r"
  operatorcommand = "/opt/bacula/bin/bsmtp -h localhost -f \"\(Bacula\) \&lt;%r\&gt;\" -s \"Bacula: Intervention needed for %j\" %r"
  mail = root@localhost = all, !skipped
  operator = root@localhost = mount
  console = all, !skipped, !saved
#
# WARNING! the following will create a file that you must cycle from
#          time to time as it will grow indefinitely. However, it will
#          also keep all your messages if they scroll off the console.
#
  append = "/opt/bacula/log/bacula.log" = all, !skipped
  catalog = all
}


#
# Message delivery for daemon messages (no job).
Messages {
  Name = Daemon
  mailcommand = "/opt/bacula/bin/bsmtp -h localhost -f \"\(Bacula\) \&lt;%r\&gt;\" -s \"Bacula daemon message\" %r"
  mail = root@localhost = all, !skipped
  console = all, !skipped, !saved
  append = "/opt/bacula/log/bacula.log" = all, !skipped
}

# Default pool definition
Pool {
  Name = Default
  Pool Type = Backup
  Recycle = yes                       # Bacula can automatically recycle Volumes
  AutoPrune = yes                     # Prune expired volumes
  Volume Retention = 365 days         # one year
  Maximum Volume Bytes = 50G          # Limit Volume size to something reasonable
  Maximum Volumes = 100               # Limit number of Volumes in Pool
}

# File Pool definition
Pool {
  Name = FilePoolTEST
  Pool Type = Backup
  Recycle = yes                       # Bacula can automatically recycle Volumes
  AutoPrune = yes                     # Prune expired volumes
  Volume Retention = 365 days         # one year
  Maximum Volume Bytes = 50G          # Limit Volume size to something reasonable
  Maximum Volumes = 100               # Limit number of Volumes in Pool
  Label Format = "Vol-"               # Auto label
}


# Scratch pool definition
Pool {
  Name = Scratch
  Pool Type = Backup
}

#
# Restricted console used by tray-monitor to get the status of the director
#
Console {
  Name = bacula-base-mon
  Password = ""
  CommandACL = status, .status
}
bacula-base@bacula-base:/opt/bacula/etc$</pre>
<div id="bkmrk-%C2%A0-1" class="secedit editbutton_section editbutton_3"> </div>
<h3 id="bkmrk-adding-the-webmin-gu" class="sectionedit4">Adding the Webmin GUI console</h3>
<p id="bkmrk-http%3A%2F%2Fwebmin.com%2Fde"><a class="urlextern" title="http://webmin.com/deb.html" href="http://webmin.com/deb.html" rel="nofollow">http://webmin.com/deb.html</a></p>
<pre class="code" id="bkmrk-wget-http%3A%2F%2Fprdownlo">  wget http://prdownloads.sourceforge.net/webadmin/webmin_1.801_all.deb
  sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python
  #I ran into an error around here where my dependencies got screwed up and had to be fixed with "sudo apt-get install -f", installing dependencies before the next command will hopefully fix it...
  sudo dpkg --install webmin_1.801_all.deb
  </pre>
<p id="bkmrk-log-into-the-webmin-">Log into the Webmin Console at bacula.example.com:10000 <br> Use user credentials <br> Install the bacula module by going to <br> Webmin → Webmin Configuration → Webmin Modules → Select “Standard module from <a class="urlextern" title="http://www.webmin.com" href="http://www.webmin.com" rel="nofollow">www.webmin.com</a>” → “Bacula-Backup” → Install</p>
<p id="bkmrk-then-edit-the-bacula">Then edit the bacula configuration by going to <br> Unused Modules → Bacula Backup System → Configuration</p>
<p id="bkmrk-the-webmin-console-e">The Webmin console expects the bacula bconsole in “/usr/local/sbin/bconsole” <br> (<a class="urlextern" title="http://webadmin-list.narkive.com/XFHAUcpM/webmin-l-bacula-module" href="http://webadmin-list.narkive.com/XFHAUcpM/webmin-l-bacula-module" rel="nofollow">http://webadmin-list.narkive.com/XFHAUcpM/webmin-l-bacula-module</a>) <br> To fix this we need to add a symlink</p>
<pre class="code" id="bkmrk-sudo-ln--s-%2Fopt%2Fbacu">  sudo ln -s /opt/bacula/bin/bconsole /usr/local/sbin/bconsole</pre>
<p id="bkmrk-the-webmin-console-a">The Webmin console also needs Perl modules <br> DBD::Pg 1 PostgreSQL database driver for the DBI module <br> DBI 48 Database independent interface for Perl</p>
<p id="bkmrk-these-can-be-install">These can be installed from the root system tree by going to <br> Others → Perl Modules → Suggested Modules → Install Selected Modules</p>
<p id="bkmrk-authen%3A%3Alibwrap-modu">Authen::Libwrap module was causing the install to fail so unselect that module. (<a class="urlextern" title="http://askubuntu.com/questions/337880/perl-dependancy-problems-installing-webmin" href="http://askubuntu.com/questions/337880/perl-dependancy-problems-installing-webmin" rel="nofollow">http://askubuntu.com/questions/337880/perl-dependancy-problems-installing-webmin</a> I found comments stating you need “apt-get install libwrap0 libwrap0-dev”, that helped the install along but it still failed a test and shouldn't be necessary for our needs)</p>
<div id="bkmrk-%C2%A0-2" class="secedit editbutton_section editbutton_4"> </div>
<h3 id="bkmrk-rebind-webmin-to-por" class="sectionedit5">Rebind Webmin to port 443 with IPTables</h3>
<pre class="code" id="bkmrk-sudo-iptables--t-nat">  sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 10000
  sudo apt-get install iptables-persistent
  #The initial installation should offer to save IPTable rules, which was already added
  #Otherwise run "sudo netfilter-persistent save"</pre>
<div id="bkmrk-%C2%A0-3" class="secedit editbutton_section editbutton_5"> </div>
<p id="bkmrk-%C2%A0-4"> </p>
<p id="bkmrk-%C2%A0-7"> </p>
