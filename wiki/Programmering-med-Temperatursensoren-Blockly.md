Av alle sensorene til air:bit er temperatursensoren den enkleste. Temperatursensoren vi bruker i air:bit, DHT, er en veldig vanlig type sensor. Heldigvis betyr dette for oss at noen andre allerede har gjort jobben med å finne ut hvordan man leser av data fra sensoren. Dette gjør det veldig mye enklere å lese av temperaturdata.

Vi kommer til å skrive en veldig enkel Arduino Sketch, der vi skriver ut temperatur og fuktighetsmålinger over en seriell forbindelse.

## Komme i gang

Igjen vil vi starte med en helt fersk tom Sketch. Du kan klikke på `Discard` knappen for å slette den tidligere koden.

### Installere `DHT sensor library` biblioteket helplink

Før vi begynner må vi laste ned og installere et Arduino bibliotek. Tidligere har vi brukt kommandoer som `delay`, `pinMode` og `digitalWrite` som alle er del av standard-biblioteket for Arduino. Vi trenger nye kommandoer for å lese av temperatur fra DHT-sensoren. Disse vil vi finne i et bibliotek som heter `DHT sensor library` som er laget av selskapet Adafruit. *Adafruit er en av de største produsentene for Arduino-utstyr.*

`Arduino IDE` har en innebygd meny for å laste ned Arduino bibliotek. Åpne en `Arduino IDE` Klikk på `Sketch`&rarr;`Include library`&rarr;`Manage libraries...` for å åpne `Library Manager`.

![Arduino IDE Manage libraries][manage-libraries-menu]

I `Library Manager` som åpner seg, må du nå søke for `DHT sensor library` som er biblioteket for sensoren DHT i air:bit. Skriv inn `DHT sensor library` i søkefeltet. Klikk på resultatet som kommer opp, velg den nyeste versjonen i Drop-Down-menyen og klikk på `Install`.

![Arduino IDE Library Manager: DHT sensor library][library-manager-dht-sensor-library]

### Installere `Adafruit_Sensor` biblioteket helplink

`DHT sensor library`-biblioteket bruker Adafruit sitt overordnete `Adafruit_Sensor` bibliotek. Dette inneholder informasjon som er til felles for alle Adafruit sine sensorer.

For å installere dette biblioteket skal vi laste ned nyeste version selv ved å klikke på denne linken: [**Adafruit_Sensor**][adafruit-sensor-latest]  
Klikk på linken **Source code** (zip) for å laste ned biblioteket.  
Det er viktig at du lagrer (**ikke åpner**) filen. Husk hvor du lagrer filen, vi må finne frem den filen i neste steg.

![Download Adafruit_Sensor library][adafruit_sensor-download]

Etter du har lastet ned biblioteket og lagret filen må vi installere biblioteket i `Arduino IDE`. Klikk i menyen på `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` (punktet under `Manage libraries`). Velg filen du nettopp lastet ned. Om nettleseren din ikke ba deg om å velge hvor filen skulle lagres, vil du mest sannsynligvis finne den under `Downloads` (`Nedlastinger`).

## Kodeblokker helplink

I dette programmet skal vi for hver gjennomgang gjennom `loop`-funksjonen lese ut måleverdiene fra sensoren, så skrive dem ut over seriell-forbindelsen. Til slutt skal vi vente en stund til vi tar neste måling.

For å starte med trenger vi to lokale variabler for å lagre måleverdiene (en for temperatur og en for fuktighet).

Temperatur og Fuktighet er verdier som representeres som kommatall. F.eks. et vanlig digitalt termometer vil vise temperatur som f.eks. `20.7°C`. Fuktighet måles som relativ fuktighet i %, f.eks. `41.2% RH`. Variabler for å lagre kommatall må bruke datatypen `float`. *`float` er en forkortelse for `floating-point decimal number` og det er slik en datamaskin vanligvis håndterer kommatall.*

Gå inn i sidefanen _Variables_ og hent ut to `Declare Variable` blokker. Gi begge typen `float` og gi dem navnene `temperature` og `humidity`. *Merk at variablene kan navngis hvordan som helst, men at vi gir dem disse navnene for å lettere huske hva de inneholder.*

Hvis vi så går inn i sidefanen _Air:Bit; DHT_ kan vi se at det finnes to blokker, `Get DHT22 Temperature` og `Get DHT22 Humidity`. Koble `Get DHT22 Temperature` til `Declare Variable` blokken som heter _temperature_ og koble den andre blokken til _humidity_ deklarasjonsblokken.
![][skjermbilde-get-DHT-blockly]
I koden over ser du at variabler også kan initialiseres samtidig med deklarasjonen. Merk at kommatall bruker `.` (punktum) som komma-tegn.

Etter disse blokkene er brukt vil den aktuelle verdien for temperatur og fuktighet være lagret i variablene, så vi kan skrive dem ut.

Inne i _Input/Output_ sidefanen finner vi blokker som heter `Serial Print` og `Serial Println`. Den første skriver ut inneholdet som er inne i sideblokken ut til serialporten, mens den andre i tillegg starter en ny linje. Ved å slette sideblokken kan du sette inn en annen blokk. *Merk at ikke alle blokker kan skrives.*

Sett in en `Serial Print` blokk. Slett så sideblokken og åpne `Variables` sidefanen og velg `temperature` blokken. Koble den blokken til `Serial Print` blokken. Videre gå gjennom og print `C` med en `Serial Println` for å indikere at verdien er i Celsius. Gjør lignende for `humidity`, der legger vi til en `%` for å indikere at verdien er i prosent.

![][skjermbilde-seriell-DHT-blockly]

Helt til slutt vil vi vente en liten stund til det leses av en ny måling. Bruk `Delay`-blokken for dette.

## Ferdig

Siden variabler kan navngis som du ønsker kan koden din se litt anderledes ut, men hovedsakelig burde ting se ut som vist under.

![][skjermbilde-DHT-blockly]

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg: **LED**][led]  
&rarr; [Gå til neste steg: **Støvsensoren**][pm]  

[pinout]: airbit-Pinout
[counting]: Variabler-og-telling-i-Arduino

[home]: airbit-Programmering
[led]: airbit-LED-Blinking
[pm]: Programmering-med-Støvsensoren

[adafruit-sensor-latest]: https://github.com/adafruit/Adafruit_Sensor/releases/latest

[manage-libraries-menu]: Arduino-IDE-Manage-Library.png
[library-manager-simple-dht]: Arduino-IDE-Library-Manager-SimpleDHT.png
[library-manager-dht-sensor-library]: Arduino-IDE-Library-Manager-DHTSensorLibrary.png
[adafruit_sensor-download]: GitHub-Adafruit_Sensor-download.png

[skjermbilde-DHT-blockly]: skjermbilde-DHT-blockly.png
[skjermbilde-get-DHT-blockly]: skjermbilde-get-DHT-blockly.png
[skjermbilde-seriell-DHT-blockly]: skjermbilde-seriell-DHT-blockly.png

