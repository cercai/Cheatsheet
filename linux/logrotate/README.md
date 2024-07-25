# Logrotate

`Logrotate` is a system utility designed to manage the automatic rotation and compression of log files.

The Ubuntu documentation page: https://doc.ubuntu-fr.org/logrotate.

Global Configuration File: `/etc/logrotate.conf`

```sh
$ cat /etc/logrotate.conf
# see "man logrotate" for details

# global options do not affect preceding include directives

# rotate log files weekly
weekly

# use the adm group by default, since this is the owning group
# of /var/log/syslog.
su root adm

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
#dateext

# uncomment this if you want your log files compressed
#compress

# packages drop log rotation information into this directory
include /etc/logrotate.d

# system-specific logs may also be configured here.
```

To manually force a rotation, you can use:
```sh
$ logrotate -f /etc/logrotate.conf
```

Individual Configuration Files: Files in the `/etc/logrotate.d/` directory.

```sh
$ cat /etc/logrotate.d/myapp
/var/log/myapp.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    size 100M
    create 0640 root utmp
    postrotate
        /bin/kill -HUP `cat /var/run/myapp.pid 2>/dev/null` 2>/dev/null || true
    endscript
}
```
