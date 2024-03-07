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
Security Onion is een all-in-one SIEM die zeer diverse technologieën "zoals Suricata , snort , zeek , ..." combineerd tot één geheel. De SIEM is netwerk gefocust (routers/switches/Netwerk Traffic) waardoor je hele netwerk zeer goed kunt monitoren. Daarnaast biedt de SIEM ook Host-based functies waarmee de Hosts kunnen gemonitord worden. 

Echter heeft de SIEM één groot nadeel, Hij gebruikt zeer veel resources (200GB opslag , 16GB RAM 4CPU cores). Maar dit compenseerd de SIEM wel met zijn mogelijkheden. Idealiter zouden wij deze SIEM willen gebruiken , maar moest het niet mogelijk zijn omwille van de resources gebruiken we Wazuh als alternatief. 

Testen hiermee is ook lastig want de meeste van ons hebben deze resources niet zomaar tot beschikking. 

## Wazuh

Wazuh is een SIEM waarmee we hebben kennisgemaakt binnen de school. Het is Redelijke eenvoudige SIEM die met behulp ELK-stack zijn logs genereerd en analyseerd. De SIEM zelf is zeer lightweight en is Integreerbaar met verschillende andere Technologieën. Belangrijk is wel op te merken dat Wazuh een beperkte schaalbaarheid heeft en het soms moeilijk kan zijn om de juiste regels te creëren.

Wazuh is een host-based SIEM. Wat betekend dat de SIEM-oploosing zich focust op het bewaken en analyseren van de individuele hosts/endpoints binnen een netwerk. 

### conclusie
De beste keuze zou Security Onion zijn voor dit project. De SIEM heeft grote set aan tools tot zijn beschikking om het netwerk te monitoren. Het nadeel is dat deze SIEM-oplossing zeer veel resources vraagt. 

Indien het niet mogelijk is om deze resources vrij te maken zal de keuze eerder vallen op wazuh. Deze SIEM-oplossing zeker een goed alternatief voor Security Onion. Maar dan is het ook zeer belangrijk om te controleren waar deze SIEM te kortkoming heeft en deze op te lossen zijn. 


# Hoe mobile clients controleren? (EDR) 
Bij EDR gaan we gebruik maken van Osquery, deze wordt door zowel wazuh als security onion ondersteund. Wazuh heeft een ingebouwde osquery module voor de wauzh agents. Die staat toe de Osquery in te stellen en data te verzamelen die gegenereerd is door de Osquery en deze dan door te sturen naar de manager.

Bij security onion wordt er gebruik gemaakt van FleetDM. FleetDM is een opensource tool die wordt gebruikt als een centraal management platform gebruikt voor Osquery.

Securety onion maakt gebruik van Launcher als management wrapper rond osquery.
In Security Onion is "Launcher" een component of hulpprogramma ontworpen om de implementatie, configuratie en uitvoering van Osquery op uw netwerk of endpoints te beheren.
Een managementwrapper is een softwarelaag of -interface die rond een ander stuk software zit om extra beheermogelijkheden te bieden, waardoor de beheertaken met betrekking tot die software vereenvoudigd worden. 
Launcher bekijkt elk uur of er een update beschikbaar is, zo ja zal deze gedownload worden en geinstalleerd worden.

Het kibana dashboard gaat een overview geven van de osquery logs in het systeem.
## Osquery
Osquery is een makkelijk te gebruiken monitoring tool dat gebruik maakt van SQL om alle verkregen informatie van operating systems samen te vatten in een relationele database. 
Osquery ondersteund Windows, macOS en linux OS.
Het is snel en lightweight. Osquery is perfect om te gebruiken voor het monitoren van devices voor real-time data.
Enkele manieren voor vulnerability management met Osquery : 
- Check voor slechte security configuratie
- ongewilde malicious applicatie of extenties
- Controleer de status van firewalls verifieren
- Controleer op verouderde of kwetsbare besturingssysteem- en softwareversies.

