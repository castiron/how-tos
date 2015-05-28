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

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
```
#### Download Tomcat7

```
cd /opt
wget http://apache.mesi.com.ar/tomcat/tomcat-7/v7.0.54/bin/apache-tomcat-7.0.54.tar.gz
tar -xzf apache-tomcat-7.0.54.tar.gz
rm apache-tomcat-7.0.54.tar.gz
mv apache-tomcat-7.0.54 /var/lib/tomcat7
```
```
sudo apt-get install tomcat7
```
#### Get Solr logging dependencies

lib folder doesn't exist after install from repo—so create it!

```
mkdir /var/lib/tomcat7/lib
```

```
cd /opt
curl -L http://mirror.metrocast.net/apache//commons/logging/binaries/commons-logging-1.1.3-bin.tar.gz | tar -xzv
mv commons-logging-1.1.3 /usr/local/src/
curl -L http://www.slf4j.org/dist/slf4j-1.7.7.tar.gz | tar -xzv
mv slf4j-1.7.7 /usr/local/src/
cp /usr/local/src/commons-logging-1.1.3/commons-logging-*.jar /var/lib/tomcat7/lib/
cp /usr/local/src/slf4j-1.7.7/slf4j-*.jar /var/lib/tomcat7/lib/
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
