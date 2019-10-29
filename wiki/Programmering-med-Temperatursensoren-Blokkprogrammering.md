Temperatursensoren vi bruker i air:bit, DHT, er en veldig vanlig type sensor. Heldigvis betyr dette for oss at noen andre allerede har gjort jobben med å finne ut hvordan man leser av data fra sensoren. Dette gjør det veldig mye enklere å lese av temperaturdata.

Vi kommer til å lage et Blokkprogram hvor vi skriver ut temperatur- og fuktighetsmålinger over en seriell forbindelse.

## Komme i gang

Igjen vil vi starte med en helt fersk tom Sketch. Du kan klikke på `Discard` knappen for å slette den tidligere koden.

### Installere `DHT sensor library` biblioteket helplink

Før vi begynner må vi laste ned og installere et Arduino bibliotek. Tidligere har vi brukt kommandoer som `delay`, `pinMode` og `digitalWrite` som alle er del av standard-biblioteket for Arduino. Vi trenger nye kommandoer for å lese av temperatur fra DHT-sensoren. Disse vil vi finne i et bibliotek som heter `DHT sensor library` som er laget av selskapet Adafruit. *Adafruit er en av de største produsentene for Arduino-utstyr.*

`Arduino IDE` har en innebygd meny for å laste ned Arduino-biblioteker. Åpne en `Arduino IDE`. Klikk på `Sketch`&rarr;`Include library`&rarr;`Manage libraries...` for å åpne `Library Manager`.

![Arduino IDE Manage libraries][manage-libraries-menu]

I `Library Manager` som åpner seg, må du nå søke på `DHT sensor library` som er biblioteket for sensoren DHT i air:bit-oppsettet. Skriv inn `DHT sensor library` i søkefeltet. Klikk på resultatet som kommer opp, velg den nyeste versjonen i nedtrekksmenyen og klikk på `Install` (eller `Update` hvis det allerede er installert).

![Arduino IDE Library Manager: DHT sensor library][library-manager-dht-sensor-library]

### Installere `Adafruit_Sensor` biblioteket helplink

`DHT sensor library`-biblioteket bruker Adafruit sitt overordnede `Adafruit_Sensor` bibliotek. Dette inneholder informasjon som er til felles for alle Adafruit sine sensorer.

For å installere dette biblioteket skal vi laste ned den nyeste versjonen selv, ved å klikke på denne linken: [**Adafruit_Sensor**][adafruit-sensor-latest]  
Klikk på linken **Source code** (zip) for å laste ned biblioteket.  
Det er viktig at du lagrer (**ikke åpner**) filen. Husk hvor du lagrer filen, vi må finne frem filen i neste steg.

![Download Adafruit_Sensor library][adafruit_sensor-download]

Etter du har lastet ned biblioteket og lagret filen må vi installere biblioteket i `Arduino IDE`. Klikk i menyen på `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` (punktet under `Manage libraries`). Velg filen du nettopp lastet ned. Om nettleseren din ikke ba deg om å velge hvor filen skulle lagres, vil du mest sannsynligvis finne den under `Downloads` (`Nedlastinger`).

## Kodeblokker helplink

For hver gjennomgang gjennom `loop`-funksjonen skal vi lese ut måleverdiene fra sensoren, så skrive dem ut over seriell-forbindelsen.

Til å starte med trenger vi to lokale variabler for å lagre måleverdiene (en for temperatur og en for fuktighet). 

Temperatur og Fuktighet er verdier som representeres som desimaltall. F.eks. et vanlig digitalt termometer vil vise temperatur som f.eks. `20.7°C`. Fuktighet måles som relativ fuktighet i `%`, f.eks. `41.2% RH`. Variabler for å lagre desimaltall må bruke datatypen `float`. *`float` er en forkortelse for `floating-point decimal number` og det er slik en datamaskin vanligvis håndterer desimaltall.*

Gå inn i sidefanen _Variables_ og hent ut to `Declare Variable` blokker. Gi begge typen `float` og gi dem navnene `temperature` og `humidity`. *Merk at variablene kan navngis hvordan som helst.*

I sidefanen _Air:Bit; DHT_ finner vi blokkene: `Get DHT22 Temperature` og `Get DHT22 Humidity`. Koble `Get DHT22 Temperature` til `Declare Variable` blokken som med _temperature_ variabelen og koble den andre blokken til _humidity_ deklarasjonsblokken.

![][skjermbilde-get-DHT-blockly]

I kodeblokkene over ser du at variabler også kan initialiseres samtidig med deklarasjonen. Merk at desimaltall bruker `.` (punktum) som desimaltegn.

Etter at disse blokkene er brukt vil de aktuelle verdiene for temperatur og fuktighet være lagret i variablene, så vi kan skrive dem ut.

Inne i _Input/Output_ sidefanen finner vi blokker som heter `Serial Print` og `Serial Println`. Den første skriver ut inneholdet som er inne i sideblokken ut til serialporte. Den andre gjør det samme, men starter en ny linje i tillegg. Ved å slette en sideblokken kan du sette inn en annen blokk for å skrive ut inneholdet i den. *Merk at ikke alle blokker kan skrives.*

Sett in en `Serial Print` blokk. Slett så sideblokken og åpne `Variables` sidefanen og velg `temperature` blokken. Koble den blokken til `Serial Print` blokken. Etter det kan du skrive ut en `C` med en `Serial Println` for å indikere at temperaturverdien er gitt i Celsius.

Vi gjør det samme for `humidity`, men legger til et `%`-tegn for å indikere at verdien er i prosent.

![][skjermbilde-seriell-DHT-blockly]

Helt til slutt vil vi vente en liten stund til det leses av en ny måling. Bruk `Delay`-blokken for dette.

## Ferdig

Siden variabler kan navngis som du ønsker kan kodeblokkene dine se litt annerledes ut, men hovedsakelig burde ting se ut som vist under.

![][skjermbilde-DHT-blockly-finished]

Og Kodelinjene under _Arduino_ fanen burde nå se noe sånt som dette ut:

```cpp
#include <DHT.h>

float temperature;

float humidity;

#define DHTPIN 9

DHT dht22(DHTPIN, DHT22);
void setup()
{
  Serial.begin(9600);

}


void loop()
{
  temperature = dht22.readTemperature();
  humidity = dht22.readHumidity();
  Serial.print(temperature);
  Serial.println("C");
  Serial.print(humidity);
  Serial.println("%");
  delay(1000);

}
```

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg i Blokkprogrammeringen: **LED**][led]  
&rarr; [Gå til neste steg i Blokkprogrammeringen: **Støvsensoren**][pm]  

[home]: airbit-Programmering
[led]: airbit-LED-Blinking-Blokkprogrammering
[pm]: Programmering-med-Støvsensoren-Blokkprogrammering

[adafruit-sensor-latest]: https://github.com/adafruit/Adafruit_Sensor/releases/latest

[manage-libraries-menu]: Arduino-IDE-Manage-Library.png
[library-manager-simple-dht]: Arduino-IDE-Library-Manager-SimpleDHT.png
[library-manager-dht-sensor-library]: Arduino-IDE-Library-Manager-DHTSensorLibrary.png
[adafruit_sensor-download]: GitHub-Adafruit_Sensor-download.png

[skjermbilde-DHT-blockly-finished]: skjermbilde-DHT-blockly-finished.png
[skjermbilde-get-DHT-blockly]: skjermbilde-get-DHT-blockly.png
[skjermbilde-seriell-DHT-blockly]: skjermbilde-seriell-DHT-blockly.png

