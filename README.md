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
//First, you need to add Elastic’s signing key so that the downloaded package can be verified
$ t -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

--For Debian, we need to then install the apt-transport-https package:
$ sudo apt-get install apt-transport-https

--The next step is to add the repository definition to your system:
$ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list

$ sudo apt-get update
$ sudo apt-get install elasticsearch

//For our example, since we are installing Elasticsearch on server, it is a good best practice to bind Elasticsearch to either a private IP or localhost:
$ sudo vim /etc/elasticsearch/elasticsearch.yml
$ network.host: "localhost"
$ http.port:9200

--Run ELasticsearch
$ sudo service elasticsearch start

//To confirm that everything is working as expected, point curl or your browser to http://localhost:9200


