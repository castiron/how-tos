## Monitor Tomcat7 daemon with Monit

This how-to explains how to use Monit to make sure your Tomcat7 daemon doesn't quit on you

#### Install Monit

On ubuntu:

```
apt-get install monit
```

#### Setup a config file

```
echo "check process tomcat with pidfile /var/run/tomcat7.pid
  stop program = "/etc/init.d/tomcat7 stop"
  start program = "/etc/init.d/tomcat7 restart"
  if failed port 8080 then alert
  if failed port 8080 for 5 cycles then restart
" >> /etc/monit/conf.d/tomcat7
```
### Restart monit

```
service monit restart
```

### Stop Tomcat7, wait 2 minutes, and watch it get restarted.

```
service tomcat7 stop
tail -f /var/lib/tomcat7/logs/catalina.out
```

#### Consider Tuning Monit

You may want to tune monit. Initial tests with this configuration seem to work. I've stopped the process and then placed a fake pid file at /var/run/tomcat7.pid, and Monit appears to detect that the process has failed (and that port 8080 is unavailable), and starts the server back up. Initially, I tried to just have Monit monitor 8080, but this was problematic because sometimes it takes Tomcat more than 2 minutes to start, and monit will re-poll the port before it's done starting, which leads to a restart loop. This combination of checking the process and checking the port seems to work well.

In any case, I recommend stopping Solr and watching Monit restart it to be sure everything works as expected.
