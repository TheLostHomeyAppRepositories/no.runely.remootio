Gir Homey flytkort for å samhandle med dine garasjeporter

Forutsetninger for Websocket API

- Din Remootio er ferdig satt opp
- Remootio og Homey må være på samme nettverk / VLAN
- Statussensoren er installert og aktivert i Remootio app'en (Homey må kunne vite nåværende status på dine garasjeporter)
- Statisk IP-adresse for Remootio er anbefalt
- "Output configuration" for din Remootio MÅ være stilt inn på en av disse:
  1: Output 1: disabled Output 2: gate impulse control
  2: Output 1: gate impulse control Output 2: free relay output
  3: Output 1: free relay output Output 2: gate impulse control
  4: Output 1 & Output 2: gate impulse control
  5: Output 1: gate impulse control Output 2: disabled

Websocket API

API i Remootio må være aktivert før man kan legge til Remootio enheter i Homey. Instruksjoner på Remootio sine sider
Noter ned 'API Secret Key' og 'API Auth Key' vist når API aktiveres

Hvilken driver bør jeg bruke

For output configurations 1, 4 og 5 - bruk "Remootio (gate impulse control)"

For output configuration 2 - bruk "Remootio (Output 1: gate impulse control, Output 2: free relay output)"

For output configuration 3 - bruk "Remootio (Output 1: free relay output, Output 2: gate impulse control)"

Sammenkobling

Når du legger til en Remootio-enhet, kan du velge å finne Remootio-enhetene dine automatisk på ditt nettverk via mDNS, eller du kan legge til enheten selv

MERK: Remootio3 med Software versjon >= 2.40 MÅ legges til manuelt fordi mDNS-støtte for øyeblikket ikke er tilgjengelig siden Remootio har skrevet om Softwaren til å støtte HomeKit og i prosessen ødelagt mDNS-støtten

- Når du blir spurt, legg inn 'API Secret Key' og API Auth Key' -> Funnet i Websocket API i Remootio app'en på din telefon
- Velg 'Finn enheter (mDNS)' for å finne enhetene automatisk på ditt nettverk med mDNS
- Velg 'Opprett enhet manuelt' for å manuelt opprette enheten ved å angi 'Navn', 'Serial Number' og 'IP-adresse'

Reversert sensorlogikk

Dersom du har reversert sensorlogikken i Remootio, må du sette 'Er sensorlogikken reversert' til 'Ja' i Remootio enhetsinnstillingene i Homey

For ytterligere dokumentasjon eller feilsøking, sjekk ut GitHub-siden


Forutsetninger for Device API

- Din Remootio er ferdig satt opp
- Din Homey og din Remootio enhet kan være på forskjellige LAN eller WAN
- Statussensoren er installert og aktivert i Remootio app'en (Homey må kunne vite nåværende status på dine garasjeporter)
- "Output configuration" for din Remootio MÅ være stilt inn på en av disse:
  1: Output 1: disabled Output 2: gate impulse control
  2: Output 1: gate impulse control Output 2: free relay output
  3: Output 1: free relay output Output 2: gate impulse control
  4: Output 1 & Output 2: gate impulse control
  5: Output 1: gate impulse control Output 2: disabled

Device API

Device API er hostet på Remootio's servere, så din Remootio enhet må ha tilgang til internett og kunne nås av Device API

VIKTIG: Device API er begrenset til 300 forespørsler per 20 dager (dette er Remootio's grenser!). Dette betyr at du bør kunne åpne og lukke porten/garasjedøren din rundt 6 ganger om dagen.
Hvis du overskrider denne grensen vil du ikke kunne sende forespørsler til Device API på en stund (usikker på tidsrammen)!

VIKTIG: Det er en automatisk spørringsstatus (deaktivert som standard), som kan konfigureres til automatisk å spørre om porten/garasjeportens status hver x time.
Du bør holde dette deaktivert eller bruke med en veldig høy verdi, for eksempel hver 7. time eller høyere, for ikke å overbruke antall spørringer som vil føre til at du ikke kan bruke Device API!

VIKTIG: Hvis porten/garasjedøren betjenes utenfor Remootio-app'en, vil statusen i Homey Remootio-app'en IKKE gjenspeile dette før den er endret i Remootio-app'en i Homey, eller når den automatiske spørringsstatusen kjøres (hvis aktivert)

For å tillate at Device API kan kontrollere din Remootio enhet, må du sette opp en App-Free key gjennom Remootio app'en på din telefon:
- Gå til shared keys
- Klikk på 'Share a new key' og velg 'Share unique key (recommended)'
- Gi nøkkelen et meningsfullt navn
- På neste skjerm, klikk på 'App-free key' og klikk på 'Share link'
- Kopier bare 'token'-verdien fra URL'en. Denne verdien skal du legge til i Remootio app'en på Homey under "parring"

Hvilken driver bør jeg bruke

Remootio Device API

Denne driveren er den eneste driveren som supporterer Device API til Remootio.

Parring

Når du legger til Remootio enheten til din Homey, kopier inn 'token'-verdien funnet i de foregående stegene. Parringsprosessen vil spørre Device API ved hjelp av 'token'-verdien og finne info om enheten.

Innstillinger

Sekunder for statusendring

Antall sekunder før porten/garasjeporten har endret status etter betjening
En forespørsel sendes til device API etter dette antallet sekunder for å angi riktig enhetsstatus. Dette gjøres for å sikre at enhetsstatusen har samme status som Remootio-enheten.

Timer mellom statusspørringer

Antall timer mellom hver forespørsel om port-/garasjeportstatus. Sett til 0 for å deaktivere automatiske statusspørringer
Siden Remootio's Device API har en veldig lav forespørselgrense, bruk denne innstillingen med forsiktighet!

For ytterligere dokumentasjon eller feilsøking, sjekk ut GitHub-siden
