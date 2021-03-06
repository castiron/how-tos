## Oracle Java, Tomcat7, Solr 4.8 for TYPO3 on Ubuntu

The how-to walks you through installing Oracle Java, Tomcat 7, and Solr 4.8 on a Ubuntu box. It has been tested on 14.04 LTS. There are some steps in here for when an older version of Java and/or Tomcat is installed. Use your best judgement in these cases. This how-to assumes you have a 64bit server. Adjust download paths accordingly if you do not.

#### Open up ports so you can access the installation

```
iptables -A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -s 63.234.145.250 --dport 8080 -j ACCEPT
sudo apt-get update
sudo apt-get install iptables-persistent
```

#### Make sure that software is up-to-date.

```
sudo apt-get update
sudo apt-get install curl wget unzip
```

#### Make sure the tomcat user exists

```
useradd tomcat
```

#### Is java installed?

```
sudo dpkg --get-selections | grep -v deinstall | grep -E '^open[jre|jdk]|j[re|dk]'
```

#### If so, remove it

```
sudo service tomcat6 stop
sudo apt-get remove java-1.6.0-openjdk
```

#### Add repo containing Oracle Java

```
sudo add-apt-repository ppa:webupd8team/java
```

#### Install Java 
Note: See next section _(Oops Java failed to install)_ if Java indeed failed to install
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
```

#### Oops Java failed to install

If you see output like the following:
```
Location: http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.tar.gz?AuthParam=1505233415_2df2c1394b73668f723a2612f9d03174 [following]
--2017-09-12 16:21:35--  http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.tar.gz?AuthParam=1505233415_2df2c1394b73668f723a2612f9d03174
Connecting to download.oracle.com (download.oracle.com)|207.108.220.152|:80... connected.
HTTP request sent, awaiting response... 404 Not Found
2017-09-12 16:21:36 ERROR 404: Not Found.

download failed
Oracle JDK 7 is NOT installed.
```

You'll need to manually download the JDK .tar.gz as Oracle now requires an account (free) to download. 

Once you have the JDK on your local machine, upload to the target host and move it where the installer expects to find it.

```
scp jdk-7u80-linux-x64.tar.gz user@host.cicnode.com:~/
mv jdk-7u80-linux-x64.tar.gz /var/cache/oracle-jdk7-installer/
apt-get install oracle-java7-installer
```


#### Download Tomcat7

```
cd /opt
wget http://apache.mesi.com.ar/tomcat/tomcat-7/v7.0.75/bin/apache-tomcat-7.0.75.tar.gz
tar -xzf apache-tomcat-7.0.75.tar.gz
rm apache-tomcat-7.0.75.tar.gz
mv apache-tomcat-7.0.75 /var/lib/tomcat7
```
```
sudo apt-get install tomcat7
```
#### Get Solr logging dependencies

if lib folder doesn't exist after install from repo, create it!

```
mkdir /var/lib/tomcat7/lib
```

```
cd /opt
curl -L http://mirror.metrocast.net/apache//commons/logging/binaries/commons-logging-1.2-bin.tar.gz | tar -xzv
mv commons-logging-1.2 /usr/local/src/
curl -L http://www.slf4j.org/dist/slf4j-1.7.19.tar.gz | tar -xzv
mv slf4j-1.7.19 /usr/local/src/
cp /usr/local/src/commons-logging-1.2/commons-logging-*.jar /var/lib/tomcat7/lib/
cp /usr/local/src/slf4j-1.7.19/slf4j-*.jar /var/lib/tomcat7/lib/
rm -rf /var/lib/tomcat7/lib/slf4j-android-*.jar
```

Change owner:group of new lib/ dir

```
chown -R tomcat7:tomcat7 /var/lib/tomcat7/lib
```

#### Install Solr with modified TYPO3 Solr install script

```
cd /home
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/install-solr.sh
chmod 777 /home/install-solr.sh && ./install-solr.sh
mv /opt/solr-tomcat/solr /home/solr
rm -rf /opt/solr-tomcat
```

#### Set permissions

```
chown -R tomcat7:tomcat7 /home/solr
```

#### Startup Tomcat to deploy the war

```
service tomcat7 start
service tomcat7 stop
```

...if you run into problemos like '...policy.d/*.policy no such file' try something like this:
```
cd /var/lib/tomcat7/conf
ln -s /etc/tomcat7/policy.d
```

#### .d zest

```
update-rc.d apache2 defaults
```

Confirm flavor of the zest by looking for: [+] 

```
service --status-all | grep 'tomcat7'
```

#### Put the correct configuration in place for Solr

```
cd /var/lib/tomcat7/webapps/solr/WEB-INF
mv web.xml web.xml.off
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/web.xml
```

#### Start tomcat7

```
service tomcat7 start
```

### Slow startup?

If you are using Java 7, and you start tomcat and it take like 10 minutes for the solr application to become available, your system may suffer from lack of entropy. This might be fixed by explicitly telling Java to use `/dev/urandom` instead of `/dev/random` to generate seeds when starting up. To do that:

Edit `/usr/lib/jvm/java-7-oracle/jre/lib/security/java.security` (or whatever your main `java.security` is) and change

```
securerandom.source=file:/dev/urandom
```
to
```
securerandom.source=file:/dev/./urandom
```
Apparently Java tries to be smart and use `/dev/random` even if `/dev/urandom` is specified there. You have to trick it by using the lil dot.

For more info see:
http://serverfault.com/questions/655616/tomcat7-hangs-on-deploying-apps
http://marc.info/?l=tomcat-user&m=132769606728228&w=2
