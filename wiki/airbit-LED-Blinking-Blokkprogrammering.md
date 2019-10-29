De enkleste komponentene på air:bit er LED lyspærene. Til å starte med skal vi prøve å få dem til å blinke. Du kan senere bruke LED til å vise status på f.eks. GPS signalet eller blinke mens du tar målinger, osv. Det kan være en enkel måte å skjekke om alt er ok mens du går rundt og tar målinger. Husk at air:bit ikke har noen skjerm, og du vil neppe alltids ha med datamaskinen din for å kunne snakke direkte med Arduinoen.

## Komme i gang helplink

Vi starter med en helt ny Sketch. Åpne opp en ny fane med [BlocklyDuino](http://airbit.uit.no:8080), som er _Blokkprogrammerings_-verktøyet vi skal bruke.

Hvis du klikker på _Arduino_ fanen kan du se de _tomme_ funksjonene for `setup` og `loop` som vi allerede har sett på tidligere.

## Kodeblokker

Hvis vi går tilbake til _Blocks_ fanen, vil vi kunne se forskjellige grupperinger i sidefanen til høyre. En av disse heter _Air:Bit; LED_. Hvis vi åpner denne vil vi se den første blokken vi skal bruke. Blokken heter _LED_.

Denne blokken slår på et LED-lys som korresponderer med fargen som er skrevet i `PIN` nedtrekksmenyen. Fargen er i starten satt til `GREEN`, som betyr at det grønne lyset kommer til å blinke. Man kan endre fargen ved å trykke på menyen og velge et annet alternativ. På høyre side av blokken ser vi en til blokk med en tall verdi. Den er koblet til en variabel kalt `Delay`. Denne blokken viser hvor lenge lyset skal være på av gangen. _Tallet viser tiden gitt i **millisekunder**._

Grip en av disse blokkene og trekk den ut på skjermen. Hvis vi går tilbake til _Arduino_ fanen vil vi kunne se at koden har forandret seg.

I _Arduino_ fanen kan vi se at både `LED_RED` og `LED_GREEN` ble definert. De ble deklarert når vi trakk ut _LED_ blokken. Om vi tar en titt på pinout skjemaet vil vi se at verdiene til disse variablene korresponderer med _LED_ pinnene. [Klikk her for å se **Pinout-skjemaet**.][pinout]

Hvis vi har lyst til å blinke både med det røde og det grønne LED-lyset kan vi legge til enda en _LED_ blokk. Gå tilbake til _Blocks_ fanen og trekke enda en _LED_ blokk inn i hovedområde, under den forrige blokken. Sett `PIN` fargen til denne blokken til den andre fargen, slik at vi har en `GREEN` og en `RED` blokk. Nå vil lysene blinke annenhver gang. Ved å øke tallet til høyre på blokkene vil lysene være på lengre.

![][skjermbilde-LEDs-blockly]

For øyeblikket vil et av lysene alltid være på. Hvis vi har lyst til å unngå dette kan vi legge til en `Delay` blokk. Denne blokken finner vi under _Control_ sidefanen. Denne har et tallblokk som viser hvor lenge den skal få programmet til å vente. Tallet gir tiden i millisekunder. Trekk ut denne blokken, og koble den til nederst på programmet.

## Ferdig

Kodeblokkene ser nå noe sånt som det her ut:

![][skjermbilde-LEDs-blockly-finished]

Og Kodelinjene under _Arduino_ fanen ser nå noe sånt som dette ut:

```cpp
#define LED_RED A1
#define LED_GREEN A0

void setup()
{
  pinMode(LED_GREEN, OUTPUT);

  pinMode(LED_RED, OUTPUT);

}


void loop()
{
  digitalWrite(LED_GREEN, HIGH);
  delay(1000);
  digitalWrite(LED_GREEN, LOW);
  digitalWrite(LED_RED, HIGH);
  delay(1000);
  digitalWrite(LED_RED, LOW);
  delay(1000);

}
```

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg: **Pinout**][pinout]  
&rarr; [Gå til neste steg i Blokkprogrammeringen: **Temperatursensoren**][dht]  

[home]: airbit-Programmering
[pinout]: airbit-Pinout
[dht]: Programmering-med-Temperatursensoren-Blokkprogrammering
[skjermbilde-LEDs-blockly]: skjermbilde-LEDs-blockly.png
[skjermbilde-LEDs-blockly-finished]: skjermbilde-LEDs-blockly-finished.png
