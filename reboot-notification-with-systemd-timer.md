# Reboot notification with Systemd timer

If an Ubuntu has an update installed that needs reboot, there will be a file created: `/var/run/reboot-required`. This script check if this file exists and sends a desktop notification.

Create the script:
```sh
#!/bin/bash

cmd=/usr/bin/notify-send
icon=face-wink
title='Újraindítás szükséges!'
message='A számítógépre olyan frissítések kerültek telepítésre, amik után a gépet újra kell indítani. Kérlek, valamikor indítsd újra!'
if [ -f /var/run/reboot-required ]
then
    $cmd -i $icon "$title" "$message"
fi

```

Make it executable: `chmod 755 /usr/local/bin/reboot-notify.sh`.

Now create a .service file: `/etc/systemd/system/reboot-notify.service`

```systemd
[Unit]
Description=Notification to reboot if updates need

[Service]
Type=simple
ExecStart=/usr/local/bin/reboot-notify.sh

[Install]
WantedBy=multi-user.target

```

And the timer file: `/etc/systemd/system/reboot-notify.timer`
```systemd
[Unit]
Description=Check if reboot needed hourly

[Timer]
OnCalendar=*-*-* *:55:55
Unit=reboot-notify.service

[Install]
WantedBy=multi-user.target
```
Enable the service and the timer with these commands:

```sh
sudo systemctl enable reboot-notify.service
sudo systemctl enable reboot-notify.timer
sudo systemctl daemon-reload
```

More on system timers: <https://www.freedesktop.org/software/systemd/man/systemd.time.html>
