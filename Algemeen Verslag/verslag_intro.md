# Welke SIEM? (alternatieven vergelijken) 
Er zijn verschillende soorten SIEMS op markt. Sommige zijn open-source andere weer niet. Voor dit project moet de SIEM aan volgende vereisten  voldoende:
- frequente updates
- schaalbaar
- open-source
- zoveel mogelijk technologieen ondersteunen en kunnen intgreren

Bij onderzoek werden volgende Open-source SIEMS vergeleken
- Graylog
- Security Onion
- Alienvault OSSIM
- Wazuh

## Graylog
Graylog is een SIEM met zijn eigen logcollector die gebruikt maakt van verschillende Technologieën waaronder Elasticsearch en MongoDB.

De SIEM is zeer flexibel in verschillende scenarios en relatief eenvoudig op te zetten. Daarnaast biedt het extreem veel filter mogelijkheden voor geavanceerde loganalyses te kunnen uitvoeren. 
Echter is deze SIEM niet de beste keuze aangezien er problemen kunnen optreden bij grotere implementaties. Verder is ook niet veel support beschikbaar voor deze SIEM. 

## AlienVault OSSIM
OSSIM is veel geavanceerdere SIEM T.O.V. Graylog. Naast het gebruik van ELasticsearch maakt de SIEM ook gebruikt van onder meer Snort en OpenVAS. Hierdoor is mogelijk om meer logs binnen het netwerk te verzamelen en ook aan vulnerability assesment te doen. Jammer genoeg zijn enkele functies betalend waardoor je SIEM niet volledig open-source kunt gebruiken. 

## Security Onion
Security Onion is een all-in-one SIEM die zeer diverse technologieën "zoals Suricata , snort , zeek , ..." combineerd tot één geheel. De SIEM is netwerk gefocust waardoor je hele netwerk zeer goed kunt monitoren. Daarnaast biedt de SIEM ook Host-based functies waarmee de Hosts kunnen gemonitord worden. 

Echter heeft de SIEM één groot nadeel, Hij gebruikt zeer veel resources (200GB opslag , 16GB RAM 4CPU cores). Maar dit compenseerd de SIEM wel met zijn mogelijkheden. Idealiter zouden wij deze SIEM willen gebruiken , maar moest het niet mogelijk zijn omwille van de resources gebruiken we Wazuh als alternatief. 

Testen hiermee is ook lastig want de meeste van ons hebben deze resources niet zomaar tot beschikking. 

## Wazuh

Wazuh is een SIEM waarmee we hebben kennisgemaakt binnen de school. Het is Redelijke eenvoudige SIEM die met behulp ELK-stack zijn logs genereerd en analyseerd. De SIEM zelf is zeer lightweight en is Integreerbaar met verschillende andere Technologieën. Belangrijk is wel op te merken dat Wazuh een beperkte schaalbaarheid heeft en het soms moeilijk kan zijn om de juiste regels te creëren.  



# Hoe mobile clients controleren? (EDR) 

 

 

# Hoe logs van switches en routers opvragen? (syslog) 

Syslog laat een netwerk node toe om logs van zijn systeem naar een ander systeem te sturen dat een syslog daemon draait, deze syslog service slaagt de logs op in bestanden. Hiervoor is er een server nodig die een syslog daemon draait, deze logs moeten dan worden doorgestuurd naar onze SIEM server door middel van een client agent of door de syslog daemon op de SIEM server zelf te laten draaien. Elke router en switch moet worden ingesteld zodat ze hun logs versturen naar de centrale server. 

 
*Procedure om syslog op switches en routers in te schakelen:* 
Er zijn meerdere opties om de logs van switches en routers naar een centraal systeem te sturen.  

We kunnen SNMP gebruiken om syslogs van het netwerk apparaat door te sturen naar de snmp-server host. Deze snmp server zou in principe op de SIEM kunnen draaien, hiervoor moeten de syslogs als traps worden verstuurd naar de snmp server. Traps zijn pakketjes die onmiddelijk bij het aanmaken worden doorgestuurd.

We kunnen ook ansible gebruiken om zo voor elke vendor een aparte playbook te schrijven die de syslogs direct doorstuurt naar de SIEM server. Voor Cisco switches bestaat er bijvoorbeeld een ansible module die ons toelaat dit in te stellen.

Of elke switch en router wordt manueel ingesteld om zijn logs te sturen naar de SIEM server. Dit vereist heel veel manueel werk en laat ons niet toe om de omgeving snel op te zetten.

https://www.cisco.com/c/en/us/td/docs/switches/metro/me1200/controller/guide/b_nid_controller_book/b_nid_controller_book_chapter_010101.pdf 

# Hoe OVA up to date houden? 