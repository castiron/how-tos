## Monitor Tomcat7 daemon with monit

This how-to explains how to use Monit to make sure your Tomcat7 daemon doesn't quit on you

#### Install Monit

On ubuntu:

```
apt-get install monit

```

#### Setup a config file

```
echo "check host tomcat with address localhost
stop program = "/etc/init.d/tomcat7 stop"
start program = "/etc/init.d/tomcat7 restart"
if failed port 8080 and protocol http then start
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

