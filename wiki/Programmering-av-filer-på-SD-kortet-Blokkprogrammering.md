Når du er ute og tar målinger med air:bit vil du ikke hele tiden ha den tilkoblet til din datamaskin. air:bit har ikke noe som forbinder seg med internett, så du vil trenge en måte å lagre måledata på. air:bit har en kortleser for microSD-kort. Et microSD-kort er i prinsippet bare en minnebrikke, veldig lik en vanlig USB-minnepenn og faktisk nøyaktig lik minnekortet som finnes for å lagre data på nyere mobiltelefoner, digitalkamera, osv.

microSD-kortet som følger med air:bit har ganske stor lagringskapasitet. Du vil kunne lagre flere år med målingsdata på dette, så i prinsippet skal du ikke bekymre deg for at du ikke har nok plass på "harddisken" til air:bit.

Du vil legge merke til at microSD-kortet kommer med en adapter. Det lille microSD-kortet passer inn i den større adapteren og former slik et vanlig SD-kort. Laptopter nå til dags har som regel porter hvor du kan plugge inn SD-kort.

Når du plugger inn SD-kortet inn i en PC, vil det dukke opp som en harddisk i Filutforskeren (`Windows Explorer`, eller `Min datamaskin`, eller `This PC`) akkurat som en vanlig USB-minnebrikke også ville gjort.

Filnavn har en rekke betingelser som må oppfylles for at Arduinoen skal klare å lage filer.

- Filnavn kan ikke være lengre en 12 bokstaver.
- Filnavn må avslutte med en filtype forkortelse (f.eks. `.txt`, `.csv`).
	- Lengden på endelsen telles som en del av de 12 bokstavene til navnet.
- Filnavn kan kun inneholde engelske bokstaver (ingen `æ`, `ø`, `å`).

## Beskrivelse av eksemplet

I dette eksemplet skal du først lage en ny fil på SD-kortet. Deretter skal du skrive tekst inn i den filen. For hver gjennomgang gjennom `loop` skal du øke en teller og skrive ned verdien av telleren.

Dette vil ligne veldig på [kodeeksemplet om variabler og telling i Arduino][counting-example]. Du vil i prinsippet gjøre nøyaktig det samme i dette eksemplet, borsett fra at du skriver til en fil på SD-kortet, i stedet for seriell-koblingen til datamaskinen.

## Ny Sketch

Igjen vil vi starte med en tom Sketch. Du kan klikke på _Discard_ knappen for å slette den tidligere koden. Du kan også trykke på _Save XML_ for å lagre prosjektet til senere.

# helplink

## Laste ned og installere bibliotek

`SD`-biblioteket er forhåndsinstallert sammen med `Arduino IDE`. Det vil ikke være nødvendig å laste ned noe for dette eksemplet.

# helplink

## Kodeblokker

Det første vi trenger for å lage tellereksempelet er en teller. Deklarer en `int` variabel med navnet `counter`. Gi variabelen verdien `0`, slik at vi alltid starter programmet med en nullstilt teller.

For at vi skal kunne kjøre programmet kontinuerlig, uten å starte telleren våres på nytt, trenger vi en `while`-`do`-løkke. Vi finner løkken inne i _Control_ sidefanen. En slik løkke vil starte om og om igjen, så lenge kjørebetingelsen dens er sann. Inne i _Logic_ sidefanen finner vi en blokk som heter `Bool`, som gir en sannhetsverdi. Hvis vi kobler en `true` `Bool`-blokk til løkken vil den aldri stoppe.

Inne i løkken kan vi sette inn en `set variable`-blokk fra _Variable_ sideblokken, som øker telleren våres med en for hver gjennomgang av løkken. For å øke variabelen må vi bruke en `addisjon`-blokk ifra _Math_ sidefanen. Sett den nye konstruksjonen i toppen av `while`-`do`-løkken.

Programmet så langt bør se slik ut:

![][skjermbilde-while-SD-blockly]

