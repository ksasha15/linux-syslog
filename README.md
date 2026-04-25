# linux-syslog
##### Поднимаем две машины — web и log.
```
administrator@testpc:~/web_log_lab$ vagrant resume
==> web: Resuming suspended VM...
==> web: Booting VM...
==> web: Waiting for machine to boot. This may take a few minutes...
    web: SSH address: 127.0.0.1:2222
    web: SSH username: vagrant
    web: SSH auth method: private key
==> web: Machine booted and ready!
==> web: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> web: flag to force provisioning. Provisioners marked to run always will still run.
==> log: Resuming suspended VM...
==> log: Booting VM...
==> log: Waiting for machine to boot. This may take a few minutes...
    log: SSH address: 127.0.0.1:2200
    log: SSH username: vagrant
    log: SSH auth method: private key
==> log: Machine booted and ready!
==> log: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> log: flag to force provisioning. Provisioners marked to run always will still run.
```
##### На web поднимаем nginx.
```
administrator@testpc:~/web_log_lab$ vagrant ssh web
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-160-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sat Apr 25 06:51:01 AM UTC 2026

  System load:           0.0
  Usage of /:            16.0% of 30.34GB
  Memory usage:          16%
  Swap usage:            0%
  Processes:             141
  Users logged in:       1
  IPv4 address for eth0: 10.0.2.15
  IPv6 address for eth0: fd17:625c:f037:2:a00:27ff:fe16:ddef


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

Use of this system is acceptance of the OS vendor EULA and License Agreements.
Last login: Fri Apr 24 21:37:55 2026 from 10.0.2.2
vagrant@web:~$ time
time         timedatectl  timeout      times
vagrant@web:~$ timedatectl
               Local time: Sat 2026-04-25 06:51:19 UTC
           Universal time: Sat 2026-04-25 06:51:19 UTC
                 RTC time: Fri 2026-04-24 21:44:13
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: no
              NTP service: inactive
          RTC in local TZ: no
vagrant@web:~$ date
Sat Apr 25 06:51:54 AM UTC 2026
vagrant@web:~$ apt update && apt install -y nginx
Reading package lists... Done
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
vagrant@web:~$ sudo -i
root@web:~# apt update && apt install -y nginx
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Hit:2 http://us.archive.ubuntu.com/ubuntu jammy InRelease
Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [3,174 kB]
Get:4 http://us.archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
Get:5 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [448 kB]
Get:6 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [5,594 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/restricted Translation-en [1,070 kB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1,029 kB]
Get:9 http://security.ubuntu.com/ubuntu jammy-security/universe Translation-en [226 kB]
Get:10 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [52.3 kB]
Get:11 http://security.ubuntu.com/ubuntu jammy-security/multiverse Translation-en [10.5 kB]
Get:12 http://us.archive.ubuntu.com/ubuntu jammy-backports InRelease [127 kB]
Get:13 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [3,439 kB]
Get:14 http://us.archive.ubuntu.com/ubuntu jammy-updates/main Translation-en [519 kB]
Get:15 http://us.archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [5,759 kB]
Get:16 http://us.archive.ubuntu.com/ubuntu jammy-updates/restricted Translation-en [1,098 kB]
Get:17 http://us.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1,268 kB]
Get:18 http://us.archive.ubuntu.com/ubuntu jammy-updates/universe Translation-en [315 kB]
Get:19 http://us.archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [59.4 kB]
Get:20 http://us.archive.ubuntu.com/ubuntu jammy-updates/multiverse Translation-en [13.5 kB]
Get:21 http://us.archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [70.1 kB]
Get:22 http://us.archive.ubuntu.com/ubuntu jammy-backports/main Translation-en [11.4 kB]
Get:23 http://us.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [30.8 kB]
Get:24 http://us.archive.ubuntu.com/ubuntu jammy-backports/universe Translation-en [16.9 kB]
Fetched 24.6 MB in 7s (3,415 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
130 packages can be upgraded. Run 'apt list --upgradable' to see them.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3 libjbig0 libjpeg-turbo8 libjpeg8
  libnginx-mod-http-geoip2 libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter libnginx-mod-mail
  libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5 libwebp7 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6
  libxpm4 nginx-common nginx-core
Suggested packages:
  libgd-tools fcgiwrap nginx-doc ssl-cert
The following NEW packages will be installed:
  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3 libjbig0 libjpeg-turbo8 libjpeg8
  libnginx-mod-http-geoip2 libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter libnginx-mod-mail
  libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5 libwebp7 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6
  libxpm4 nginx nginx-common nginx-core
0 upgraded, 25 newly installed, 0 to remove and 130 not upgraded.
Need to get 3,549 kB of archives.
After this operation, 11.5 MB of additional disk space will be used.
Get:1 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libxau6 amd64 1:1.0.9-1build5 [7,634 B]
Get:2 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libxdmcp6 amd64 1:1.1.3-0ubuntu5 [10.9 kB]
Get:3 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libxcb1 amd64 1.14-3ubuntu3 [49.0 kB]
Get:4 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libx11-data all 2:1.7.5-1ubuntu0.3 [120 kB]
Get:5 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libx11-6 amd64 2:1.7.5-1ubuntu0.3 [667 kB]
Get:6 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 fonts-dejavu-core all 2.37-2build1 [1,041 kB]
Get:7 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 fontconfig-config all 2.13.1-4.2ubuntu5 [29.1 kB]
Get:8 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libdeflate0 amd64 1.10-2 [70.9 kB]
Get:9 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libfontconfig1 amd64 2.13.1-4.2ubuntu5 [131 kB]
Get:10 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libjpeg-turbo8 amd64 2.1.2-0ubuntu1 [134 kB]
Get:11 http://us.archive.ubuntu.com/ubuntu jammy/main amd64 libjpeg8 amd64 8c-2ubuntu10 [2,264 B]
Get:12 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libjbig0 amd64 2.1-3.1ubuntu0.22.04.1 [29.2 kB]
Get:13 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libwebp7 amd64 1.2.2-2ubuntu0.22.04.2 [206 kB]
Get:14 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libtiff5 amd64 4.3.0-6ubuntu0.13 [185 kB]
Get:15 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libxpm4 amd64 1:3.5.12-1ubuntu0.22.04.2 [36.7 kB]
Get:16 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libgd3 amd64 2.3.0-2ubuntu2.3 [129 kB]
Get:17 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 nginx-common all 1.18.0-6ubuntu14.8 [40.1 kB]
Get:18 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-http-geoip2 amd64 1.18.0-6ubuntu14.8 [12.1 kB]
Get:19 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-http-image-filter amd64 1.18.0-6ubuntu14.8 [15.6 kB]
Get:20 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-http-xslt-filter amd64 1.18.0-6ubuntu14.8 [13.9 kB]
Get:21 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-mail amd64 1.18.0-6ubuntu14.8 [45.8 kB]
Get:22 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-stream amd64 1.18.0-6ubuntu14.8 [73.0 kB]
Get:23 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libnginx-mod-stream-geoip2 amd64 1.18.0-6ubuntu14.8 [10.1 kB]
Get:24 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 nginx-core amd64 1.18.0-6ubuntu14.8 [484 kB]
Get:25 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 nginx amd64 1.18.0-6ubuntu14.8 [3,882 B]
Fetched 3,549 kB in 3s (1,280 kB/s)
Preconfiguring packages ...
Selecting previously unselected package libxau6:amd64.
(Reading database ... 45823 files and directories currently installed.)
Preparing to unpack .../00-libxau6_1%3a1.0.9-1build5_amd64.deb ...
Unpacking libxau6:amd64 (1:1.0.9-1build5) ...
Selecting previously unselected package libxdmcp6:amd64.
Preparing to unpack .../01-libxdmcp6_1%3a1.1.3-0ubuntu5_amd64.deb ...
Unpacking libxdmcp6:amd64 (1:1.1.3-0ubuntu5) ...
Selecting previously unselected package libxcb1:amd64.
Preparing to unpack .../02-libxcb1_1.14-3ubuntu3_amd64.deb ...
Unpacking libxcb1:amd64 (1.14-3ubuntu3) ...
Selecting previously unselected package libx11-data.
Preparing to unpack .../03-libx11-data_2%3a1.7.5-1ubuntu0.3_all.deb ...
Unpacking libx11-data (2:1.7.5-1ubuntu0.3) ...
Selecting previously unselected package libx11-6:amd64.
Preparing to unpack .../04-libx11-6_2%3a1.7.5-1ubuntu0.3_amd64.deb ...
Unpacking libx11-6:amd64 (2:1.7.5-1ubuntu0.3) ...
Selecting previously unselected package fonts-dejavu-core.
Preparing to unpack .../05-fonts-dejavu-core_2.37-2build1_all.deb ...
Unpacking fonts-dejavu-core (2.37-2build1) ...
Selecting previously unselected package fontconfig-config.
Preparing to unpack .../06-fontconfig-config_2.13.1-4.2ubuntu5_all.deb ...
Unpacking fontconfig-config (2.13.1-4.2ubuntu5) ...
Selecting previously unselected package libdeflate0:amd64.
Preparing to unpack .../07-libdeflate0_1.10-2_amd64.deb ...
Unpacking libdeflate0:amd64 (1.10-2) ...
Selecting previously unselected package libfontconfig1:amd64.
Preparing to unpack .../08-libfontconfig1_2.13.1-4.2ubuntu5_amd64.deb ...
Unpacking libfontconfig1:amd64 (2.13.1-4.2ubuntu5) ...
Selecting previously unselected package libjpeg-turbo8:amd64.
Preparing to unpack .../09-libjpeg-turbo8_2.1.2-0ubuntu1_amd64.deb ...
Unpacking libjpeg-turbo8:amd64 (2.1.2-0ubuntu1) ...
Selecting previously unselected package libjpeg8:amd64.
Preparing to unpack .../10-libjpeg8_8c-2ubuntu10_amd64.deb ...
Unpacking libjpeg8:amd64 (8c-2ubuntu10) ...
Selecting previously unselected package libjbig0:amd64.
Preparing to unpack .../11-libjbig0_2.1-3.1ubuntu0.22.04.1_amd64.deb ...
Unpacking libjbig0:amd64 (2.1-3.1ubuntu0.22.04.1) ...
Selecting previously unselected package libwebp7:amd64.
Preparing to unpack .../12-libwebp7_1.2.2-2ubuntu0.22.04.2_amd64.deb ...
Unpacking libwebp7:amd64 (1.2.2-2ubuntu0.22.04.2) ...
Selecting previously unselected package libtiff5:amd64.
Preparing to unpack .../13-libtiff5_4.3.0-6ubuntu0.13_amd64.deb ...
Unpacking libtiff5:amd64 (4.3.0-6ubuntu0.13) ...
Selecting previously unselected package libxpm4:amd64.
Preparing to unpack .../14-libxpm4_1%3a3.5.12-1ubuntu0.22.04.2_amd64.deb ...
Unpacking libxpm4:amd64 (1:3.5.12-1ubuntu0.22.04.2) ...
Selecting previously unselected package libgd3:amd64.
Preparing to unpack .../15-libgd3_2.3.0-2ubuntu2.3_amd64.deb ...
Unpacking libgd3:amd64 (2.3.0-2ubuntu2.3) ...
Selecting previously unselected package nginx-common.
Preparing to unpack .../16-nginx-common_1.18.0-6ubuntu14.8_all.deb ...
Unpacking nginx-common (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-http-geoip2.
Preparing to unpack .../17-libnginx-mod-http-geoip2_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-http-geoip2 (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-http-image-filter.
Preparing to unpack .../18-libnginx-mod-http-image-filter_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-http-image-filter (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-http-xslt-filter.
Preparing to unpack .../19-libnginx-mod-http-xslt-filter_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-http-xslt-filter (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-mail.
Preparing to unpack .../20-libnginx-mod-mail_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-mail (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-stream.
Preparing to unpack .../21-libnginx-mod-stream_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-stream (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package libnginx-mod-stream-geoip2.
Preparing to unpack .../22-libnginx-mod-stream-geoip2_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking libnginx-mod-stream-geoip2 (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package nginx-core.
Preparing to unpack .../23-nginx-core_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking nginx-core (1.18.0-6ubuntu14.8) ...
Selecting previously unselected package nginx.
Preparing to unpack .../24-nginx_1.18.0-6ubuntu14.8_amd64.deb ...
Unpacking nginx (1.18.0-6ubuntu14.8) ...
Setting up libxau6:amd64 (1:1.0.9-1build5) ...
Setting up libxdmcp6:amd64 (1:1.1.3-0ubuntu5) ...
Setting up libxcb1:amd64 (1.14-3ubuntu3) ...
Setting up libdeflate0:amd64 (1.10-2) ...
Setting up nginx-common (1.18.0-6ubuntu14.8) ...
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /lib/systemd/system/nginx.service.
Setting up libjbig0:amd64 (2.1-3.1ubuntu0.22.04.1) ...
Setting up libnginx-mod-http-xslt-filter (1.18.0-6ubuntu14.8) ...
Setting up libx11-data (2:1.7.5-1ubuntu0.3) ...
Setting up fonts-dejavu-core (2.37-2build1) ...
Setting up libjpeg-turbo8:amd64 (2.1.2-0ubuntu1) ...
Setting up libwebp7:amd64 (1.2.2-2ubuntu0.22.04.2) ...
Setting up libx11-6:amd64 (2:1.7.5-1ubuntu0.3) ...
Setting up libnginx-mod-http-geoip2 (1.18.0-6ubuntu14.8) ...
Setting up libjpeg8:amd64 (8c-2ubuntu10) ...
Setting up libnginx-mod-mail (1.18.0-6ubuntu14.8) ...
Setting up libxpm4:amd64 (1:3.5.12-1ubuntu0.22.04.2) ...
Setting up fontconfig-config (2.13.1-4.2ubuntu5) ...
Setting up libnginx-mod-stream (1.18.0-6ubuntu14.8) ...
Setting up libtiff5:amd64 (4.3.0-6ubuntu0.13) ...
Setting up libfontconfig1:amd64 (2.13.1-4.2ubuntu5) ...
Setting up libnginx-mod-stream-geoip2 (1.18.0-6ubuntu14.8) ...
Setting up libgd3:amd64 (2.3.0-2ubuntu2.3) ...
Setting up libnginx-mod-http-image-filter (1.18.0-6ubuntu14.8) ...
Setting up nginx-core (1.18.0-6ubuntu14.8) ...
 * Upgrading binary nginx                                                                                     [ OK ]
Setting up nginx (1.18.0-6ubuntu14.8) ...
Processing triggers for ufw (0.36.1-4ubuntu0.1) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.11) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
root@web:~#
root@web:~# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2026-04-25 06:53:40 UTC; 8min ago
       Docs: man:nginx(8)
    Process: 2372 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2373 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2465 (nginx)
      Tasks: 3 (limit: 1585)
     Memory: 4.8M
        CPU: 64ms
     CGroup: /system.slice/nginx.service
             ├─2465 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─2467 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" >
             └─2468 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" >

Apr 25 06:53:40 web systemd[1]: Starting A high performance web server and a reverse proxy server...
Apr 25 06:53:40 web systemd[1]: Started A high performance web server and a reverse proxy server.
root@web:~# ss -tlnp | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=2468,fd=6),("nginx",pid=2467,fd=6),("nginx",pid=2465,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=2468,fd=7),("nginx",pid=2467,fd=7),("nginx",pid=2465,fd=7))
root@web:~# ss -tln | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
LISTEN 0      511             [::]:80           [::]:*
root@web:~#
```
##### Настраиваем центральный лог-сервер на rsyslog
```
administrator@testpc:~/web_log_lab$ vagrant ssh log
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-160-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sat Apr 25 07:05:16 AM UTC 2026

  System load:           0.08
  Usage of /:            15.6% of 30.34GB
  Memory usage:          15%
  Swap usage:            0%
  Processes:             139
  Users logged in:       0
  IPv4 address for eth0: 10.0.2.15
  IPv6 address for eth0: fd17:625c:f037:2:a00:27ff:fe16:ddef

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

Use of this system is acceptance of the OS vendor EULA and License Agreements.
Last login: Fri Apr 24 21:37:51 2026 from 10.0.2.2
vagrant@log:~$ apt list rsyslog
Listing... Done
rsyslog/jammy-updates,jammy-security,now 8.2112.0-2ubuntu2.2 amd64 [installed,automatic]
N: There is 1 additional version. Please use the '-a' switch to see it
vagrant@log:~$ vi /etc/rsyslog.conf
vagrant@log:~$ nano /etc/rsyslog.conf
vagrant@log:~$ sudo -i
root@log:~# nano /etc/rsyslog.conf
root@log:~# cat /etc/rsyslog.conf
# /etc/rsyslog.conf configuration file for rsyslog
#
# For more information install rsyslog-doc and see
# /usr/share/doc/rsyslog-doc/html/configuration/index.html
#
# Default logging rules can be found in /etc/rsyslog.d/50-default.conf


#################
#### MODULES ####
#################

module(load="imuxsock") # provides support for local system logging
#module(load="immark")  # provides --MARK-- message capability

# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")

# provides kernel logging support and enable non-kernel klog messages
module(load="imklog" permitnonkernelfacility="on")

###########################
#### GLOBAL DIRECTIVES ####
###########################

#
# Use traditional timestamp format.
# To enable high precision timestamps, comment out the following line.
#
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

# Filter duplicated messages
$RepeatedMsgReduction on

#
# Set the default permissions for all log files.
#
$FileOwner syslog
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
$PrivDropToUser syslog
$PrivDropToGroup syslog

#
# Where to place spool and state files
#
$WorkDirectory /var/spool/rsyslog

#
# Include all config files in /etc/rsyslog.d/
#
$IncludeConfig /etc/rsyslog.d/*.conf

# Add remote logs
$template RemoteLogs,"/var/log/rsyslog/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& ~
root@log:~# systemctl restart rsyslog
root@log:~# ss -tuln
Netid      State       Recv-Q      Send-Q            Local Address:Port           Peer Address:Port     Process
udp        UNCONN      0           0                       0.0.0.0:514                 0.0.0.0:*
udp        UNCONN      0           0                 127.0.0.53%lo:53                  0.0.0.0:*
udp        UNCONN      0           0                10.0.2.15%eth0:68                  0.0.0.0:*
udp        UNCONN      0           0                          [::]:514                    [::]:*
tcp        LISTEN      0           25                      0.0.0.0:514                 0.0.0.0:*
tcp        LISTEN      0           128                     0.0.0.0:22                  0.0.0.0:*
tcp        LISTEN      0           4096              127.0.0.53%lo:53                  0.0.0.0:*
tcp        LISTEN      0           25                         [::]:514                    [::]:*
tcp        LISTEN      0           128                        [::]:22                     [::]:*
```
##### Настраиваем аудит, который будет отслеживать изменения конфигураций nginx. Все критичные логи с web должны собираться и локально и удаленно. Все логи с nginx должны уходить на удаленный сервер (локально только критичные). Логи аудита должны также уходить на удаленную систему.
```
