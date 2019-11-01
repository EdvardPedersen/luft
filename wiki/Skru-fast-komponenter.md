Nå vi har loddet fast header-pinner og plugger på alle delene, så kan vi skru fast delene i treboksen.

## Dette trenger du

* Treboksen
* Arduino Uno
* Header shield
* Støvmåleren og ledningen
* Temperatursensoren ("*Sukkerbiten*")  
  og ledningen
* GPS modulen og antenna
* microSD kortleseren
* ZipLock-posen med smådeler
* Tang

## Start

Ta alle delene og ledninger fra hverandre og løsne shieldet fra Arduinoen. Dette vil gjøre det enklere for deg å komme til skrue-hullene og feste mutter senere.

# helplink

## Skruene

Ta frem treboksen din og finn plastskruene og mutterene i ZipLock-posen. Plassér Arduinoen på sin rette plass i treboksen. Plugg skruer inn i hullene i Arduinoen og boksen fra utsiden. Skru fast Arduinoen. (Merk at en av mutterene har veldig lite plass, så det er best å skru skruen og holde fast mutteren. Det går også fint hvis du ikke skrur inn alle skruene)

Før du skrur fast støvmåleren, bør du koble til den hvite og blå ledningen inn i støvmåleren. Ledningen passer bare én vei, så du kan ikke gjøre noe galt der. Plassér så Støvmåleren ved siden av Arduinoen og skru fast.

![Skruer i treboksen][skrews-placement]

Plugg ledningen i temperatursensoren. Plassér sensoren i siden på treboksen. Den hvite klossen ("*Sukkerbiten*") passer gjennom det store firkantete hullet og skruen går da gjennom et tilsvarende hull i sensoren. *Sukkerbiten* går ikke hele veien gjennom hullet, den skal såvidt stikke ut fra sideplaten. Skru fast _sukkerbiten_.

# helplink

## Plugge sammen komponenter

Klipp til litt dobbeltsidig teip fra ZipLock-posen. Dette skal du lime fast på den gule/hvite siden av GPS antenna.

![Dobbeltsidig teip på GPS antenna][tape-on-gps]

Ta nå shieldet og plugg det inn i Arduinoen. Så kan du ta GPS modulen og plugge den i oppå shieldet. Fjern beskyttelsen på teipen på GPS antenna og lim den fast nederst i det øverste høyre hjørnet av treboksen, pass på at det er nok plass til at lokket kan lukkes. Plugg i og knepp til ledningen til GPS chipen.

![Plassering av GPS module og antenne][gps-placement]

Så kan du plugge i SD-kortleseren. Den vil stikke ut gjennom den smale åpningen sideplaten.

![Plassering av kortleseren][sd-placement]

# helplink

### Ledningen til Temperatursensoren
Temperatursensoren har tre plugger som er merket med `+`, `out` og `-`. Det er viktig at du plugger i rett ledning i rett plugg i shieldet. 

Se nå på pinnene for temperatursensoren på shieldet. Der er det fire pinner med markeringene `VCC`, `Data` og to ganger `GND`.

* Pinnen for `VCC` skal kobles til `+` på sensoren
* Pinnen for `Data` skal kobles til `out` på sensoren
* En av pinnene for `GND` (spiller ingen rolle hvilken) skal kobles til `-` på sensoren.

# helplink

### Ledningen til Støvmåleren

Se på Støvmåleren der ledningen er koblet til. Her er det enklest å se at markeringen lengst til venstre viser `TxD`. Følg den siden av ledningen og plugg inn ledningen med den siden i `TX`-pinnen for støvmåleren på shieldet. Siden det bare er fire pinner på shieldet vil det være en plugg til overs.

![Tilkobling av ledningen til Støvsensoren][pm-cable]

# helplink

### Alt på plass

Nå burde boksen se ut som vist på bildet under

![Nesten ferdig][all-placement]

# helplink

## Batteriet

Lim til slutt fast to brede strimler av dobbeltsidig teip på bunnplaten og sideplaten på nedsiden av treboksen, der det står `Batteri`. Ta batteriet og lim det fast.

## Gå videre

&uarr; [Gå til **innholdsfortegnelsen**][home]  
&larr; [Gå tilbake til forrige steg: **Lodde sensorene**][sensors]  
&rarr; [Gå til neste steg: **Sette på lokket**][lid]  

[home]: Guide-Bygging-og-Lodding
[sensors]: Lodde-sensorene
[lid]: Sette-på-lokket

[skrews-placement]: 20171019_111902.jpg
[tape-on-gps]: 20171019_130944.jpg
[gps-placement]: 20171019_132825.jpg
[sd-placement]: 20171019_132839.jpg
[pm-cable]: 20171019_120201_cable.jpg
[all-placement]: 20171019_134129.jpg