Under økningen av telleren kan vi sette in skrivningen til microSD-kortet. For å skrive til kortet trenger vi andre blokker enn `Serial Print`-blokkene. Åpne _Air:bit; SD_ sidefanen. Der finner vi en del blokker som ligner `Serial` blokkene vi brukte tidligere. Trekk ut en `SD Print`-blokk, og en `SD Println`-blokk, og sett dem sammen. Det er viktig at de har samme filnavn, ikke har norske eller spesielle bokstaver, er under 8 bokstaver, og at navnet slutter med `.txt`, men navnet trenger ikke å være `filename.txt`. 

Fra [pinout skjemaet][pinout] ser du at det er flere pinner som kobler SD-kortleseren til Arduinoen. Vi trenger `CS` pinnen i kodeblokkene vi skal bruke.

Det som er koblet til `Input string` variabelen er det som vil bli skrevet til SD-kortet. I `SD Print` blokken kan vi skrive inn `Dette er line nr.:`, mens i `SD Println`-blokken kan vi sette inn teller variabelen. Dette gjør at du vil skrive en linje for hver gjennomgang av løkken som forteller hvilken linje som nettop ble skrevet.

En siste blokk trengs for å overføre teksten til SD-kortet. Denne blokken heter `SD Flush`. Uten denne blokken vil ingenting bli skrevet til SD-kortet.

Det kan også være en god idé å legge til en `Delay`-blokk etter `SD Flush`-blokken på slutten, slik at du kun skriver en ny linje etter f.eks. ett sekund med venting.

## Ferdig

Kodeblokkene bør nå se slik ut:

![][skjermbilde-SD-blockly]

Kodelinjene skal se slik ut:

```cpp
#include <SD.h>

#include "AirBitUtilsClass.h

int counter;

#define SD_CS_PIN 10

File file;
AirBitUtilsClass airbitUtils;
void setup()
{
  // Activate CS-Pin control
  pinMode(SD_CS_PIN, OUTPUT);

  // Startup SD-card reader
  SD.begin(SD_CS_PIN);

  // Define filename
  char filename[] = "filename.txt";

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

}


void loop()
{
  counter = 0;
  while (true) {
    counter = counter + 1;
    file.print("Dette er line nr.:");
    file.print(counter);
    file.flush();
    delay(1000);
  }

}
```

## Last ned koden

For å kunne kjøre koden må du laste den ned. Dette gjør du ved å trykke på _Save Arduino Code_ knappen på [BlocklyDuino siden](http://airbit.uit.no:8080). Du kan velge hva filen din skal hete. Gå så til hvor du lastet ned filen og åpne filen med `Arduino IDE`-programmet. Den vil spørre deg om å lage en en mappe med samme navn som filen, godta forespørselen. Du kan så overføre koden til Arduinoen som vanlig, ved å trykke på _Last Opp_ knappen (den høyrepekende pilen). 

### Test Koden

**Merk:** SD-kortet må være plugget i air:bit når du skrur på strømmen. Du kan ikke sette i SD-kortet mens air:bit er på og kjører. Koden som starter opp SD-kortleseren og åpner filen ligger i `setup`. Dvs., den kjøres bare én gang når air:bit starter opp.

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

## Veien videre

Å bare skrive verdien til en teller-variabel er kanskje litt kjedelig og gir ikke så mye mening. Men du har kanskje lagt merke til at det ikke er noe særlig forskjell fra å skrive til en seriell-kobling og å skrive til en fil på et SD-kort. Om du bare ser på koden i `loop`, og sammenligner med [teller-eksemplet som bruker seriell-kommunikasjon][counting-example], så ser du lite forskjell.

Ved å kombinere koden fra dette eksempelet med koden fra de tidligere eksempelene, kan du nå lagre målingene dine!

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg i Blokkprogrammeringen: **GPS-antenna**][gps]  
&darr; [Gå til hovedsider for **Guider**][guides]

[home]: airbit-Programmering
[gps]: Programmering-med-GPS-antenna-Blokkprogrammering
[guides]: airbit-Guider

[counting-example]: Variabler-og-telling-i-Arduino
[pinout]: airbit-Pinout
[notepad]: testfile-notepad.png

[skjermbilde-while-SD-blockly]: skjermbilde-while-SD-blockly.png
[skjermbilde-SD-blockly]: skjermbilde-SD-blockly.png
