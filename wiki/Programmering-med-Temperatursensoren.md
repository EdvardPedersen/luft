Temperatursensoren vi bruker i air:bit, DHT, er en veldig vanlig type sensor. Heldigvis betyr dette for oss at noen andre allerede har gjort jobben med å finne ut hvordan man leser av data fra sensoren. Dette gjør det veldig mye enklere å lese av temperaturdata.

Vi kommer til å skrive en Arduino Sketch, der vi skriver ut temperatur- og fuktighetsmålinger over en seriell forbindelse.

## Komme i gang

Igjen vil vi starte med en helt tom Sketch. Du kan klikke på `File`&rarr;`New` menyen for å starte en ny Sketch.

``` cpp
void setup() {
  
}

void loop() {
  
}
```

# helplink

### Installere `DHT sensor library` biblioteket

Før vi begynner må vi laste ned og installere et Arduino bibliotek. Tidligere har vi brukt kommandoer som `delay`, `pinMode` og `digitalWrite` som alle er del av standard-biblioteket for Arduino. Vi trenger nye kommandoer for å lese av temperatur fra DHT-sensoren. Disse vil vi finne i et bibliotek som heter `DHT sensor library` som er laget av selskapet Adafruit. *Adafruit er en av de største produsentene for Arduino-utstyr.*

`Arduino IDE` har en innebygd meny for å laste ned Arduino-biblioteker. Klikk på `Sketch`&rarr;`Include library`&rarr;`Manage libraries...` for å åpne `Library Manager`.

![Arduino IDE Manage libraries][manage-libraries-menu]

I `Library Manager` som åpner seg, må du nå søke på `DHT sensor library` som er biblioteket for sensoren DHT i air:bit-oppsettet. Skriv inn `DHT sensor library` i søkefeltet. Klikk på resultatet som kommer opp, velg den nyeste versjonen i nedtrekksmenyen og klikk på `Install` (eller `Update` hvis det allerede er installert).

![Arduino IDE Library Manager: DHT sensor library][library-manager-dht-sensor-library]

# helplink

### Installere `Adafruit_Sensor` biblioteket

`DHT sensor library`-biblioteket bruker Adafruit sitt overordnede `Adafruit_Sensor` bibliotek. Dette inneholder informasjon som er til felles for alle Adafruit sine sensorer.

For å installere dette biblioteket skal vi laste ned den nyeste versjonen selv, ved å klikke på denne linken: [**Adafruit_Sensor**][adafruit-sensor-latest]  
Klikk på linken **Source code** (zip) for å laste ned biblioteket.  
Det er viktig at du lagrer (**ikke åpner**) filen. Husk hvor du lagrer filen, vi må finne frem filen i neste steg.

![Download Adafruit_Sensor library][adafruit_sensor-download]

Etter du har lastet ned biblioteket og lagret filen må vi installere biblioteket i `Arduino IDE`. Klikk i menyen på `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` (punktet under `Manage libraries`). Velg filen du nettopp lastet ned. Om nettleseren din ikke ba deg om å velge hvor filen skulle lagres, vil du mest sannsynligvis finne den under `Downloads` (`Nedlastinger`).

# helplink

## Globale definisjoner

Først må `DHT`-biblioteket inkluderes i Arduino-Sketchen. I `C++` bruker vi `#include` direktivet for dette:

``` cpp
#include <DHT.h>
```

Ta en titt på [Pinout Skjemaet][pinout]. Definér en konstant for pinnen til Temperatursensoren:

``` cpp
#define DHTPIN 9
```

Tidligere i [eksempelet for telling][counting] deklarerte du en variabel med datatype `int`. Nå må du deklarere en variabel for å kontrollere forbindelsen med DHT sensoren. Datatypen for dette kommer fra det inkluderte `DHT` biblioteket og heter `DHT`. For å deklarere denne variabelen trenger du å vite to ting: hvilken pinne den er koblet til på, og hvilken slags type DHT du bruker.

Pinnen har du allerede definert i `DHTPIN` konstanten i linjen over, og i vårt tilfelle bruker vi DHT-sensor-typen `DHT22`.

``` cpp
DHT dhtSensor(DHTPIN, DHT22);
```

