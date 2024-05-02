# Configureren van Security Onion
## Deployen Van Agents
Het Deployen van agents op hosts en endpoints gebeurd via een elastic agent install bestand. Voor elk Operating System bestaat er andere installer. 
Deze installers moet je via het SOC-dashboard gedowload worden en vervolgens uitgevoerd als administrator op windows of als root op linux/MacOS. 
Ook is het aangeraden op een geschikte hostname te kiezen voor het apparaat , want de agent gebruikt de hostname van de host als naam in de SIEM. 

Naast het installeren van de agents op elke gewenste host , is het ook belangrijk om de firewall rules van de SIEM goed te zetten. 
Doe je dit niet dan zal de agent niet bereikbaar zijn.

## Verwijderen Van Agents
Moest er foutieve agents zijn gedeployed op een endpoints/host , dan kun je die op onderstaande manier verwijderen. Dit wordt aangeraden aangezien de agents nogal veel resources gebruiken zelfs als ze geen connectie hebben met de SOC. 

### Windows

1. **Open een PowerShell-prompt als Administrator**: Rechtsklik op het PowerShell-pictogram en selecteer "Uitvoeren als administrator".

2. **Voer vanuit de PowerShell-prompt het volgende commando uit**:
   ```shell
   C:\"Program Files"\Elastic\Agent\elastic-agent.exe uninstall
   ```

### Linux 


1. Open een terminalvenster.

2. Voer het volgende commando uit om het Elastic Agent verwijderingsproces te starten:
   ```bash
   sudo /opt/Elastic/Agent/elastic-agent uninstall
   ```
3. Voer je beheerderswachtwoord in wanneer daarom wordt gevraagd (let op: je ziet mogelijk geen tekens terwijl je het wachtwoord invoert, maar het wordt wel geregistreerd).

### MacOs

Hier zijn de stappen voor het verwijderen van Elastic Agent op macOS met behulp van het `sudo` commando:

1. Open de Terminal. Dit kun je vinden in de map 'Programma's' onder 'Hulpprogramma's', of je kunt het zoeken met Spotlight (Cmd + Spatie en typ "Terminal").

2. Voer het volgende commando in en druk op Enter:
   ```bash
   sudo /Library/Elastic/Agent/elastic-agent uninstall
   ```

3. Voer je beheerderswachtwoord in wanneer daarom wordt gevraagd (let op: je ziet mogelijk geen tekens terwijl je het wachtwoord invoert, maar het wordt wel geregistreerd).


## Herstarten Van Agents
Moesten er agents in de fleet server een andere state hebben dan healty kun je altijd proberen de agents een keer de restarten. Bij dsync kunnen er af en toe state problemen optreden een restart zou dit moeten fixen. 

### Windows

1. **Open een PowerShell-prompt als Administrator**: Rechtsklik op het PowerShell-pictogram en selecteer "Uitvoeren als administrator".

2. **Voer vanuit de PowerShell-prompt het volgende commando uit**:
   ```shell
   C:\"Program Files"\Elastic\Agent\elastic-agent.exe restart
   ```

### Linux 


1. Open een terminalvenster.

2. Voer het volgende commando uit om het Elastic Agent verwijderingsproces te starten:
   ```bash
   sudo /opt/Elastic/Agent/elastic-agent restart
   ```
3. Voer je beheerderswachtwoord in wanneer daarom wordt gevraagd (let op: je ziet mogelijk geen tekens terwijl je het wachtwoord invoert, maar het wordt wel geregistreerd).


### MacOs

Hier zijn de stappen voor het verwijderen van Elastic Agent op macOS met behulp van het `sudo` commando:

1. Open de Terminal. Dit kun je vinden in de map 'Programma's' onder 'Hulpprogramma's', of je kunt het zoeken met Spotlight (Cmd + Spatie en typ "Terminal").

2. Voer het volgende commando in en druk op Enter:
   ```bash
   sudo /Library/Elastic/Agent/elastic-agent restart 
   ```

3. Voer je beheerderswachtwoord in wanneer daarom wordt gevraagd (let op: je ziet mogelijk geen tekens terwijl je het wachtwoord invoert, maar het wordt wel geregistreerd).


## Aanmaken en instellen van filter rules (OS-query)

