# IoTLEDBot
By Michael Smidt

Updated 1-10-2025

## Introductie
Met deze manual leer je zelf een LED-strip aan te sturen met je telefoon en Telegram. Je kunt dan via commando's te sturen aan een bot je LED-strip inschakelen en van kleur laten veranderen. Je hebt hiervoor het volgende nodig:

1. NodeMCU board, ESP8266
2. Arduino IDE
3. LED-strip
4. Smartphone met de Telegram-app (let op: voor je account heb je een werkend telefoonnummer nodig)
5. Wifi-verbinding.

Voor deze manual wordt uitgegaan dat je de IDE en de drivers voor de NodeMCU al hebt geïnstalleerd. 

## Stap 1: Setup Telegram
Open Telegram op je smartphone en volg de stappen om je account aan te maken. Vervolgens op het hoofdmenu, klik op het zoek-icoontje en typ BotFather in. Selecteer vervolgens het BotFather account. Let erop dat het account een blauw vinkje heeft.
![](../IoTLEDBot/Images/BotFather.png)

Nadat je op start drukt krijg je een lijst met commando's om te geven aan BotFather. We gaan een nieuwe bot maken. Daarvoor gebruiken we het commando `/newbot`. Volg de stappen die BotFather geeft.
![](../IoTLEDBot/Images/BotSetup.png)

Als alles is gelukt, krijg je de nodige info voor je bot:
![](../IoTLEDBot/Images/SuccessBot.png)

Kopieer de HTTP API token op een veilige plaats. **Deel dit niet met anderen; ze kunnen je bot dan gebruiken zonder jouw toestemming.** Open vervolgens de bovenste link. Je bot is nu klaar om te gebruiken.

## Stap 2: Setup Arduino
Om deze applicatie te maken heb je 3 libraries nodig: UniversalTelegramBot, ArduinoJson, en Adafruit NeoPixel. 

Open de Arduino IDE en klik op het derde icoontje in de linker zijbalk. Dit opent je Library Manager. 
![](../IoTLEDBot/Images/Libraries.png)

In de zoekbalk, typ Adafruit NeoPixel. Klik op install en vervolgens Install All. Doe hetzelfde voor UniversalTelegramBot; deze installeert ook ArduinoJson.

Als alles goed is gegaan, kun je de libraries filteren op Installed en zie je het onderstaande.
![](../IoTLEDBot/Images/Installed.png)

## Stap 3: Wifi verbinden
Herstart de Arduino IDE. Ga vervolgens naar File > Examples > UniversalTelegramBot > ESP8266 > EchoBot. Dit openent een nieuw scherm.

Vul vervolgens je SSID en je Wifi-wachtwoord in bij het onderstaande stuk code. De BOT_TOKEN is de HTTP API token de je van de BotFather hebt gekregen. 
![](../IoTLEDBot/Images/Wifi.png)

Nadat je dit hebt ingevuld, open je de Serial Monitor. Zet die op 115200 baud rechtsonder.
![](../IoTLEDBot/Images/Serial1.png)

Sluit je NodeMCU aan en upload de sketch naar je microcontroller. In de Serial Monitor kun je vervolgens zien of de Wifi-verbinding juist tot stand komt. 
![](../IoTLEDBot/Images/Serial2.png)

Als het goed is, kan jouw bot nu communiceren met de NodeMCU. Stuur een testbericht met je smartphone; hij stuurt een exacte kopie terug!
![](../IoTLEDBot/Images/Test.png)

We kunnen nu je LED-strip aansluiten.

## Stap 4: LED controle
Koppel je NodeMCU los van je PC en sluit de LED-strip aan. De strip heeft 3 draden: geel, rood, en zwart. Het gele draad sluiten we aan op de D1 pin, rood op een 3V pin, en zwart op een GND pin. 

Nu moeten we ervoor zorgen dat de strip aan te sturen is met code. Scroll omhoog tot je `#include <UniversalTelegramBot.h>` ziet. Hieronder plaatsen we de NeoPixel library:

![](../IoTLEDBot/Images/Load%20headers.png)
*Let op spelling en hoofdletters!*

De strip moet nu worden aangemaakt in de code. Scroll tot je `unsigned long bot_lasttime;` tegenkomt. Hieronder gaan we de strip configureren: 
![](../IoTLEDBot/Images/Define.png)

`NUMPIXELS` verwijst hier naar hoeveel lampjes lang je LED-strip is. In dit voorbeeld gebruiken we een strip van 12 lampjes lang.

Nu moet de strip worden aangezet wanneer de microcontroller opstart. Plaats dit als derde regel in de `setup`:

![](../IoTLEDBot/Images/setup.png)

Vervolgens maken we een functie aan om de LED-strip in te stellen. We noemen deze `setColor()`. Scroll tot onder `HandleIncomingMessages` en plaats daar het volgende:

![](../IoTLEDBot/Images/Setpixels.png)

We maken gelijk ook een functie aan voor een disco-mode. Deze functie, `discoEffect()` plaatsen we direct onder `setColor()`:

![](../IoTLEDBot/Images/Disco.png)

Nu moet de NodeMCU een functie krijgen om de berichten uit te lezen en daarmee de strip aan te sturen. We passen `HandleIncomingMessages` aan met een else-if statement:

![](../IoTLEDBot/Images/Handlemessages.png)

*Tip: Je kan ook zelf kleuren toevoegen! Zoek de RGB waardes op van je favoriete kleur en maak een extra else if statement aan met de naam van je kleur.*

Sluit je NodeMCU nu weer aan je PC en upload de sketch. Stuur een commando naar je bot en je LED-strip wordt vanzelf aangestuurd!


