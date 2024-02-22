ELK STACK vs Security Onion vs Apache Metron vs OSSEC

# ELK STACK:
Gebruikte technologieen: Elastic search , Logstash , Kibana 
Logverzameling: via Logstash = (syslog ,beats, ...)
=> security Onion en wazuh maken hier gebruik van. 


# Security Onion 
Gebruikte technologieen: Suricata , Snort , Zeek , Elastic Search, kibana 
Logverzameling: Suricata , Snort (syslog , host-based logs)
voordelen:
- ALl-in-one (zoals bv. Netwerk beveiliging en monitoring)
- Geavanceerde netwerkmonitoring doormiddel van Suricata en Snort=IDS

nadelen:
- Resource-intensief

# Graylog
Gebruikte technologieen:  Elasticsearch , MongoDB , Graylog 
Logverzameling: eigen graylog collecter
voordelen:
- Flexibel in verschillende scenarios
- Sterke filter mogelijkheden in loganalyse
- Relatief eenvoudig op te zetten

nadelen:
- Minder schaalbaar , problemen bij grotere implementaties
- Minder support

# AlienVault OSSIM
Gebruikte technologieen:  Elasticsearch , OpenVAS , Snort
Logverzameling: logs uit syslog SNMP , ... 
voordelen:
- Veel technolgieen geintgreerd 
- ingebouwde beveilingsregels => threat dectection


nadelen:
- Niet alle functies open soure

# Wazuh
Gebruikte technologieen: Elasticsearch , logstash Kibana
Logverzameling: via Logstash = (syslog ,beats, ...) 
voordelen:
- Host-based en netwerkmonitoring
- simpele implementatie
- Integratie met Elastic Stack

nadelen:
- beperkte schaalbaarheid
- moeilijker in regelcreatie


# Vereiste gemeente infrastructuur
- frequente updates
- schaalbaar
- open source
- zoveel mogelijk technologieen ondersteunen en kunnen intgreren
- hoe verzamelen ze logs ?
- gebruikte technologieen in de siem

guldentops zegt wazuh
vermonden zegt security onion => uitgebreider wazuh is een subset hiervan
--> resource intensief 
