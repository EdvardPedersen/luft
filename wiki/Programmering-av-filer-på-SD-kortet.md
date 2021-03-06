Når du er ute og tar målinger med air:bit vil du ikke hele tiden ha den tilkoblet til din datamaskin. air:bit har ikke noe som forbinder seg med internett, så du vil trenge en måte å lagre måledata på. air:bit har en kortleser for microSD-kort. Et microSD-kort er i prinsippet bare en minnebrikke, veldig lik en vanlig USB-minnepenn og faktisk nøyaktig lik minnekortet som finnes for å lagre data på nyere mobiltelefoner, digitalkamera, osv.

microSD-kortet som følger med air:bit har ganske stor lagringskapasitet. Du vil kunne lagre flere år med målingsdata på dette, så i prinsippet skal du ikke bekymre deg for at du ikke har nok plass på "harddisken" til air:bit.

Du vil legge merke til at microSD-kortet kommer med en adapter. Det lille microSD-kortet passer inn i den større adapteren og former slik et vanlig SD-kort. Laptopter nå til dags har som regel porter hvor du kan plugge inn SD-kort.

Når du plugger inn SD-kortet inn i en PC, vil det dukke opp som en harddisk i Filutforskeren (`Windows Explorer`, eller `Min datamaskin`, eller `This PC`) akkurat som en vanlig USB-minnebrikke også ville gjort.

- Filnavn kan ikke være lengre en 12 bokstaver.
- Filnavn må avslutte med en filtype forkortelse (f.eks. `.txt`, `.csv`).
	- Lengden på endelsen telles som en del av de 12 bokstavene til navnet.
- Filnavn kan kun inneholde engelske bokstaver (ingen `æ`, `ø`, `å`).

## Beskrivelse av eksemplet

I dette eksemplet skal du først lage en ny fil på SD-kortet. Deretter skal du skrive tekst inn i den filen. For hver gjennomgang gjennom `loop` skal du øke en teller og skrive ned verdien av telleren.

Dette vil ligne veldig på [kodeeksemplet om variabler og telling i Arduino][counting-example]. Du vil i prinsippet gjøre nøyaktig det samme i dette eksemplet, borsett fra at du skriver til en fil på SD-kortet, i stedet for seriell-koblingen til datamaskinen.

# helplink

## Ny Sketch

Start med en helt ny Sketch. Menypunkt: `File`&rarr;`New`.

``` cpp
void setup () {

}

void loop() {

}
```

# helplink

## Laste ned og installere bibliotek

`SD`-biblioteket er allerede forhåndsinstallert sammen med `Arduino IDE`. Det vil ikke være nødvendig å laste ned noe for dette eksemplet.

# helplink

## Globale definisjoner

Først må du inkludere `SD`-biblioteket. Bruk navnet `<SD.h>` for `#include` direktivet.

Fra [pinout skjemaet][pinout] ser du at det er flere pinner som kobler SD-kortleseren til Arduinoen. Vi trenger en definisjon for `CS` pinnen i koden vår.

``` cpp
#define SD_CS_PIN 10
```

Lag så en global variabel får å representere skrivingen til filen på SD-kortet. Vi bruker datatypen `File` for dette. Merk at å lage en variabel vil **ikke** automatisk lage en fil på SD-kortet. Koden for å lage og skrive til filen kommer senere.

``` cpp
File file;
```

*I koden over navngis tilkoblingen til filen `file`. Du kan bruke hvilket som helst navn for variabelen du vil. Dette vil ikke være navnen på filen du lagrer på SD-kortet!*

Til slutt må vi lage en global teller-variabel. Dette vil være et heltall, så bruk datatypen `int`.

# helplink

## `setup`

Først må du aktivere kontroll over `CS`-pinnen. Dette funker akkurat som du gjør for å aktivere pinnene til LED-lysene. Bruk `pinMode` med pinnen som første og `OUTPUT` som andre argument.

``` cpp
  pinMode(SD_CS_PIN, OUTPUT);
```

