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

//Install the official Elastic APT package signing key:

$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -



//Install the apt-transport-https package, which is required to retrieve deb packages served over HTTPS:

$ sudo apt-get install apt-transport-https



//Add the APT repository information to your server’s list of sources:

$ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic.list



//Update the list of available packages:

$ sudo apt-get update



//Install the elasticsearch package:

$ sudo apt-get install -y elasticsearch



//For our example, since we are installing Elasticsearch on server, it is a good best practice to bind Elasticsearch to either a private IP or localhost:

$ sudo vim /etc/elasticsearch/elasticsearch.yml

$ network.host: 0.0.0.0

$ http.port:9200



//Run ELasticsearch

$ sudo systemctl start elasticsearch.service



//Check Elasticsearch status

$ sudo systemctl status elasticsearch.service



//To confirm that everything is working as expected, point curl or your browser to http://[anyIP]:9200

//It will show some JSON data about 
