# Sending notifications at startup and shutdown with a script (with systemd)

The goal is to send notification when the linux box starts up or before shutting down.

We should have a script that does the notifications (`/usr/local/bin/EmailStartStop`). It expects a working mail commanf. This example is a bit of an exaggeration as it tries to send pushbullet notifications also.


```sh
#!/bin/bash

### BEGIN INIT INFO
# Provides:        emailstartstop
# Required-Start:  $NetworkManager $network $remote_fs $syslog $postfix $mail-transport-agent
# Required-Stop:   $mail-transport-agent $NetworkManager $network $remote_fs $syslog
# Default-Start:   2 3 4 5
# Default-Stop:    1 6 0
# Short-Description: Sends an email at startup and shutdown
### END INIT INFO

# Description: Sends an email at system start and shutdown

#############################################
#                                           #
# Send an email on system start/stop to     #
# a user.                                   #
#                                           #
#############################################

EMAIL="myemailaddress@gmail.com"
HOSTN=`hostname`
RESTARTSUBJECT="[$HOSTN] - System startup"
SHUTDOWNSUBJECT="[$HOSTN] - System shutdown"
RESTARTBODY="Hey, $HOSTN started successfully.

Runlevel: "`/sbin/runlevel`"
Start up Date and Time: "`date`
SHUTDOWNBODY="Uhm, I just want you to know that $HOSTN is shutting down.

Runlevel: "`/sbin/runlevel`"
Shutdown Date and Time: "`date`" 
Uptime: "`uptime`
LOCKFILE=/var/local/SystemEmail
RETVAL=0

# Source function library.
# . /etc/init.d/functions
. /etc/profile > /dev/null 2>&1
. /etc/bash.bashrc > /dev/null 2>&1

stop()
{
echo "Pushbullet message..."
/usr/local/bin/pushbullet.sh "${SHURDOWNSUBJECT}" "${SHUTDOWNBODY}"

echo -n $"Sending Shutdown Email: "
echo "${SHUTDOWNBODY}" | mail -s "${SHUTDOWNSUBJECT}" ${EMAIL}
RETVAL=$?

if [ ${RETVAL} -eq 0 ]; then
rm -f ${LOCKFILE}
echo "  success"
else
echo "  failure"
fi
sleep 6
echo
return ${RETVAL}
}

start()
{
echo "Checking if internet is available..."
val=10
while [ $val -ge 0 ]
do
  host smtp.gmail.com > /dev/null 2>&1
  err=$?
  if [ $err -eq 0 ]
  then
     val=-1
  else
     sleep 2
     let val--
  fi
done 
echo "Pushbullet message..."
/usr/local/bin/pushbullet.sh "${RESTARTSUBJECT}" "${RESTARTBODY}"
echo -n $"Sending Startup Email: "
echo "${RESTARTBODY}" | mail -s "${RESTARTSUBJECT}" ${EMAIL}
RETVAL=$?

if [ ${RETVAL} -eq 0 ]; then
touch ${LOCKFILE}
echo "  success"
else
echo "  failure"
fi
echo
return ${RETVAL}
}

case $1 in
stop)
stop
;;

start)
start
;;

*)
echo "Call with start or stop parameter..."
;;
esac
exit ${RETVAL}

```

Systemd service file (`/etc/systemd/system/startupnotification.service`)

```ini
[Unit]
Description=Send notification at startup and shutdown
Requires=network.target

[Service]
Type=exec
RemainAfterExit=yes
ExecStart=/usr/local/bin/EmailStartStop start
ExecStop=/usr/local/bin/EmailStartStop stop

[Install]
WantedBy=multi-user.target
```

We have to *enable* this service with:

`sudo systemctl enable startupnotification`