Vanligvis ville du initialisere seriell-kommunikasjonen til datamaskinen nå ved å bruke `Serial.begin` kommandoen, men la oss heller starte opp SD-kortet. Bruk `SD.begin` kommandoen og spesifiser `CS`-pinnen kortleseren er koblet til.

``` cpp
  SD.begin(SD_CS_PIN);
```

Nå må du bestemme deg for et navn på filen. Filnavnet er det du vil se i Filutforskeren (`Min datamaskin`) når du viser innholdet på SD-kortet etter du har plugget det inn i datamaskinen. Et filnavn er flere bokstaver etter hverandre. I `C++` bruker vi datatypen `char` for én bokstav. Vi bruker firkantparanteser `[` og `]` for å lage en variabel som lagrer *flere* verdier av samme type etter hverandre. Akkurat det vi trenger, det ser slik ut:

``` cpp
  char filename[] = "testfile.txt";
```

I koden over ser du deklarasjonen av variabelen `filename`. Firkantparantesene skrives rett **etter** navnet. Verdien er bokstavene `testfile.txt`, og som vanlig står tekst mellom to anførelsestegn `"`.

I koden over er filnavnet `testfile.txt` valgt, men du kan bruke hvilket navn du vil (igjen unngår vi norske bokstaver, og holder navnet under 8 bokstaver). Å la filnavnet slutte med `.txt` er veldig vanlig, og vil automatisk fortelle til datamaskinen din at du mener å lagre vanlig tekst i filen.

Lage så en ny fil på SD-kortet. Dette gjøres med kommandoen `SD.open`. Kommandoen tar først imot filnavnet, og så en *modus*. Filer kan åpnes for lesing, skriving, eller begge deler. Koden ser slik ut:

``` cpp
  file = SD.open(filename, O_CREAT | O_WRITE);
```

`SD.open` kommandoen vil gi oss tilkoblingen til filen som resultat, derfor lagrer vi denne i vår globale variabel for filtilkoblingen (kalt `file`). `O_CREAT | O_WRITE` betyr: Lag ny fil (`O_CREAT`) og åpne filen for skriving (`O_WRITE`). *Du finner `|` (pipe-symbolet) til venstre for `1`-tasten i den øverste raden på et vanlig norsk tastatur.*

Skriv så en velkomsttekst som den første linjen i filen:

``` cpp
  file.println("Dette er starten av filen.");
```

Merk at vi bruker `file.println` der vi ville brukt `Serial.println` før. `file` viser her til navnet på fil-variabelen du bruker. Den vil være annerledes dersom du har gitt variabelen ett annet navn.

Det holder ikke å bare bruke `print`, eller `println` kommandoene for å skrive noe til filen. Hver gang du er ferdig med å skrive ned en viktig del informasjon bør du bruke `flush`- kommandoen for å sikre at alt blir lagret på SD-kortet. Siden vi nå er kommet til slutten av `setup`-blokken er det en god idé og gjøre dette nå, før vi fortsetter med `loop`.

``` cpp
  file.flush();
```

Helt til slutt må du også huske å initialisere teller-variabelen til `0`.

Nå burde koden din se omtrent slik ut:

``` cpp
#include <SD.h>

#define SD_CS_PIN 10

File file;
int counter;

void setup () {
  // Activate CS-Pin control
  pinMode(SD_CS_PIN, OUTPUT);

  // Startup SD-card reader
  SD.begin(SD_CS_PIN);

  // Define filename
  char filename[] = "testfile.txt";

  // Create new file and open for writing
  file = SD.open(filename, O_CREAT | O_WRITE);

  file.println("Dette er den første linjen i filen.");
  file.flush(); // Force saving data to SD-card

  // Start counter at 0
  counter = 0;
}

void loop() {

}
```

# helplink

## `loop`

For hver gjennomgang gjennom `loop` vil vi skrive en ny linje med tekst. Teksten vi skriver skal inneholde verdien til telleren vår, som vi må øke hver gang vi kjører `loop`.

Vi kan starte med å øke telleren med én. Bruk `+=` instruksjonen til dette.

