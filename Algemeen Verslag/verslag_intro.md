# Waarom Wazuh? (alternatieven vergelijken) 

 

 

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
