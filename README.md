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
```
----- Output omitted ------
```
("nginx",pid=2465,fd=6))
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
```
##### Настраиваем аудит, который будет отслеживать изменения конфигураций nginx. Все критичные логи с web собираюся и локально и удаленно. Все логи с nginx уходят на удаленный сервер (локально только критичные). Логи аудита также уходят на удаленную систему.
```
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
root@web:~# apt list nginx
Listing... Done
nginx/jammy-updates,jammy-security,now 1.18.0-6ubuntu14.8 amd64 [installed]
N: There is 1 additional version. Please use the '-a' switch to see it
root@web:~# nano /etc/nginx/nginx.conf
root@web:~# cat /etc/nginx/nginx.conf
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log syslog:server=192.168.56.15:514,tag=nginx_access,severity=info combined;
        error_log /var/log/nginx/error.log;
        error_log syslog:server=192.168.56.15:514,tag=nginx_error;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
root@web:~# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@web:~# systemctl restart nginx
root@web:~# mv /var/www/html/index.nginx-debian.html /var/www/
root@web:~# ll /var/www/
html/                    index.nginx-debian.html
root@web:~#
root@log:~# cat /var/log/rsyslog/log/
rsyslogd.log  systemd.log
root@log:~# cat /var/log/rsyslog/web/nginx_access.log
Apr 25 07:42:59 web nginx_access: 192.168.56.1 - - [25/Apr/2026:07:42:59 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.0.0 Safari/537.36 Edg/147.0.0.0"
root@log:~# cat /var/log/rsyslog/web/nginx_error.log
cat: /var/log/rsyslog/web/nginx_error.log: No such file or directory
root@log:~# ll /var/www/
ls: cannot access '/var/www/': No such file or directory
root@log:~# cat /var/log/rsyslog/web/nginx_error.log
Apr 25 07:47:02 web nginx_error: 2026/04/25 07:47:02 [error] 2571#2571: *3 directory index of "/var/www/html/" is forbidden, client: 192.168.56.1, server: _, request: "GET / HTTP/1.1", host: "192.168.56.10"
root@log:~#
```