Så skriver vi litt tekst, som f.eks `Dette er linje nr.:`. Så skriver vi tekst med bare et mellomrom, for å skille `:` fra verdien til telleren. Så kan vi skrive selve teller-verdien. Bruk `file.print`-kommandoen flere ganger for å skrive alle delene av teksten og avslutt linjen ved å bruke `file.println`-kommandoen.

Når vi er ferdige, må vi forsikre oss at teksten blir lagret på SD-kortet. Bruk `file.flush`-kommandoen helt til slutt.

``` cpp
void loop() {
  counter += 1;

  file.print("Dette er linje nr.:");
  file.print(" ");
  file.print(counter);
  file.println("");

  file.flush();
}
```

Det kan også være en god idé å legge til en `delay`-kommando etter `flush`-kommandoen på slutten, slik at du kun skriver en ny linje etter f.eks. ett sekund med venting.

# helplink

## Test 1

1. Last opp koden du har skrevet så langt til air:bit.
1. Trekk så ut ledningen fra datamaskinen.
1. Sett inn microSD-kortet i kortleseren til air:bit gjennom sprekken på siden der det står `SD-kort`. Trykk inn kortet til det sier "klikk".
1. Plugg USB-ledningen i batteriet til air:bit.
1. Vent i ca. 10 sekunder.
1. Trekk så ut ledningen fra batteriet igjen.
1. Trykk igjen på SD-kortet i air:bit, kortet burde sprette ut.
1. Skyv microSD-kortet i adapteren som følgte med til SD-kortet og plugg det i en SD-kortleser på din datamaskin.
1. PC-en din burde automatisk finne SD-kortet ditt når du plugger det inn. 
   (Om den ikke åpnes automatisk, velg `Min datamaskin` (`This PC`) i startmenyen.)
1. SD-kortet vises som en egen ny harddisk. Dobbel-klikk for å vise filene på SD-kortet.
1. Du burde kun se én fil her. Filen burde ha det navnet du brukte i koden din, f.eks. `testfile.txt`. Det kan hende at du ikke ser filendelsen (`.txt`) siden Windows på noen datamaskiner skjuler filendelser.  
   Om du høyreklikker på filen, og velger `Egenskaper` (`Properties`) vil du få opp et vindu med hele filnavnet (inklusive `.txt`).
1. Dobbelklikk på filen for å åpne den og vise innholdet.  
   ![Filinnholdet av testfile.txt i Notepad][notepad]

# helplink

## Ikke overskriv filen

Om du tester ut koden vi har skrevet så langt flere ganger, vil du legge merke til at air:bit skriver over filen hver gang du kobler den til strøm. Dvs. all data som lå i filen blir slettet hver gang du skrur på air:bit måleren. Dette virker ikke helt fornuftig. Det hadde kanskje vært bedre om vi bare la til ny data i en eksisterende fil dersom det allerede ligger noe der fra før.

Om du ser nøyere på `setup`-blokken, ser du problemet. Vi bruker `O_CREAT` modusen for å lage en **ny** fil, der vi bruker `SD.open`-kommandoen. Og dette gjør vi hver gang air:bit kjører `setup`, dvs. hver gang du plugger i strømmen. Det vi trenger her er en betingelse som sjekker om filen allerede finnes. Vi vil ikke bruke `O_CREAT` når det allerede finnes en fil på SD-kortet som har samme navn som vi vil bruke.

Vi kan bruke `SD.exists`-kommandoen for å spørre om en fil med et gitt navn eksisterer på SD-kortet.

La oss lage en `if`-`else`-kombinasjon og flytte linja vi har nå inn i `else`-blokken:

``` cpp
  if (SD.exists(filename)) {
    // Do something new here if file already exists...
  } else {
    // Create new file and open for writing
    file = SD.open(filename, O_CREAT | O_WRITE);
    file.println("Dette er den første linjen i filen.");
  }
```

