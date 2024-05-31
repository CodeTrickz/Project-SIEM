# Installatie 
Security onion biedt meerdere installaties , in deze installatie wordt enkel security onion standalone besproken. Heb je interresse in een andere installatie van security dan kun je meer informatie vinden onder
[Security Onion installaties](https://docs.securityonion.net/en/2.4/architecture.html).

## Hardware requirements
Voor de standalone installatie zijn volgende requirements nodig: 
![requirements.png](./afbeeldingen/requirements.png)

Bovenstaande requirements zijn noodzakelijk anders zal de installatie falen. Meer resources zijn altijd wenselijk ze zullen de SIEM betere prestaties doen geven.
Requirements voor andere installaties hier te vinden onder [Security Onion requirements](https://docs.securityonion.net/en/latest/hardware.html).

## Installatie proces
Onderstaande afbeeldingen tonen alle stappen die u moet doorlopen om Security Onion succesvol te installeren. Houd er rekening mee dat het installatieproces van Security Onion lang kan duren en sommige instellingen alleen wijzigbaar zijn met herinstallatie.

![install1.png](./afbeeldingen/install1.png)
   - Kies "yes".

![install2.png](./afbeeldingen/install2.png)
   - Kies "install".

![install3.png](./afbeeldingen/install3.png)
   - Kies voor standalone voor deze installatie.
   - Opmerking: Deze versie kan zelfstandig draaien in een productieomgeving en heeft de meeste features actief. Enkele features, zoals een Honeypot, zijn hier niet inbegrepen.

![install4.png](./afbeeldingen/install4.png)
   - Kies "standard".

![install5.png](./afbeeldingen/install5.png)
   - Typ "agree".

![install6.png](./afbeeldingen/install6.png)
   - Kies een FQDN voor de Security Onion-installatie. Let op: deze FQDN kan na installatie niet meer gewijzigd worden!

![install7.png](./afbeeldingen/install7.png)
   - Deze waarschuwing verschijnt alleen als de standaard FQDN wordt gekozen. Kies "Use Anyway" indien gewenst.

![install8.png](./afbeeldingen/install8.png)
   - Kies een NIC voor de managementinterface.

![install9.png](./afbeeldingen/install9.png)
   - Kies "static IP".
   - Let op: dit IP wordt ook gebruikt in alle docker containers en is niet meer wijzigbaar!

![install10.png](./afbeeldingen/install10.png)
   - Kies een IP met subnetmask voor Security Onion.
     

![install11.png](./afbeeldingen/install11.png)
   - Kies een standaard gateway.

![install12.png](./afbeeldingen/install12.png)
   - Kies één of meerdere DNS-servers die Security Onion moet gebruiken.

![install13.png](./afbeeldingen/install13.png)
   - Kies een DNS-suffix die Security Onion moet gebruiken.

![install14.png](./afbeeldingen/install14.png)
   - Kies "yes".

![install15.png](./afbeeldingen/install15.png)
   - Kies "Direct" als u geen proxy wilt gebruiken.

![install16.png](./afbeeldingen/install16.png) 
   - Kies een e-mailadres dat als root account wordt aangemaakt voor het Security Onion-dashboard en Kibana.
   - Dit hoeft geen bestaand e-mailadres te zijn. Let op: het rootaccount kan niet gewijzigd worden!

![install17.png](./afbeeldingen/install17.png)
   - Kies een wachtwoord voor het rootaccount.

![install18.png](./afbeeldingen/install18.png)
   - Bevestig het wachtwoord voor het rootaccount.

![install19.png](./afbeeldingen/install19.png)
   - Kies hoe u Security Onion wilt bereiken.
   - Let op: als u 'hostname' kiest, moet er een DNS-server zijn die het IP van Security Onion kan vertalen. Security Onion moet ook deze DNS-server ingesteld hebben!

![install20.png](./afbeeldingen/install20.png)
   - Kies "yes".

![install21.png](./afbeeldingen/install21.png)
   - Kies een IP-range die toegang heeft tot het Security Onion-dashboard. Kies 0.0.0.0/0 als elk IP-adres toegang mag hebben tot dit dashboard.

![install25.png](./afbeeldingen/install25.png)
   - Controleer de samenvatting en kies "OK".

![install22.png](./afbeeldingen/install22.png)
   - Kies "OK".

![install23.png](./afbeeldingen/install23.png)
   - Navigeer naar het gekozen IP/hostname via de browser. Log in met het aangemaakte rootaccount.

![install24.png](./afbeeldingen/install24.png)
   - Als de installatie succesvol is, zou u het bovenstaande scherm moeten zien na het inloggen.





----
# Firewall
## configuratie firewall terminal
Tijdens de installatie van Security Onion heeft Docker in IP-tables een aantal firewallregels aangemaakt. Aan deze regels moet niets aangepast worden. Echter is er ook een firewalld-service aanwezig op de Security Onion waarin de regels van IP-tables niet aanwezig zijn die standaard actief is. Dit kan voor problemen zorgen daarom wordt deze service best gestopt en gedisabled.

### Commando's

```bash
# Stop de firewalld-service
sudo systemctl stop firewalld

# Schakel de firewalld-service uit zodat deze niet opnieuw start bij een reboot
sudo systemctl disable firewalld
```

## configuratie firewall Security Onion Dashboard
Bij Het Security Onion Dashboard is er wel bijkomende firewall configuratie nodig indien je wenst dat bepaalde docker containers instaat zijn om te communiceren met de SIEM. Default staat dit op een deny-all. 

1. Ga naar Administration.
2. Klik op Configuration.
3. Zoek firewall in de lijst en open de opties.
4. Open de optie hostgroups.
5. Zoek de functie waarvan je de firewall rules wilt wijzigen.
6. Klik op deze functie.
7. Vul Het IP of IP-range in die toegang mogen hebben tot deze functie. 
   - Indien de IP-range niet gekend is of eender welk IP aan deze functie mag vul 0.0.0.0/0 in.
   - Let op bij een IP-range moet het submask worden ingevuld in prefix-notatie zoals bijvoorbeeld /24.
8. Druk op het vinkje om de configuratie op te slaan.

Bij onderstaande afbeelding wordt de firewall configuratie aangepast van elastic agents.
![firewall.PNG](./afbeeldingen/firewall.PNG)

Meer uitleg over alle firewall functies is te vinden onder [Firewall Info](https://docs.securityonion.net/en/latest/firewall.html#).




----
# Elastic Agents
## Deployen Van Agents
Het Deployen van agents op hosts en endpoints gebeurd via een elastic agent install bestand. Voor elk Operating System bestaat er andere installer. 

De installatie kan zowel manueel als automatisch , de automatische install gebeurd via ansible of intune.

### Manuele installatie
De installers moeten via het SOC-dashboard worden gedownload en vervolgens worden uitgevoerd als administrator op windows of als root op linux/MacOS. 

Ook is het aangeraden om een geschikte hostname te kiezen voor het apparaat , want de agent gebruikt de hostname van de host als naam binnen de SIEM oplossing. 

### Automatisch

#### Intune
De installatie via intune staat volledig beschreven in de [intune handleiding](../Intune/intune.md), zo kan de al bestaande infrastructuur makkelijk worden geintegreerd in de SIEM.

#### Ansible
Als hosts beschikbaar zijn via ssh vanuit 1 ansible client dan is het installeren van de elastic agent heel simpel met 1 script. Dit vereist wel dat je op de ansible client het script lokaal hebt gedownload op een gekende locatie met liefst een duidelijk onderscheidbare naam.

Scripts om een Elastic Agent in te stellen via ansible zijn heel simpel en zien er als volgt uit:

      # Install windows script
      hosts: windows_hosts
      become: true

      tasks: 
      - name: Script installeren
        script: /path/naar/windowselasticagentinstaller.exe
---
      # Install Linux script
      hosts: linux_hosts
      become: true

      tasks: 
      - name: Script installeren
        script: /path/naar/linuxelasticagentinstaller.exe
Het pad naar de installer wordt vervangen door het werkelijke pad op de ansible client.

### Deployed
Eenmaal het script is geinstalleerd op de hosts zullen de logs binnen beginnen komen op de SIEM. Let wel op dat er steeds een verbinding moet zijn met de SIEM anders zal de install falen, aangezien de installer geen verbinding kan maken met de SIEM.

Naast het installeren van de agents op elke gewenste host , is het ook belangrijk om de firewall rules van de SIEM goed te zetten. 
Doe je dit niet dan zal de agent niet bereikbaar zijn.

Meer informatie omtrent Elastic agents is hier te vinden onder [Elastic agents info](https://docs.securityonion.net/en/latest/elastic-fleet.html).

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

## Aanmaken en instellen van Elastic Agent Policies/integrations 
Op het security onion dashboard, druk op het navigatiemenu aan de linkerzijde. Onder tools druk op ‘Elastic Fleet’. Dit zal je rechtstreeks brengen naar de Elastic Fleet.
![dashboard](./afbeeldingen/dashboard.png)
Dit is het centrale management platform dat gebruikt wordt voor het bedienen van de Elastic agents. Op het scherm zijn hier alle agents te zien die in het netwerk zijn geïnstalleerd. 
Ga naar agent policies, hier kunt u alle reeds aangemaakte agent policies vinden.

   1. Druk op Create agent policy.
![create](./afbeeldingen/create.png)
   2. Geef de gewenste naam aan de agent policy (bv: Windows policy, Linux policy,etc..).
   3. Onder advanced options kunt u nog een omschrijving toevoegen naar wens. De andere opties stelt u in naarmate uw toebehoren. In onze situatie hebben wij gekozen voor de default instellingen.
![aanmaak](./afbeeldingen/voorbeeldaanmaak.png)
Op deze moment heb jij jou eigen Elastic agent policy aangemaakt. Proficiat!
Nu is het tijd om over te gaan naar de volgende stap, het toevoegen van de integrations. Zoals al eerder aangehaald hierboven zullen de integrations bepalen welke data jou policy zal ophalen.
## Windows policy integrations
   1. Selecteer jou policy waar je integrations aan wilt toevoegen. In dit geval de Windows policy.
   2. Druk aan de rechterkant van de pagina op `Add integration`.
![add](./afbeeldingen/addint1.png)
   3.	Op deze pagina kunt u alle geïnstalleerde integrations vinden. In de zoekbalk kunt u specifieke integrations opzoeken naar keuzen.
![w1](./afbeeldingen/windowsint1.png)
   4.	In dit geval hebben wij gekozen voor de standaard `Windows` integration en de `Custom Windows Event Logs`. De laatste integration geeft de keuze om nog extra eigen inbreng te geven.
   5.	Selecteer de Windows integration.
   6.	Selecteer `Add Windows`.
![w2](./afbeeldingen/windowsint2.png)
   7. Geef een gepaste naam en omschrijving.
![w3](./afbeeldingen/windowsint3.png)
   8.	Verder kunt u de integration nog instellen naar keuze. Voor ons voldoen de standaard instellingen.
   9.	Kies op welke Agent policy u de integration wilt toevoegen. In dit geval de Windows policy.
![w4](./afbeeldingen/windowsint4.png)
   10. Save and continue.

Nu is de eerste Windows integration toegevoegd aan de policy.

De volgende integration die toegevoegd gaat worden is de `Custom Windows Event Logs` integration. Deze integration staat toe om te laten kiezen welke Windows Event logs er zullen worden opgehaald. 
Volg dezelfde stappen als hier boven.

- Aangekomen bij de instellingen van de integration, zul je een ‘Channel name’ moeten opgeven. Dit zal bepalen welke Windows Event logs er zullen worden opgehaald. 
![w5](./afbeeldingen/windowsint5.png)
Wij hebben gekozen voor de logs van de Windows powershell op te vragen. Verder volg de stappen zoals hier boven aangegeven om de integration toe te voegen.
Echter is het mogelijk bij deze integration channels te blijven toevoegen. Volg de stappen exact hetzelfde als hierboven om extra channels toe te voegen naar keuzen.
Meer informatie omtrent policy's is te vinden onder [Elastic agent policy](https://www.elastic.co/guide/en/fleet/current/agent-policy.html). 

## PFsense integration
De volgende integration gaan we toevoegen op een alternatieve manier. Bij het toevoegen van de integrations aan de Windows policy werd de integration direct toegevoegd via de policy. Nu gaan we dit bekijken via een andere manier.
1.	Op het Kibana dashboard druk Add integrations.
![pf1](./afbeeldingen/pfint1.png)
2.	Zoek de gewenste integration, in dit geval pfsense integration.
![pf2](./afbeeldingen/pfint2.png)
3.	Add Pfsense.
![pf3](./afbeeldingen/pfint3.png)
4.	Geef gewenste naam en omschrijving.
5.	Onder collect pfSense logs (input: udp). Verander Syslog Host van `localhost` naar `0.0.0.0`
![pf4](./afbeeldingen/pfint4.png)
6.	Voeg de integration toe aan de `so-grid-nodes_general` policy. Dit is de standaard policy die bij aanmaak van security onion mee wordt aangemaakt.

![pf5](./afbeeldingen/pfint44.png)
7.	Save and continue.

### PFsense configuratie
Voor het te laten werken van de Pfsense integration zullen er nog enkele dingen moeten worden aangepast in de Pfsense firewall.
1.	Navigeer naar status &#8594; System logs, druk op settings
![pf](./afbeeldingen/pfint5.png)
2.	Beneden aan de pagina, vink aan `Enable Remote Logging`
3.	Kies een specifieke interface voor forwarding
4.	Onder `Remote log servers` voer het IP van de security onion en poort 9001. De Elastic integration verwacht standaard dat de Pfsense logs op poort 9001.
5.	Onder `Remote Syslog Contents`selecteer de logs die je wilt forwarden naar de agent. Selecteer Everything.
![pf](./afbeeldingen/pfint6.png)
### Security onion configuratie
Vervolgens zal het verkeer van de pfsense moeten worden toegelaten op `poort 9001`.
1.	Ga naar Administration &#8594; Configuration
2.	Vanboven aan de pagina druk op de `options` menu en enable de `Show all configurable settings, including advanced settings.` optie.
![pf](./afbeeldingen/pfint7.png)
3.	Aan de linkerkant, ga naar `Firewall`, selecteer `hostgroup`, en druk `customhostgroup0` groep. Aan de rechterkant voer het IP adres van de Pfsense firewall in.
![pf](./afbeeldingen/pfint8.png)
4.	Aan de linkerkant, ga naar `Firewall`, selecteer `portgroups`, selecteer de `customportgroup0` en druk op `udp`. Voor `9001`in langs de rechterkant.
![pf](./afbeeldingen/pfint9.png)
5.	Aan de linkerkant ga naar `Firewall`, selecteer `role` en kies het node type dat de Pfsense logs zal ontvangen. In dit geval `standalone`.
6.	Vanaf hier Chain &#8594; INPUT &#8594; hostgroups &#8594; customhostgroup0 &#8594; portgroups.
7.	Aan de rechterkant, typ ` customgroup0`.
![pf](./afbeeldingen/pfint10.png)
8.	Onder het `options` menu. Druk op `SYNCHRONIZE GRID` voor de regels met onmiddellijk effect te laten ingaan.


Meer informatie over de pfsense integration kan hier worden geraadpleegt [PFsense configuratie](https://docs.securityonion.net/en/2.4/pfsense.html).

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
# Dashboard Users
## Aanmaken van Dashboard Users


----
# Dasboard data lezen
## Algemene Overview

## Agent data

## Firewall data

## andere filters

----
# Known Issue's


----






