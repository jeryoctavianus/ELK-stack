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


# Installing Kibana



# Installing Logstash
//Download Logstash package
$ wget --no-check-certificate https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb

//Extract and install
$ dpkg -i logstash-5.2.0.deb

//Logstash is installed but it is not configured yet
-- Generate SLL certificates
$ sudo mkdir -p /etc/pki/tls/certs
$ sudo mkdir /etc/pki/tls/private

--Using IP address
//If you don't have a DNS setup—that would allow your servers, that you will gather logs from, to resolve the IP address of your ELK Server—you will have to add your ELK Server's private IP address to the subjectAltName (SAN) field of the SSL certificate that we are about to generate. To do so, open the OpenSSL configuration file:
$ sudo vi /etc/ssl/openssl.cnf

//Find the [ v3_ca ] section in the file, and add this line under it (substituting in the ELK Server's private IP address):
$ subjectAltName = IP: ELK_server_private_IP

//Now generate the SSL certificate and private key in the appropriate locations (/etc/pki/tls/), with the following commands:
$ cd /etc/pki/tls
$ sudo openssl req -config /etc/ssl/openssl.cnf -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt

--Configure Logstash
//Let's create a configuration file input.conf and set up our "file" input:
$ sudo nano /etc/logstash/conf.d/input.conf

//Insert the following input configuration:
input{
	file{
		#path=>"/var/log/apache2/access.log"
		path=>"/home/jeryoctavianus/Desktop/data science/cuaca.log"
		start_position=>"beginning"
	}
}

//Let's create a configuration file filter.conf:
$ sudo nano /etc/logstash/conf.d/filter.conf

