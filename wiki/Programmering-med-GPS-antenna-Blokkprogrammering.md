Neste komponent vi kan se nærmere på er GPS-antennen til air:bit. Igjen kommer vi til å sjekke GPS posisjonen og skrive ut dataen fra GPS-en til seriell kommunikasjon. Men for å gjøre ting litt mer interessant kommer vi også til å lyse opp LED-lysene for å vise at du har kontakt med nok GPS-satellitter til å kunne bestemme posisjon.

Bruk av LED-lysene for å vise GPS signal kan være en god idé, siden det fort kan skje at man mister kontakt med GPS-en avhengig av vær- og terrengforhold, fjell eller tunneller på veien. Det vil også være mye vanskeligere å få GPS-kontakt når du er inne i hus, spesielt om huset har tykke betongvegger. Når du går rundt ute og tar målinger forventes det ikke at du har en datamaskin koblet til air:bit hele tiden for å kunne lese ut tekst du sender ut via seriell-koblingen.

## Ny Sketch

Igjen vil vi starte med en tom Sketch. Du kan klikke på _Discard_ knappen for å slette den tidligere koden. Du kan også trykke på _Save XML_ for å lagre prosjektet til senere.

## Laste ned og installere `TinyGPS++` biblioteket helplink

Igjen skal vi bruke et bibliotek du ikke finner i `Library Manager`. Klikk på følgende link for å laste ned: **[TinyGPS++ Biblioteket][tiny-gpp-dl-link]**

[![Nedlastning av TinyGPS++ biblioteket](TinyGPSPlusPlus-library-download.png)][tiny-gpp-dl-link]

Klikk på den store grønne pilen der det står Download. Husk å ikke åpne filen, men lagre den!

Åpne en `Arduino IDE`. I `Arduino IDE` programmet, finn menyen `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` og velg filen du nettopp lastet ned.

## TMP

*Merk at vi kaller `!` for **NOT**-operatoren. Dvs. betingelsen leses som: NOT isUseful*
## Kodeblokker helplink

GPS-en forteller oss om den har en gyldig posisjon. I tillegg til å printe ut teksten over USB-ledningen til datamaskinen vil vi bruke LED-lysene for å blinke grønt når vi har kontakt med GPS-satellittene, og blinke rødt når vi ikke får kontakt med satellittene. Dette betyr at vi må gjøre litt sjekking etter feilkoder.

Først må vi instruere Arduinoen til å prøve å oppdatere GPS informasjonen sin. Vi bruker `GPS update Data`-blokken i _Air:Bit; GPS_ sidefanen for å gjøre dette. Blokken vil *høre* etter innkommende data fra GPS-en, samt forsikre seg om at en fullstending GPS måling blir gjort.

Vi kan se at de fleste `GPS`-blokkene krever en `GPS_RX` og en `GPS_TX` verdi. De riktige verdiene for disse kan du finne i [pinout skjemaet][pinout]. Verdiene vil være `RX` og `TX` pinnene for GPS-antennen.

![][skjermbilde-update-GPS-blockly]

En tommelfingerregel i programmering er at man bør gjøre håndtering av feil øverst i kodeblokkene sine. Dvs. sjekk først for alle feil. Dette er fordi det som regel er enklere å teste mot spesifikke feilbetingelser. 

Etter denne blokken kan vi være sikre på at vi har fått informasjon fra GPS-en. La oss nå sjekke om vi har en gyldig posisjon. Vi bruker `GPS is Valid`-blokken for dette. Denne blokken returner enten `true` eller `false`, altså er det en sannhetsverdi (`bool`) verdi. Deklarer en variabel med navnet `gpsvalid`, gi den typen `bool` og koble `GPS`-blokken til den.  til `gps`-variabelen for dette. 

Dersom du har oppnådd kontakt med satellitter for å så ha mistet kontakten igjen (f.eks. fordi du går inn i en tunnel, et hus med tykke vegger, o.l.) vil `GPS is Valid`-blokken fortsette å gi `true` som svar, derfor må vi også sjekke om dataen har blitt oppdatert siden siste gang vi leste av. Vi bruker `GPS is Updated`-blokken for dette. Deklarer en ny `bool` variabel som tar imot denne verdien.

![][skjermbilde-variables-GPS-blockly]

Får at GPS informasjonen skal være nyttig må den være både _Valid_ of _Updated_. For å sjekke om begge variablene er sanne bruker vi en `Logical AND`-blokk, som vi finner under _Logic_ sidefanen. `Logical And` betyr *OG*, dvs. både høyre og venstre betingelse må være `true` for at hele betingelsen skal være `true`.