### FleetDM
FleetDM, of Fleet, is een open-source tool ontwikkeld door Kolide. Het is een gecentraliseerd beheerplatform voor osquery, een open-source framework voor de beveiliging van endpoints dat query's en monitoring van endpoints mogelijk maakt met behulp van SQL-achtige query's. Fleet zorgt voor een webinterface, hier gaan we de date die we hebben verkregen met osquery op visualiseren. 
Fleet werkt met een query libary van vragen. Bijvoorbeeld :
Wat operating system is geinstalleerd op mijn apparaat?
```
1. Select Queries in the top navigation.
2. Select Create new query (or browse your organization's queries for "operating system information" in the search bar).
3. Type the query you would like to run, SELECT * FROM os_version;.
4. Select Run query, then select All hosts (your device may be the only host added to Fleet), and finally select Run to execute the query.
```
# Hoe logs van switches en routers opvragen? (syslog) 

Syslog laat een netwerk node toe om logs van zijn systeem naar een ander systeem te sturen dat een syslog daemon draait, deze syslog service slaagt de logs op in bestanden. Hiervoor is er een server nodig die een syslog daemon draait, deze logs moeten dan worden doorgestuurd naar onze SIEM server door middel van een client agent of door de syslog daemon op de SIEM server zelf te laten draaien. Elke router en switch moet worden ingesteld zodat ze hun logs versturen naar de centrale server. 

 
*Procedure om syslog op switches en routers in te schakelen:* 
Er zijn meerdere opties om de logs van switches en routers naar een centraal systeem te sturen.  

We kunnen SNMP gebruiken om syslogs van het netwerk apparaat door te sturen naar de snmp-server host. Deze snmp server zou in principe op de SIEM kunnen draaien, hiervoor moeten de syslogs als traps worden verstuurd naar de snmp server. Traps zijn pakketjes die onmiddelijk bij het aanmaken worden doorgestuurd. Security Onion bevat zelf een snmp agent, bij wazuh zou deze nog eens opgezet moeten worden.

We kunnen ook ansible gebruiken om zo voor elke vendor een aparte playbook te schrijven die de syslogs direct doorstuurt naar de SIEM server. Voor Cisco switches bestaat er bijvoorbeeld een ansible module die ons toelaat dit in te stellen. Dit vereist verschillende playbooks per vendor en is dus een complexer alternatief.

Of elke switch en router wordt manueel ingesteld om zijn logs te sturen naar de SIEM server. Dit vereist heel veel manueel werk en laat ons niet toe om de omgeving snel op te zetten.

Switches en Routers moeten worden opgezet om alle logs naar de siem te versturen. Als we het netwerk monitoren met Zeek, dan zal deze alle syslogs loggen ookal waren deze niet voor deze SIEM bedoelt.


# Hoe OVA up to date houden? 

Er wordt een ansible file geschreven die controleert of we de meest recente versie van de SIEM hebben, bij een outdated versie pullen we de nieuwste versie en wordt deze lokaal binnengehaald. De OVA wordt dan gedeployed en alle ansible scripts die nodig zijn om het netwerk correcct in te stellen worden uitgevoerd.

# Werking van gekozen SIEM

## SIEM schema
![alt text for screen readers](SIEM2.png)
## Waar schiet de SIEM te kort ? 

### eventuele oplossingen voor deze tekortkomingen 


# Bronnen
### wazuh
https://wazuh.com/
### Security Onion
https://securityonionsolutions.com/
### OSSIM
https://cybersecurity.att.com/
### Graylog
https://graylog.org/
### Syslog
https://www.cisco.com/c/en/us/td/docs/switches/metro/me1200/controller/guide/b_nid_controller_book/b_nid_controller_book_chapter_010101.pdf 
### Osquery
https://fleetdm.com/guides/osquery-a-tool-to-easily-ask-questions-about-operating-systems
https://docs.securityonion.net/en/2.3/osquery.html
### FleetDM
https://fleetdm.com/docs/using-fleet/learn-how-to-use-fleet
https://medium.com/@itdanny/security-onion-part-2-tools-1cd95e350811
https://docs.securityonion.net/en/2.3/fleet.html#fleet





