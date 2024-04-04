# Configureren van Security Onion
## Deployen Van Agents
Het Deployen van agents op hosts en endpoints gebeurd via een elastic agent install bestand. Voor elk Operating System bestaat er andere installer. 
Deze installers moet je via het SOC-dashboard gedowload worden en vervolgens uitgevoerd als administrator op windows of als root op linux/MacOS. 
Ook is het aangeraden op een geschikte hostname te kiezen voor het apparaat , want de agent gebruikt de hostname van de host als naam in de SIEM. 

Naast het installeren van de agents op elke gewenste host , is het ook belangrijk om de firewall rules van de SIEM goed te zetten. 
Doe je dit niet dan zal de agent niet bereikbaar zijn.

## Aanmaken en instellen van filter rules (OS-query)