*I koden over kalles variabelen `dhtSensor`, men du kan bruke hvilket som helst navn som du har lyst å bruke. Det anbefales å bruke et deskriptivt navn som virker intuitivt å bruke. Merk også at vi bruker konstanten `DHT22`. Fra beskrivelsen som vistes når du installerte DHT-biblioteket kan du se at det finnes flere varianter av DHT-sensorer. air:bit sin sensor er av den nyere `DHT22` varianten.*

# helplink

## `setup`

I dette eksemplet for programmering med DHT sensoren skal vi bruke en seriell forbindelse med PC-en, så vi må initialisere den serielle forbindelsen, slik vi har gjort tidligere.

``` cpp
  Serial.begin(9600);
```
Merk:
``` cpp
  dhtSensor.begin();
```
Kreves også i `setup` for nyere versjoner av bibliotekene.

# helplink

## `loop`

For hver gjennomgang gjennom `loop`-funksjonen skal vi lese ut måleverdiene fra sensoren, så skrive dem ut over seriell-forbindelsen.

Til å starte med trenger vi to lokale variabler for å lagre måleverdiene (en for temperatur og en for fuktighet). Lokale variabler deklareres akkurat som globale variabler (som f.eks. `dhtSensor`), men de varer bare for én gjennomgang gjennom `loop`-funksjonen, dvs. variabelen lages på nytt for hver gjennomgang. Variabler for å lagre en måling fra sensorer lages best som lokale variabler i stedet for globale variabler.

Temperatur og Fuktighet er verdier som representeres som desimaltall. F.eks. et vanlig digitalt termometer vil vise temperatur som f.eks. `20.7°C`. Fuktighet måles som relativ fuktighet i `%`, f.eks. `41.2% RH`. Variabler for å lagre desimaltall må bruke datatypen `float`. *`float` er en forkortelse for `floating-point decimal number` og det er slik en datamaskin vanligvis håndterer desimaltall.*

``` cpp
  float temperature = 0.0;
  float humidity = 0.0;
```

I koden over ser du at variabler også kan initialiseres samtidig med deklarasjonen. Merk at desimaltall bruker `.` (punktum) som desimaltegn. *Igjen, merk at variablene kan navngis hvordan som helst.*

Nå kan vi bruke `readTemperature` og `readHumidity` kommandoene som hører til `dhtSensor` variabelen vår. Resultatene vil bli gitt henholdsvis i grader Celsius (`°C`) og `%` relativ fuktighet.

``` cpp
  temperature = dhtSensor.readTemperature();
  humidity = dhtSensor.readHumidity();
```

Etter disse kommandoene vil den aktuelle verdien for temperatur og fuktighet være lagret i variablene, så vi kan skrive dem ut.

``` cpp
  Serial.print(temperature);
  Serial.print("°C");
  Serial.print("\t");
  Serial.print(humidity);
  Serial.print("%");
  Serial.println("");
```

I den tredje linjen over ser du at `\t` skrives ut. I `C++` brukes `\` (backslash) for spesielle tegn. `\t` er et spesiellt tegn kalt tabulator. Det brukes for å skille kolonner i en tabell fra hverandre. I et vanlig tekst-program, som f.eks. Word, vil du skrive et tabulator-symbol ved å trykke Tab-tasten til venstre for `Q` på det norske tastaturet.

Helt til slutt vil vi vente en liten stund til det leses av en ny måling. Bruk `delay`-kommandoen for dette.

``` cpp
  delay(1500);
```

## Ferdig

Siden variabler kan navngis som du ønsker kan koden din se litt annerledes ut, men hovedsakelig burde ting se ut som vist under.

``` cpp
#include <DHT.h>

#define DHTPIN 9
DHT dht22(DHTPIN, DHT22);

void setup() {
  Serial.begin(9600);
}

void loop() {
  // Declare variables for sensor readings
  float temperature = 0;
  float humidity = 0;
  // Take readings from sensor
  temperature = dht22.readTemperature();
  humidity = dht22.readHumidity();

  // Print out readings
  Serial.print(temperature);
  Serial.print("°C");
  Serial.print("\t");
  Serial.print(humidity);
  Serial.print("%");
  Serial.println("");

  // Wait 2.5 seconds until next reading.
  delay(2500);
}
```

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
[library-manager-dht-sensor-library]: Arduino-IDE-Library-Manager-DHTSensorLibrary.png
[adafruit_sensor-download]: GitHub-Adafruit_Sensor-download.png