//Insert the following configuration
filter{
	grok{
		match => {"message" => 
			[
			#IBANDUNG8, IBANDUNG29
			'%{IP:ip_address} %{USER:identity} %{USER:auth} \[%{HTTPDATE:req_ts}\] "%{WORD:http_verb} %{GREEDYDATA:req_path}\?%{WORD:txt_id}=%{WORD:sensor_id}&%{WORD:txt_tempf}=%{NUMBER:tempf:float}&%{WORD:txt_humidity}=%{NUMBER:humidity:float}&%{WORD:txt_softwaretype}=%{GREEDYDATA:softwaretype}&%{WORD:txt_action}=%{WORD:action}&%{WORD:txt_realtime}=%{INT:realtime:int}&%{WORD:txt_rtfreq}=%{INT:rtfreq:int} %{GREEDYDATA:http_version}" %{INT:http_status:int} %{INT:num_bytes:int}',
			#IBANDUNG8
			'%{IP:ip_address} %{USER:identity} %{USER:auth} \[%{HTTPDATE:req_ts}\] "%{WORD:http_verb} %{GREEDYDATA:req_path}\?%{WORD:txt_id}=%{WORD:sensor_id}&%{WORD:txt_tempf}=%{NUMBER:tempf:float}&%{WORD:txt_humidity}=%{NUMBER:humidity:float}&%{WORD:txt_softwaretype}=%{GREEDYDATA:softwaretype}&%{WORD:txt_action}=%{WORD:action}&%{WORD:txt_realtime}=%{INT:realtime:int}&%{WORD:txt_rtfreq}=%{INT:rtfreq:int}&%{WORD:txt_dateutc}=%{GREEDYDATA:dateutc} %{GREEDYDATA:http_version}" %{INT:http_status:int} %{INT:num_bytes:int}',
			#IBANDUNG4
			'%{IP:ip_address} %{USER:identity} %{USER:auth} \[%{HTTPDATE:req_ts}\] "%{WORD:http_verb} %{GREEDYDATA:req_path}\?%{WORD:txt_id}=%{WORD:sensor_id}&%{WORD:txt_tempf}=%{NUMBER:tempf:float}&%{WORD:txt_humidity}=%{NUMBER:humidity:float}&%{WORD:txt_dewptf}=%{NUMBER:dewptf:float}&%{WORD:txt_windchill}=%{NUMBER:windchillf:float}&%{WORD:txt_winddir}=%{NUMBER:winddir:float}&%{WORD:txt_windspeed}=%{NUMBER:windspeedmph:float}&%{WORD:txt_windgust}=%{NUMBER:windgustmph:float}&%{WORD:txt_rain}=%{NUMBER:rainin:float}&%{WORD:txt_dailyrainin}=%{NUMBER:dailyrainin:float}&%{WORD:txt_weeklyrain}=%{NUMBER:weeklyrainin:float}&%{WORD:txt_monthlyrain}=%{NUMBER:monthlyrainin:float}&%{WORD:txt_yearlyrain}=%{NUMBER:yearly_rainin:float}&%{WORD:txt_solarradiation}=%{NUMBER:solarradiation:float}&%{WORD:txt_UV}=%{NUMBER:uv:float}&%{WORD:txt_indoortempf}=%{NUMBER:indoortempf:float}&%{WORD:txt_indoorhumidity}=%{NUMBER:indoorhumidity:float}&%{WORD:txt_baromin}=%{NUMBER:baromin:float}&%{WORD:txt_lowbatt}=%{NUMBER:lowbatt:float}&%{WORD:txt_dateutc}=%{GREEDYDATA:dateutc}&%{WORD:txt_softwaretype}=%{GREEDYDATA:softwaretype}&%{WORD:txt_action}=%{WORD:action}&%{WORD:txt_realtime}=%{INT:realtime:int}&%{WORD:txt_rtfreq}=%{INT:rtfreq:int} %{GREEDYDATA:http_version}" %{INT:http_status:int} %{INT:num_bytes:int}',
			#IBANDUNG7
			'%{IP:ip_address} %{USER:identity} %{USER:auth} \[%{HTTPDATE:req_ts}\] "%{WORD:http_verb} %{GREEDYDATA:req_path}\?%{WORD:txt_id}=%{WORD:sensor_id}&%{WORD:txt_action}=%{WORD:action}&%{WORD:txt_realtime}=%{INT:realtime:int}&%{WORD:txt_rtfreq}=%{INT:rtfreq:int}&%{WORD:txt_dewptf}=%{NUMBER:dewptf:float}&%{WORD:txt_windspeed}=%{NUMBER:windspeedmph:float}&%{WORD:txt_softwaretype}=%{GREEDYDATA:softwaretype}&%{WORD:txt_dateutc}=%{GREEDYDATA:dateutc}&%{WORD:txt_tempf}=%{NUMBER:tempf:float}&%{WORD:txt_humidity}=%{NUMBER:humidity:float} %{GREEDYDATA:http_version}" %{INT:http_status:int} %{INT:num_bytes:int}',
			#IBANDUNG10, IBANDUNG24, IBANDUNG26
			'%{IP:ip_address} %{USER:identity} %{USER:auth} \[%{HTTPDATE:req_ts}\] "%{WORD:http_verb} %{GREEDYDATA:req_path}\?%{WORD:txt_id}=%{WORD:sensor_id}&%{WORD:txt_tempf}=%{NUMBER:tempf:float}&%{WORD:txt_humidity}=%{NUMBER:humidity:float}&%{WORD:txt_dewptf}=%{NUMBER:dewptf:float}&%{WORD:txt_windchill}=%{NUMBER:windchillf:float}&%{WORD:txt_winddir}=%{NUMBER:winddir:float}&%{WORD:txt_windspeed}=%{NUMBER:windspeedmph:float}&%{WORD:txt_windgust}=%{NUMBER:windgustmph:float}&%{WORD:txt_rain}=%{NUMBER:rainin:float}&%{WORD:txt_dailyrainin}=%{NUMBER:dailyrainin:float}&%{WORD:txt_weeklyrain}=%{NUMBER:weeklyrainin:float}&%{WORD:txt_monthlyrain}=%{NUMBER:monthlyrainin:float}&%{WORD:txt_yearlyrain}=%{NUMBER:yearlyrainin:float}&%{WORD:txt_solarradiation}=%{NUMBER:solarradiation:float}&%{WORD:txt_UV}=%{NUMBER:uv:float}&%{WORD:txt_indoortempf}=%{NUMBER:indoortempf:float}&%{WORD:txt_indoorhumidity}=%{NUMBER:indoorhumidity:float}&%{WORD:txt_baromin}=%{NUMBER:baromin:float}&%{WORD:txt_lowbatt}=%{NUMBER:lowbatt:float}&%{WORD:txt_dateutc}=%{GREEDYDATA:dateutc}&%{WORD:txt_softwaretype}=%{GREEDYDATA:softwaretype}&%{WORD:txt_action}=%{WORD:action}&%{WORD:txt_realtime}=%{INT:realtime:int}&%{WORD:txt_rtfreq}=%{INT:rtfreq:int} %{GREEDYDATA:http_version}" %{INT:http_status:int} %{INT:num_bytes:int}'
			]
		}
	}

	mutate{
			convert=>{
				"response"=>"integer"
				"bytes"=>"integer"
			}
	}

	mutate{
			remove_field => [ "headers", "@version", "host", "identity", "auth", "http_verb", "req_path", "txt_id", "txt_tempf", "txt_humidity", "txt_softwaretype", "txt_action", "txt_realtime", "txt_rtfreq", "txt_dewptf", "txt_windchill", "txt_winddir", "txt_windspeed", "txt_windgust", "txt_rain", "txt_dailyrainin", "txt_weeklyrain", "txt_monthlyrain", "txt_yearlyrain", "txt_solarradiation", "txt_UV", "txt_indoortempf", "txt_indoorhumidity", "txt_baromin", "txt_lowbatt", "txt_dateutc", "http_version", "http_status", "num_bytes" ]
	}
}

//Let's create a configuration file output.conf:
$ sudo nano /etc/logstash/conf.d/output.conf

//Insert the following configuration
output{
	elasticsearch{
		hosts => ["202.138.234.141:9200"]
		index => "cuaca"
		document_type => "default"
		http_compression => true
	}
}

//Test your Logstash configuration with this command:
$ sudo service logstash configtest

//It should display Configuration OK if there are no syntax errors. Otherwise, try and read the error output to see what's wrong with your Logstash configuration.

//Restart Logstash, and enable it, to put our configuration changes into effect:
$ sudo service logstash restart