Trekk en `Logical AND`-blokk ut og koble de to forrige verdiene til den. Deklarer en tredje `bool` variabel som heter `isuseful` som tar in verdien ifra `Logical And`-blokken, slik at verdien blir lagret.

Hvis dataen ikke er brukbar, trenger vi ikke å gå videre i programmet. Trekk ut en `if - do`-blokk fra _Logic_ sidefanen.

``` cpp
  if (!(gps.location.isValid() && gps.location.isUpdated())) {
    // No valid position.
    // I.e. no GPS fix.
  }
```

Følgende kode vil være lik den over, men kan være litt enklere å lese:

``` cpp
  bool gpsValid = gps.location.isValid();
  bool gpsUpdated = gps.location.isUpdated();
  bool isUseful = gpsValid && gpsUpdated;
  if (!isUseful) {
    // No valid position.
    // I.e. no GPS fix.
  }
```

Vi lagrer sannhetsverdier (eller påstander) i variabler av type `bool`. I motsetning til `int` (som lagrer heltall) kan `bool`-variabler kun ha verdiene `true` eller `false`.


I tilfeller der vi ikke har en gyldig posisjon, har vi ikke kontakt med satellitten. Da ville vi blinke rødt. Husk hvordan vi gjorde det: 

1. Skru på rødt lys. `digitalWrite` med `HIGH` som andre argument.
1. Vent litt. `delay`
1. Skru av rødt lys. `digitalWrite` med `LOW` som andre argument.

``` cpp
  bool gpsValid = gps.location.isValid();
  bool gpsUpdated = gps.location.isUpdated();
  bool isUseful = gpsValid && gpsUpdated;
  if (!isUseful) {
    // No valid position.
    // I.e. no GPS fix.
    Serial.println("No valid GPS position");
    digitalWrite(LED_RED, HIGH);
    delay(500);
    digitalWrite(LED_RED, LOW);
    return;
  }
```

Vi bruker `return`-instruksjonen for å stoppe utføringen av kode inni `loop`-funksjonen. Dette vil få Arduinoen til å hoppe rett til neste gjennomgang av `loop`-funksjonen og starte GPS avlesningen **helt** på nytt igjen. I tilfeller der vi ikke har noe brukbar GPS informasjon, må vi bare vente til vi får et godt signal, så å starte på nytt virker som en god idé.

*Merk at koden over også har med en instruksjon for å skrive ut (over seriell-koblingen til datamaskinen) at vi ikke fikk kontakt.*

Neste steg er: Blinke grønt, og skrive ut GPS dataen over seriell-koblingen til datamaskinen.

Det enkleste først: Blink det grønne LED-lyset for å vise at vi har fått kontakt med nok GPS-satellitter.

`gps.location.lat()` og `gps.location.lng()` gir oss posisjonen i lengde- og breddegrader som desimaltall. I en `Serial.print()`-kommando kan du legge til et argument for å spesifisere antall desimaler bak kommaet. Eksemplet under vil skrive ut posisjonen med `6` desimaler bak kommaet:

``` cpp
  Serial.print("Latitude: ");
  Serial.print(gps.location.lat(), 6); // Latitude in degrees
  Serial.print("\t");
  Serial.print("Longitude: ");
  Serial.print(gps.location.lng(), 6); // Longitude in degrees
  Serial.println();
```

## Ferdig

Avhengig av hvilke verdier du skrive ut, hvordan du navngir dine variabler, osv. kan koden din se litt annerledes ut enn her. Men koden bør ligne dette:


## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake forrige neste steg: **Støvsensoren**][pm]  
&rarr; [Gå til neste steg: **SD-kortet og filer**][sd]  

[tiny-gpp-dl-link]: http://arduiniana.org/libraries/tinygpsplus/
[tiny-ggp-dl-img]: TinyGPSPlusPlus-library-download.png

[home]: airbit-Programmering
[pm]: Programmering-med-Støvsensoren
[sd]: Programmering-av-filer-på-SD-kortet

[pinout]: airbit-Pinout
[debugging-var-out-of-scope]: Feilsøking-av-programmeringsfeil#bruk-av-variabler-utenfor-scope

![][skjermbilde-and-GPS-blockly]
![][skjermbilde-check-GPS-blockly]
![][skjermbilde-GPS-blockly]

[skjermbilde-update-GPS-blockly]: skjermbilde-update-GPS-blockly.png
[skjermbilde-variables-GPS-blockly]: skjermbilde-variables-GPS-blockly.png
[skjermbilde-and-GPS-blockly]: skjermbilde-and-GPS-blockly.png
[skjermbilde-check-GPS-blockly]: skjermbilde-check-GPS-blockly.png
[skjermbilde-GPS-blockly]: skjermbilde-GPS-blockly.png
