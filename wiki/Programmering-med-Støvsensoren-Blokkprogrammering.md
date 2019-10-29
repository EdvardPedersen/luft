Støvsensoren til air:bit er litt mer komplisert enn temperatursensoren, men igjen finnes det et Arduino-bibliotek for å lese av målinger fra sensoren.

Som i temperatursensor-eksemplet kommer vi til å demonstrere hvordan vi tar målinger fra støvsensoren, og skrive dem ut til seriell-forbindelsen til PC-en.

## Ny Sketch

Igjen vil vi starte med en helt fersk tom Sketch. Du kan klikke på `Discard` knappen for å slette den tidligere koden.

## Laste ned bibliotek helplink

Akkurat som DHT-biblioteket vi brukte for temperatursensoren, finnes det et bibliotek for støvsensoren. Desverre oppstår det komplikasjoner med de andre sensorene i air:bit når vi bruker denne. Derfor har air:bit teamet laget sin egen versjon for dette biblioteket. Last ned den nyeste, dvs. den øverste, versjonen i listen du finner når du klikker på denne linken: (klikk der det står `airbit-teacher-workshop `) **[SDS011 biblioteket](https://github.com/skolelab/SDS011/releases)**  
Det er viktig at du lagrer (**ikke åpner**) filen. Husk hvor du lagrer filen, vi må finne frem filen i neste steg.

Etter du har lastet ned biblioteket og lagret filen må vi installere biblioteket i `Arduino IDE`. Åpne en `Arduino IDE`. Klikk i menyen på `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` (punktet under `Manage libraries`). Velg filen du nettopp lastet ned. Om nettleseren din ikke ba deg om å velge hvor filen skulle lagres, vil du mest sannsynligvis finne den under `Downloads` (`Nedlastinger`).

## tmp
Konsulter så [pinout skjemaet][pinout]. Som vanlig definerer vi konstanter for pinnene som brukes for kommunikasjon med Arduinoen.

``` cpp
#define PM_TX 2
#define PM_RX 3
```


## `loop` helplink

Som med temperatursensoren, skal vi lese ut målingene fra støvsensoren og skrive dem ut. Støvsensoren tar målinger av to forskjellige partikelstørrelser: 2.5µm og 10µm. Begge tall gir konsentrasjoner som desimaltall. Enheten for verdiene er i `µg/m³` (mikrogram per kubikkmeter)

Trekk ut deklarasjonsblokker for variablene `PM10` og `PM25`, som skal inneholde verdiene til de to målingsverdiene.

Støvmålingsblokken, som vi finner unner _Air:Bit; SDS_ sidefanen er litt spesiellt. Trekk en slik blokk ut i hovedområdet. Den tar inn to pinneverdier og to tekstverdier. I tillegg gir den tilbake en heltallsverdi.

De to pinneverdiene skal samsvare med `SDS` sensorens pinner som vi finner i [pinout skjemaet][pinout]. `PIN_RX` skal være `3` og `PIN_TX` skal være `2`.

De to tekst verdiene skal være variabelnavnene til variablene som skal inneholde sensormålingene. Trekk ut 2 deklarasjonsblokker for variabler. Kall den ene `PM10` og den andre `PM25`. Skriv navnene til variablene inn i `SDS` blokken. 

`SDS`-blokken kan feile, så den returnerer en feilverdi som vi lagrer i en variabel. Trekk ut enda en deklarasjonsblokk kalt `error` og sett verdien til å være ulik `0`.

![][skjermbilde-variables-declare-SDS-blockly]

I den tekniske spesifikasjonen til Støvsensoren står det at den kun kan leses av én gang hvert sekund. `SDS`-blokken vil derfor feile om vi spør etter målinger oftere enn det. Dersom `SDS`-blokken feilet vil den returnene en verdi ulik `0`. Den beste måten å håndtere denne situasjonen på er å prøve på nytt helt til vi har fått inn ny data fra støvsensoren.

For å sikre at vi faktisk har en gyldig måleverdi fra sensoren, må vi altså prøve å kjøre `SDS`-blokken om og om igjen helt til feilverdien er lik `0` (dvs. ingen feil). Når vi vil be Arduinoen om å kjøre den samme koden flere ganger bruker vi i programmering en løkke (*loop* på engelsk).

I `C++`, kodespråket til Arduino, finnes det tre typer løkker. I dette tilfellet skal vi benytte oss av *while-do*-løkken. Denne løkken kjører en kommando (eller flere) og repeterer dersom en betinglse er sann. Dette ser slikt ut i blokkprogrammering:

![][skjermbilde-while-do-blockly]

I kodebiten over ser du at vi sjekker om `error` er ulik `0`. Med én gang `error` er `0`, er vi sikre på at vi har fått gyldige målinger fra Støvsensoren og kan fortsette med å skrive dem ut. Så la oss fylle inn `read`-kommandoen innenfor `while`-`do`-blokken vår.

![][skjermbilde-variables-set-SDS-blockly]

**Merk** at vi deklarer `error` utenfor løkken! [Bruk av variabler utenfor scope][debugging-scopes] er en vanlig feil som lett er gjort når du skriver kode. Gå til [Feilsøking av programmeringsfeil][debugging-scopes] for å vite mer om hvorfor vi flyttet deklarasjonen av `error` utenfor løkken.

Nå som verdiene er hentet må vi skrive dem ut over seriellforbindelsen. Siden Støvsensoren kun kan avleses én gang per sekund er det lurt å bruke en `Delay`-blokk til slutt. Få Arduinoen til å vente i minst ett sekund.

![][skjermbilde-seriellprint-SDS-blockly]

## Ferdig

Kodeblokkene skal nå se slik ut:

![][skjermbilde-SDS-blockly]

Koden skal nå se slik ut:

``` cpp
#include <SDS011.h>

float PM25;

float PM10;

int error;

#define SDS_RX 3
#define SDS_TX 2

SDS011 sds;
void setup()
{
  sds.begin(PM_TX, PM_RX);

  Serial.begin(9600);

}


void loop()
{
  PM25;
  PM10;
  error = 1;
  while (error != 0) {
    error = sds.read(&PM25, &PM10);
  }
  Serial.println(PM25);
  Serial.println(PM10);
  delay(1000);

}
```

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg i Blokkprogrammeringen: **Temperatursensoren**][dht]  
&rarr; [Gå til neste steg i Blokkprogrammeringen: **GPS-antenna**][gps]  

[home]: airbit-Programmering
[dht]: Programmering-med-Temperatursensoren-Blokkprogrammering
[gps]: Programmering-med-GPS-antenna-Blokkprogrammering

[pinout]: airbit-Pinout
[debugging-scopes]: Feilsøking-av-programmeringsfeil#bruk-av-variabler-utenfor-scope


[skjermbilde-SDS-blockly]: skjermbilde-SDS-blockly.png
[skjermbilde-seriellprint-SDS-blockly]: skjermbilde-seriellprint-SDS-blockly.png
[skjermbilde-variables-declare-SDS-blockly]: skjermbilde-variables1-SDS-blockly.png
[skjermbilde-variables-set-SDS-blockly]: skjermbilde-variables2-SDS-blockly.png

