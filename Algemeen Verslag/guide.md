# Installatie 
Security onion biedt meerdere installaties , in deze installatie wordt enkel security onion standalone besproken. Heb je interresse in een andere installatie van security dan kun je meer informatie vinden op [Security Onion installaties ] (https://docs.securityonion.net/en/2.4/architecture.html).

## Hardware requirements
Voor de standalone installatie zijn volgende requirements nodig: 
![requirements.png](./afbeeldingen/requirements.png)

Bovenstaande requirements zijn noodzakelijk anders zal de installatie falen. Meer resources zijn altijd wenselijk ze zullen de SIEM betere prestaties doen geven.

## installatie proces
Onderstaande afbeeldingen zijn alle stappen die u moet doornemen om security onion succesvol te installeren. Hou rekening ermee dat het installatie proces van security onion lang kan duren en sommige instelling enkel wijzigbaar zijn met herinstallatie. 

![install1.png](./afbeeldingen/install1.png)
Kies "yes"

![install2.png](./afbeeldingen/install2.png)
Kies "install" 

![install3.png](./afbeeldingen/install3.png)
Voor verschillende doeleinden kun je hier verschillende versie van security onion kiezen. Voor deze installatie wordt standalone gekozen. 

Deze versie kan op zijn eigen draaien in productie omgeving en heeft de meeste features actief. Enkele features zoals een Honeypot zitten hier niet in.

![install4.png](./afbeeldingen/install4.png)
Kies  "standard" 

![install5.png](./afbeeldingen/install5.png)
type "agree"

![install6.png](./afbeeldingen/install6.png)
Kies een FQDN voor de security onion installatie. Let op deze FQDN kan na installatie niet meer gewijzigd worden!

![install7.png](./afbeeldingen/install7.png)
Deze waarschuwing komt enkel de default FQDN wordt gekozen. Indien je dit wenst kies "Use Anyway"

![install8.png](./afbeeldingen/install8.png)
Kies een NIC voor als management interface.

![install9.png](./afbeeldingen/install9.png)
Kies "static IP". Let op dit ip wordt ook gebruikt in alle docker containers en is niet meer wijzigbaar! 

![install10.png](./afbeeldingen/install10.png)
Kies een IP met subnetmask voor security Onion. 

![install11.png](./afbeeldingen/install11.png)
Kies een default gateway.

![install12.png](./afbeeldingen/install12.png)
Kies één of meerdere DNS servers die security onion moet gebruiken. 

![install13.png](./afbeeldingen/install13.png)
Kies een DNS suffix die security onion moet gebruiken.

![install14.png](./afbeeldingen/install14.png)
Kies "yes" 

![install15.png](./afbeeldingen/install15.png)
Kies "Direct" indien u geen proxy wenst te gebruiken.

![install16.png](./afbeeldingen/install16.png)
Kies een email adres die als root accout wordt aangemaakt voor security onion dashboard en Kibana. Dit moet geen bestaande email zijn. Let op de root account kan niet gewijzigd worden!

![install17.png](./afbeeldingen/install17.png)
Kies een wachtwoord voor het root account. 

![install18.png](./afbeeldingen/install18.png)
Bevestig het wachtwoord voor het root account.

![install19.png](./afbeeldingen/install19.png)
Kies hoe je het security onion wilt bereiken. Let op indien u hostname kiest moet er een DNS server bestaan die het IP van Security Onion kan vertalen. Security Onion moet ook deze DNS server ingesteld hebben staan!

![install20.png](./afbeeldingen/install20.png)
Kies "yes"

![install21.png](./afbeeldingen/install21.png)
Kies een IP range die aan het security onion dashboard mag geraken. Kies 0.0.0.0/0 als eender welk IP mag connecteren naar dit dashboard. 

![install25.png](./afbeeldingen/install25.png)
kijk de samenvatting na en kies "OK".

![install22.png](./afbeeldingen/install22.png)
Kies "ok"

![install23.png](./afbeeldingen/install23.png)
Navigeer naar het IP/hostname dat gekozen werd via de browser. Log in met het aangemaakte root account.

![install24.png](./afbeeldingen/install24.png)
Na login zou je bovenstaand scherm moeten bekomen als de installatie succesvol is geslaagd. 





----
# Firewall
## configuratie firewall

----
# Elastic Agents
## Deployen Van Agents
Het Deployen van agents op hosts en endpoints gebeurd via een elastic agent install bestand. Voor elk Operating System bestaat er andere installer. 
Deze installers moet je via het SOC-dashboard gedowload worden en vervolgens uitgevoerd als administrator op windows of als root op linux/MacOS. 
Ook is het aangeraden op een geschikte hostname te kiezen voor het apparaat , want de agent gebruikt de hostname van de host als naam in de SIEM. 
De installatie kan Zowel manueel als automatisch , de automatische install gebeurd via ansible.

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

## Verwijderen van agents uit Fleet
In Kibana bevindt zich een Fleet Server, waar alle ingezette agents en hun status worden weergegeven. Je kunt hier naartoe navigeren via het Security Onion-dashboard en vervolgens op "Elastic Fleet" klikken. Als je agents in Fleet hebt die niet langer actief moeten zijn, volg dan deze eenvoudige stappen:

1. Ga naar de agent die je wilt stoppen.
2. Klik op de drie bolletjes om de instellingen van de agent te openen.
3. Kies "Unenroll agent".

Na deze stappen is de agent niet langer actief in de Fleet Server.

----
# Agent Policy's
Een policy is een soort standaard met regels die je kunt toevoegen aan Elastic Agents die geïnstalleerd zijn op de end-points binnen een netwerkomgeving. 
De policy bepaalt welke gegevens de agent zal verzamelen en zal terugsturen naar het centraal management platform. 
Elke Elastic agent policy heeft een set van integrations. Deze integrations bepalen welke data er zal worden opgehaald uit elke Elastic agent.

## Aanmaken en instellen van Elastic Agent Policies
Op het security onion dashboard, druk op het navigatiemenu aan de linkerzijde. Onder tools druk op ‘Elastic Fleet’. Dit zal je rechtstreeks brengen naar de Elastic Fleet.

Dit is het centrale management platform dat gebruikt wordt voor het bedienen van de Elastic agents. Op het scherm zijn hier alle agents te zien die in het netwerk zijn geïnstalleerd. 
Ga naar agent policies, hier kunt u alle reeds aangemaakte agent policies vinden.
## Aanmaken windows policy
1. Druk op Create agent policy
![create](./afbeeldingen/create.png)
2. Geef de gewenste naam aan de agent policy (bv: Windows policy)
3. Onder advanced options kunt u nog een omschrijving toevoegen naar wens. De andere opties stelt u in naarmate uw toebehoren. In onze situatie hebben wij gekozen voor de default instellingen.
![aanmaak](./afbeeldingen/voorbeeldaanmaak.png)

### pfsense

### cisco


----
# Certificaten
## Instellen van Certificaten

----
# ansible scripts

----
# manueel updaten

----
# SSH
## manueel toevoegen ssh keys

## ssh config

----
# Known Issue's







