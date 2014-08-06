## Oracle Java, Tomcat7, Solr 4.8 for TYPO3 on Centos

The how-to walks you through installing Oracle Java, Tomcat 7, and Solr 4.8 on a Centos box. It has been tested on 6.4 and 6.5 centos machines. There are some steps in here for when an older version of Java and/or Tomcat is installed. Use your best judgement in these cases. This how-to assumes you have a 64bit server. Adjust download paths accordingly if you do not.

#### Open up ports so you can access the installation

```
iptables -A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -s 75.148.94.141 --dport 8080 -j ACCEPT
/sbin/service iptables save
```

#### Make sure that software is up-to-date.

Yum update will likely update the kernel, so be sure you want to do this. You can always exclude the kernel by adding exclude=kernel* to /etc/yum.conf.

```
yum update
yum install curl
yum install wget
yum install unzip
```

#### Make sure the tomcat user exists

```
useradd tomcat
```

#### Is java installed?

```
rpm -qa | grep -E '^open[jre|jdk]|j[re|dk]'
```

#### If so, remove it

```
service tomcat6 stop
yum remove java-1.6.0-openjdk
```

#### Download Java from Oracle

```
cd /opt
wget --no-cookies \
--no-check-certificate \
--header "Cookie: oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/7u60-b19/jdk-7u60-linux-x64.rpm" \
-O /opt/jdk-7-linux-x64.rpm
```

#### Install Java 

```
rpm -Uvh /opt/jdk-7-linux-x64.rpm
alternatives --install /usr/bin/java java /usr/java/jdk1.7.0_60/jre/bin/java 20000
alternatives --install /usr/bin/jar jar /usr/java/jdk1.7.0_60/bin/jar 20000
alternatives --install /usr/bin/javac javac /usr/java/jdk1.7.0_60/bin/javac 20000
alternatives --install /usr/bin/javaws javaws /usr/java/jdk1.7.0_60/jre/bin/javaws 20000
alternatives --set java /usr/java/jdk1.7.0_60/jre/bin/java
alternatives --set javaws /usr/java/jdk1.7.0_60/jre/bin/javaws
alternatives --set javac /usr/java/jdk1.7.0_60/bin/javac
alternatives --set jar /usr/java/jdk1.7.0_60/bin/jar
```

#### Download Tomcat7

```
cd /opt
wget http://apache.mesi.com.ar/tomcat/tomcat-7/v7.0.55/bin/apache-tomcat-7.0.55.tar.gz
tar -xzf apache-tomcat-7.0.55.tar.gz
rm apache-tomcat-7.0.55.tar.gz
mv apache-tomcat-7.0.55 /var/lib/tomcat7
```

#### Get Solr logging dependencies

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

#### Get init.d script

```
cd /etc/init.d
wget https://gist.githubusercontent.com/lthurston/c7b1812fcc025a154147/raw/fbb8023e39e90d3028680c6b2ffecf1681ceb6a8/tomcat7
chmod 755 tomcat7
```

#### Install Solr with modified TYPO3 Solr install script

Note that this may throw a few errors, but you should be able to ignore them:

```
cd /home
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/install-solr.sh
chmod 777 /home/install-solr.sh && ./install-solr.sh
mv /opt/solr-tomcat/solr /home/solr
rm -rf /opt/solr-tomcat
```

#### Set permissions

```
chown -R tomcat:tomcat /var/lib/tomcat7
chown -R tomcat:tomcat /home/solr
```

#### Startup Tomcat to deploy the war

```
service tomcat7 start
service tomcat7 stop
```

#### Chkconfig sauce
```
chkconfig --add tomcat7
```

Check for runlevels 2 - 5:

```
chkconfig --list tomcat7
tomcat7        	0:off	1:off	2:on	3:on	4:on	5:on	6:off
```

#### Put the correct configuration in place for Solr

```
cd /var/lib/tomcat7/webapps/solr/WEB-INF
mv web.xml web.xml.off
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/web.xml
chown tomcat:tomcat web.xml
```

#### Start tomcat7

```
service tomcat7 start
```
