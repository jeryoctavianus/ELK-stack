# ELK-stack
Using ELasticsearch, Logstash, and Kibana


# Installing Java
--Installing the default JRE/JDK
//ELK requires the installation of Java 8 and higher.
$ sudo apt-get update
$ sudo apt-get install default-jre
$ sudo apt-get install default-jdk

--Installing the Oracle JDK
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
$ sudo apt-get update

--Oracle JDK8
$ sudo apt-get install oracle-java8-installer

--Check Java version you are running to see verison 1.8.*
$ java -version

# Installing Elasticsearch
