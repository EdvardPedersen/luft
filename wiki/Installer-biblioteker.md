For å få alle komponentene til airbit til å fungere må man installere noen ekstra biblioteker i `Arduino IDE` programmet.

# DHT biblioteket

Vi trenger et spesielt bibliotekt for å lese av temperatur målinger fra DHT-sensoren. Bibliotek heter `DHT sensor library`, og er laget av selskapet Adafruit. _Adafruit er en av de største produsentene for Arduino-utstyr._

`Arduino IDE` har en innebygd meny for å laste ned Arduino-biblioteker. Åpne en `Arduino IDE`. Klikk på `Sketch`&rarr;`Include library`&rarr;`Manage libraries...` for å åpne `Library Manager`.

![Arduino IDE Manage libraries][manage-libraries-menu]

I `Library Manager` som åpner seg, må du nå søke på `DHT sensor library` som er biblioteket for sensoren DHT i air:bit-oppsettet. Skriv inn `DHT sensor library` i søkefeltet. Klikk på resultatet som kommer opp, velg den nyeste versjonen i nedtrekksmenyen og klikk på `Install` (eller `Update` hvis det allerede er installert).

![Arduino IDE Library Manager: DHT sensor library][library-manager-dht-sensor-library]

# Eksterne (`.zip`) biblioteker

Noen biblioteker må bli hentet ned fra eksterne kilder, og installert på et annet vis enn `DHT sensor library`-biblioteket. Gå til [denne linken](https://github.com/EdvardPedersen/luft/releases/tag/v.0.0.1). Du skal ikke laste ned `Source` filene, men alle de andre filene under **Assets** i sise _release_ skal lastes ned. 

Linken igjen: [https://github.com/EdvardPedersen/luft/releases/tag/v.0.0.1](https://github.com/EdvardPedersen/luft/releases/tag/v.0.0.1 "airbit biblioteker")

Filene som skal lastes ned heter:
- Adafruit_Sensor
- AirBitDateTimeClass
- SDS011-airbit-teacher-workshop
- TinyGpsPlus

Etter du har lastet ned bibliotekene og lagret filene må vi installere bibliotekene i `Arduino IDE`. Klikk i menyen på `Sketch`&rarr;`Include library`&rarr;`Add ZIP library` (punktet under `Manage libraries`). Velg en av filene du nettopp lastet ned. Gjør dette for alle filene du lastet ned.

_**Merk:** Om nettleseren din ikke ba deg om å velge hvor filene skulle lagres, vil du mest sannsynligvis finne dem under `Downloads` (`Nedlastinger`)._

&uarr; [Gå til **innholdsfortegnelsen**][setup-home]  
&larr; [Gå tilbake til forrige steg: **Laste opp tom sketch**][upload-empty-sketch]  
&darr; [Gå til: **Guider**][guides-home]  

[setup-home]: Oppsett-for-programmering
[upload-empty-sketch]: Laste-opp-tom-sketch-til-Arduinoen
[guides-home]: airbit-Guider

[manage-libraries-menu]: Arduino-IDE-Manage-Library.png
[library-manager-dht-sensor-library]: Arduino-IDE-Library-Manager-DHTSensorLibrary.png
[adafruit_sensor-download]: GitHub-Adafruit_Sensor-download.png