I koden over ser du at `if`-blokken fortsatt er tom, så hva må gjøres dersom filen allerede eksisterer? Vi må fortsatt åpne filen og vi har allerede funnet ut at vi **ikke** vil bruke `O_CREAT`. Å bare bruke `O_WRITE` vil virkelig funke, men det er et lite problem med det også: Når en fil åpnes settes skrivemarkøren i filen på begynnelsen av filen. Dette er ikke noe problem i en ny fil, hvor ellers kunne markøren være, side det ikker er noe der fra før. Men å ha markøren i begynnelsen av en fil som allerede inneholder data vil føre til at du overskriver data som er der. Det vi virkelig vil er å åpne filen, og plassere markøren helt i slutten, slik at vi bare legger til ny tekst uten å røre det som allerede står i filen. Heldigvis har vi `O_APPEND` modusen som gjør nettopp dette. *append* er engelsk for: legg til. Med dette ser hele `if`- `else`-blokken slik ut:

``` cpp
  if (SD.exists(filename)) {
    // Open existing file for writing and append
    file = SD.open(filename, O_WRITE | O_APPEND);
    file.println("--------------------");
    file.println("Filen ble åpnet på nytt.");
  } else {
    file = SD.open(filename, O_CREAT | O_WRITE);
    file.println("Dette er den første linjen i filen.");
  }
```

Merk at koden over skriver en linje med 20 bindestreker i begynnelsen når en eksisterende fil åpnes. Dette vil gjøre det enkelt å finne skillet mellom to seksjoner i filen.

# helplink

## Test 2

Med de nye endringene vil du kunne slå av og på air:bit flere ganger. Filen på SD-kortet vil bare bli større og større ettersom air:bit skriver mer og mer tekst til filen.

Du kan slette filen fra SD-kortet når du har kortet plugget inn i datamaskinen. Da vil filen ikke lengre eksistere, og air:bit vil starte med en helt ny fil neste gang du setter SD-kortet inn i air:bit igjen.

``` cpp
#include <SD.h>

#define SD_CS_PIN 10

File file;
int counter;

void setup () {
  // Activate CS-Pin control
  pinMode(SD_CS_PIN, OUTPUT);

  // Startup SD-card reader
  SD.begin(SD_CS_PIN);

  // Define filename
  char filename[] = "testfile.txt";

  if (SD.exists(filename)) {
    // Open existing file for writing and append
    file = SD.open(filename, O_WRITE | O_APPEND);
    file.println("--------------------");
    file.println("Filen ble åpnet på nytt.");
  } else {
    file = SD.open(filename, O_CREAT | O_WRITE);
    file.println("Dette er den første linjen i filen.");
  }
  file.flush(); // Force saving data to SD-card

  // Start counter at 0
  counter = 0;
}

void loop() {
  counter += 1;

  file.print("Dette er linje nr.:");
  file.print(" ");
  file.print(counter);
  file.println("");

  file.flush();

  delay(1000); // Wait a second.
}
```

**Merk:** SD-kortet må være plugget i air:bit når du skrur på strømmen. Du kan ikke sette i SD-kortet mens air:bit er på og kjører. Koden som starter opp SD-kortleseren og åpner filen ligger i `setup`. Dvs., den kjøres bare én gang når air:bit starter opp.

## Veien videre

Å bare skrive verdien til en teller-variabel er kanskje litt kjedelig og gir ikke så mye mening. Men du har kanskje lagt merke til at det ikke er noe særlig forskjell fra å skrive til en seriell-kobling og å skrive til en fil på et SD-kort. Om du bare ser på koden i `loop`, og sammenligner med [teller-eksemplet som bruker seriell-kommunikasjon][counting-example], så ser du lite forskjell.

Ved å kombinere koden fra dette eksempelet med koden fra de tidligere eksempelene, kan du nå lagre målingene dine!

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg: **GPS-antenna**][gps]  
&darr; [Gå til hovedsider for **Guider**][guides]

[home]: airbit-Programmering
[gps]: Programmering-med-GPS-antenna
[guides]: airbit-Guider

[counting-example]: Variabler-og-telling-i-Arduino
[pinout]: airbit-Pinout
[notepad]: testfile-notepad.png
