
# Elasticsearch Installation: #
* https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html
  > wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
* Installing from the APT repositoryedit
  > sudo apt-get install apt-transport-https
* Save the repository definition to /etc/apt/sources.list.d/elastic-5.x.list:
  > echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
* You can install the Elasticsearch Debian package with:
  > sudo apt-get update && sudo apt-get install elasticsearch

  > sudo /bin/systemctl daemon-reload
  > sudo /bin/systemctl enable elasticsearch.service

  > sudo systemctl start elasticsearch.service
  > sudo systemctl stop elasticsearch.service

* https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html
* rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
* Create a file called elasticsearch.repo in the /etc/yum.repos.d/ directory for RedHat based 
  > [elasticsearch-5.x]
  > name=Elasticsearch repository for 5.x packages
  > baseurl=https://artifacts.elastic.co/packages/5.x/yum
  > gpgcheck=1
  > gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
  > enabled=1
  > autorefresh=1
  > type=rpm-md

* sudo yum install elasticsearch

  > sudo /bin/systemctl daemon-reload
  > sudo /bin/systemctl enable elasticsearch.service

  > sudo systemctl start elasticsearch.service
  > sudo systemctl stop elasticsearch.service
  
* To tail the journal:
  > sudo journalctl -f
* To list journal entries for the elasticsearch service:
  > sudo journalctl --unit elasticsearch
* To list journal entries for the elasticsearch service starting from a given time:
  > sudo journalctl --unit elasticsearch --since  "2016-10-30 18:17:16"

# Kibana Installation: #
* https://www.elastic.co/guide/en/kibana/current/rpm.html
* rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
  >Create a file called kibana.repo in the /etc/yum.repos.d/ directory for RedHat based distributions

  >[kibana-5.x]
  >name=Kibana repository for 5.x packages
  >baseurl=https://artifacts.elastic.co/packages/5.x/yum
  >gpgcheck=1
  >gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
  >enabled=1
  >autorefresh=1
  >type=rpm-md
 * Install and create service 
  > sudo yum install kibana
  > sudo /bin/systemctl daemon-reload
  > sudo /bin/systemctl enable kibana.service
  > sudo systemctl start kibana.service
  > sudo systemctl stop kibana.service
  
* https://www.elastic.co/guide/en/kibana/current/deb.html
  >wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
* Installing from the APT repositoryedit
  >sudo apt-get install apt-transport-https
* Save the repository definition to /etc/apt/sources.list.d/elastic-5.x.list:
    >echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" 
    >sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
    >sudo apt-get update && sudo apt-get install kibana
    >sudo /bin/systemctl daemon-reload
    >sudo /bin/systemctl enable kibana.service
    >sudo systemctl start kibana.service
    >sudo systemctl stop kibana.service
    
# Setting Up SSL/TLS on a Cluster
  * https://www.elastic.co/guide/en/x-pack/current/ssl-tls.html

# Setting Up User Authentication
  * https://www.elastic.co/guide/en/x-pack/current/setting-up-authentication.html#setting-up-authentication

# How Authentication Works
  * https://www.elastic.co/guide/en/x-pack/current/_how_authentication_works.html 

# PKI User Authentication
  * https://www.elastic.co/guide/en/x-pack/current/pki-realm.html

# Native User Authentication
  * https://www.elastic.co/guide/en/x-pack/current/native-realm.html

# Elasticsearch.yml
##sudo nano /etc/elasticsearch/elasticsearch.yml
### sudo systemctl stop elasticsearch.service
### sudo systemctl start elasticsearch.service
### sudo systemctl status elasticsearch.service

      network.host: "your fully qualified DNS name"
      xpack.ssl.key:                     /etc/elasticsearch/x-pack/node1/node1.key
      xpack.ssl.certificate:             /etc/elasticsearch/x-pack/node1/node1.crt
      xpack.ssl.certificate_authorities: [ "/etc/elasticsearch/x-pack/ca/ca.crt" ]
      xpack.security.transport.ssl.enabled: true
      xpack.security.http.ssl.enabled: true
      xpack.security.http.ssl.client_authentication: required

       xpack.security.authc.realms:
        pki1:
          type: pki
          order: 0
        native:
          type: native
          order: 1
# Kibana.yml
## sudo nano /etc/kibana/kibana.yml
### sudo systemctl stop kibana.service
### sudo systemctl start kibana.service
### sudo systemctl status kibana.service

      elasticsearch.ssl.cert: /etc/elasticsearch/x-pack/client/client.crt
      elasticsearch.ssl.key: /etc/elasticsearch/x-pack/client/client.key

      To disregard the validity of SSL certificates, change this setting's value to false.
      elasticsearch.ssl.verify: false

      server.ssl.key:                     /etc/elasticsearch/x-pack/node1/node1.key
      server.ssl.cert:                   /etc/elasticsearch/x-pack/node1/node1.crt
      elasticsearch.ssl.ca:              [ "/etc/elasticsearch/x-pack/ca/ca.crt" ]


# Role_mapping.yml
    Example: 
      superuser:
        - "CN=nest"
        - "CN=esclient.mnit.com"
