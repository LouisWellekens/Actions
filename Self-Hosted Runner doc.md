# Handleiding Github Actions: Self hosted runner installeren

# 1.1 VM Installeren

Maak binnen VirtualBox een Ubuntu(64bit) VM aan.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.1%20VM.png)


De virtual machine heeft  de volgende Specs wanneer we hem de eerste keer opstarten.
Belangrijk: kies de 22.04 versie van Ubuntu en geef zeker 4GB RAM.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.2%20VM%20Specs.png)

Doorloop de installatie en selecteer de normale Ubuntu Server

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.3.png)

Blijf de standaardinstellingen volgen en kies hier voor Continue.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.4.png)

Kies hier voor een username en paswoord

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.5.png)

Hier kiezen we voor OpenSSH te installeren.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.6.png)

Kies hier voor Docker, we gaan dit nu nog niet gebruiken maar later eventueel wel.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.7.png)

We kunnen inloggen en krijgen toegang tot de CLI

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/1.8.PNG)

# 1.2 VM Configureren

Run het volgende commando (paswoord ingeven)

```

sudo apt update && sudo apt upgrade -y

```

Kies hier voor Ok

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.1.PNG)



# 1.3 Github Actions configureren
Nu keren we even terug naar Github waar we de stappen gaan volgen om de runner binnen een repository te creëren.

Voor dit voorbeeld heb ik gewoon de gedeelde OPS repository gebruikt.

Kies bovenaan voor Settings

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.2.PNG)

Navigeer naar Actions -> Runners -> New Self-hosted runner

Hier kan je ook meteen de reeds toegevoegde runners zien en hun status

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.3.PNG)

Hier kiezen we bovenaan voor Linux en x64

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.4.PNG)

Kopieer hierbij de commando's van GitHub in volgorde (makkelijker met SSH)

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.5.PNG)

Eenmaal we aan configure komen krijgen we het volgende scherm

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.6.PNG)

Vanaf dit punt is de installatie voltooid en kunnen we verder configureren

De runner group laten we (voor nu) op Default.

Als naam kies ik hier voor selfhost2.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.7.PNG)

De labels laat ik nu ook standaard op default.

De runner is succesvol geïnstalleerd ! De work folder laten we ook default staan.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.8.PNG)

Binnen Github zien we de runner nu actief staan. Maar nog offline

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.9.PNG)

Tenslotte runnen we volgend commando om de runner actief te zetten

```
./run.sh

```

Met als resultaat

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.10.PNG)

Ook binnen Github Actions staat de runner nu op Idle en niet meer op Offline
We kunnen nu Workflows laten lopen op deze aangemaakte runner.


![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.11.PNG)


Binnen de .Net Repository staan 2 Workflows klaar voor te testen

De Hello World echoot gewoon Hello World
De .NET workflow test een .NET Applicatie


![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.12.PNG)

Probeer beide workflows eens manueel uit. Wanneer ze voltooien krijgen ze een groen vinkje.

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.13.PNG)

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.14.PNG)

Op de server zelf krijgen we deze berichten

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.15.PNG)


# 1.4 VM heropstarten en online brengen

De runners moeten na heropstarting opnieuw online gebracht worden.
Wanneer we een runner VM aflsuiten en opnieuw opstarten blijft de status als Offline.

Om dit op te lossen moeten we de volgende commando's uitvoeren.

Voor de commando's:

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.16.PNG)

Commando's:

```
cd actions-runner
./run.sh

```

Na de commando's

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.17.PNG)

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/2.18.PNG)

De runner is nu weer klaar om Jobs uit te voeren

# 1.5 Workflow uitgelegd

De workflows zijn yml files die uit verschillende jobs bestaan. 
Jobs zijn verschillende stappen die uitgevoerd worden op een runner.
Elke stap van een job is een Action of een script dat uitgevoerd worden. Deze worden stap per stap doorlopen.

De workflows worden getriggerd door acties binnen de repository.
Ze kunnen ook manueel getriggerd worden, of op bepaalde tijdstippen.

Actions zijn vooropgestelde scripten die vanuit de GitHub Marketplace geïmporteerd kunnen worden.

Workflows worden binnen de gepaste repository opgeslaan onder de workflows map zoals hier:

devops-22-23-ops-a02/.github/workflows/

! Workflows worden enkel uitgevoerd als ze zich in deze folder  bevinden.

Hieronder een voorbeeld van de workflow die een .NET applicatie bouwt en test.
Deze workflow is een aangepaste versie van een template die door github Actions zelf is opgebouwd.

Hier terug te vinden : https://github.com/HoGentTIN/devops-22-23-ops-a02/actions/new?category=none&query=.net

En dan de .NET core template van github Actions zelf !

![](/logboek/Devops%20Actions%20Imgs/Github%20Actions/3.1.PNG)

1. Begint met de naam van de workflow in dit geval.net

2. on: Specifieert wanneer de workflow moet runnen.

Workflow dispatch is toegevoegd om manueel de workflow te kunnen starten, verder heeft dit geen belang

De workflow wordt automatisch uitgevoerd bij Pull/Push requests op de main branch zoals je daaronder kan zien.

3. Hier begint de job build

    runs on : Specifieert op welke runner de jobs moet runnen, hier dus self-hosted
    env : is een variable die de dotnet directory specifieert

    Hier beginnen de stappen van de build.
    Deze job bestaat volledig uit actions die al op voorhand voor ons gebouwd zijn.

    de actions bestaan uit een naam (die aanpasbaar is en niet noodzakelijk)
    gevolgd door de action in dit geval begint het met: action/checkoutv3

    Deze actions zijn beschikbaar op de Github Actions marketplace.
    https://github.com/marketplace?type=actions

    Om te bekijken wat elke action juist doet kan je ze hier opzoeken.
    Bijvoorbeeld onze eerste action kan je hier terugvinden
    https://github.com/marketplace/actions/checkout

    








