De enkleste komponentene på air:bit er LED lyspærene. Så for å starte med skal vi prøve å få dem til å blinke. Du kan senere bruke LED til å vise status på f.eks. GPS signalet eller blinke mens du tar målinger, osv. Det kan være en enkel måte å skjekke om alt er ok mens du går rundt og tar målinger. Husk at air:bit ikke har noen skjerm, og du vil neppe alltids ha med datamaskinen din for å kunne snakke direkte med Arduinoen.

## Komme i gang helplink

Vi starter med en helt ny Sketch. Åpne opp en ny fane med [BlocklyDuino](http://airbit.uit.no:8080), som er _Blokkprogrammerings_-verktøyet vi skal bruke.

Hvis du klikker på _Arduino_ kan vi se _tomme_ funksjonene for `setup` og `loop` som vi allerede har sett på tidligere.

## Kodeblokker

Hvis vi går tilbake til _Blocks_ fanen vil vi kunne se i sidefanen til høyre forskjellige grupperinger. En av disse heter _Air:Bit; LED_. Hvis vi åpner denne vil vi se den første blokken vi skal bruke. Blokken heter _LED_.

Denne blokken slår på et LED-lys som korresponderer med fargen som er skrevet i `PIN` dropdown-meyen. I starten står fargen på `GREEN`, altså vil det grønne lyse blinke. På høyre side av blokken ser vi en til blokk med en tall verdi. Den er koblet til en variabel kalt `Delay`. Dette viser hvor lenge lyset skal være på, før det slår seg av. Tallet viser tiden gitt i millisekunder.

Grip en av disse blokkene og trekk den ut på skjermen. Hvis vi går tilbake til _Arduino_ fanen vil vi kunne se at koden har forandret seg.

Så må vi ta en titt på pinout skjemaet og finne hvilke pinner vi skal bruke for LED pærene. [Klikk her for å se **Pinout-skjemaet**.][pinout]

Fra skjemaet kan vi se at vi skal bruke pinne nummer `A1` for den blanke, rødt-lysende LED, og `A0` for den grønne LED. 

I _Arduino_ fanen kan vi se at begge disse _LED_ pinnene ble deklarert når vi trakk ut _LED_ blokken.

Hvis vi har lyst til å blinke både med det røde og det grønne LED-lyset kan vi legge til enda en _LED_ blokk. Gå tilbake til _Blocks_ fanen og trekke enda en _LED_ blokk inn i hovedområde, under den forrige blokken. Sett `PIN` fargen til denne blokken til den andre fargen, slik at vi har en `GREEN` og en `RED` blokk. Nå vil lysene blinke annenhver gang. Ved å øke tallet til høyre på blokkene vil lysene være på lengre.

For øyeblikket vil et av lysene alltid være på. Hvis vi har lyst til å unngå dette kan vi legge til enda en blokk. Blokken vi skal legge til heter _Delay_, og vi finner den under _Control_ sidefanen. Denne har igjen et tall som viser hvor lenge den programmet skal vente når den når denne biten av koden. Tallet gir tiden i millisekunder. Trekk denne blokken, og koble den til nederst på programmet.

## Ferdig

Mye rart man kan gjøre med blinking og mange rare rytmer man kan få til her om man bare er litt kreativ, men hvis vi tar utgangspunkt i den helt kjedelige blinkingen i ett sekund mellomrom, vi du få kode som ligner på dette:

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg: **Pinout**][pinout]  
&rarr; [Gå til neste steg i Blokk programmering: **Temperatursensoren**][dht]  

[home]: airbit-Programmering
[pinout]: airbit-Pinout
[dht]: Programmering-med-Temperatursensoren-Blockly
