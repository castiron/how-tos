# Assumes centos, assumes java 1.6 already installed, assumes 64bit.

iptables -A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -s 75.148.94.141 --dport 8080 -j ACCEPT
/sbin/service iptables save

yum update
yum install curl
yum install wget
yum install unzip
useradd tomcat

# Is java installed?
rpm -qa | grep -E '^open[jre|jdk]|j[re|dk]'


# If Java is installed
service tomcat6 stop
yum remove java-1.6.0-openjdk

# Download Java
cd /opt
wget --no-cookies \
--no-check-certificate \
--header "Cookie: oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/7u60-b19/jdk-7u60-linux-x64.rpm" \
-O /opt/jdk-7-linux-x64.rpm

# Intall Java 
rpm -Uvh /opt/jdk-7-linux-x64.rpm

# Setup alternatives (not sure if this is neccessray, but can't hurt)
alternatives --install /usr/bin/java java /usr/java/jdk1.7.0_60/jre/bin/java 20000
alternatives --install /usr/bin/jar jar /usr/java/jdk1.7.0_60/bin/jar 20000
alternatives --install /usr/bin/javac javac /usr/java/jdk1.7.0_60/bin/javac 20000
alternatives --install /usr/bin/javaws javaws /usr/java/jdk1.7.0_60/jre/bin/javaws 20000
alternatives --set java /usr/java/jdk1.7.0_60/jre/bin/java
alternatives --set javaws /usr/java/jdk1.7.0_60/jre/bin/javaws
alternatives --set javac /usr/java/jdk1.7.0_60/bin/javac
alternatives --set jar /usr/java/jdk1.7.0_60/bin/jar

# Get tomcat
cd /opt
wget http://apache.mesi.com.ar/tomcat/tomcat-7/v7.0.54/bin/apache-tomcat-7.0.54.tar.gz
tar -xzf apache-tomcat-7.0.54.tar.gz
rm apache-tomcat-7.0.54.tar.gz
mv apache-tomcat-7.0.54 /var/lib/tomcat7


# Dependencies
cd /opt
curl -L http://mirror.metrocast.net/apache//commons/logging/binaries/commons-logging-1.1.3-bin.tar.gz | tar -xzv
mv commons-logging-1.1.3 /usr/local/src/
curl -L http://www.slf4j.org/dist/slf4j-1.7.7.tar.gz | tar -xzv
mv slf4j-1.7.7 /usr/local/src/
cp /usr/local/src/commons-logging-1.1.3/commons-logging-*.jar /var/lib/tomcat7/lib/
cp /usr/local/src/slf4j-1.7.7/slf4j-*.jar /var/lib/tomcat7/lib/
rm -rf /var/lib/tomcat7/lib/slf4j-android-*.jar

# Get init.d script
cd /etc/init.d
wget https://gist.githubusercontent.com/zdavis/87313b787f9a5074d58c/raw/fb8243a573fcd63628db621628302de34533e7ca/tomcat7
chmod 755 tomcat7

#cd /var/lib/tomcat7/conf
#wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/web.xml

cd /home
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/install-solr.sh
chmod 777 /home/install-solr.sh && ./install-solr.sh
mv /opt/solr-tomcat/solr /home/solr
rm -rf /opt/solr-tomcat

chown -R tomcat:tomcat /var/lib/tomcat7
chown -R tomcat:tomcat /home/solr

# Start tomcat to get solr up ins
service tomcat7 start
service tomcat7 stop

cd /var/lib/tomcat7/webapps/solr/WEB-INF
mv web.xml web.xml.off
wget https://raw.githubusercontent.com/castiron/dockers/master/typo3-solr/config/web.xml


iptables -A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -s 75.148.94.141 --dport 8080 -j ACCEPT
/sbin/service iptables save

service tomcat7 start

