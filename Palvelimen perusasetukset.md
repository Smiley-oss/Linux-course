
# Palvelimen perusasetukset
## Tämä dokumentti kuvaa Azure‑virtuaalikoneen peruskonfiguraation, jossa rakennetaan turvallinen ja toimiva Linux‑palvelin. Raportti etenee vaiheittain järjestelmän päivityksestä verkon diagnostiikkaan ja käyttäjähallintaan. Jokainen vaihe sisältää teknisen analyysin ja havainnot, jotka osoittavat ymmärryksen palvelimen toiminnasta ja sen hallinnasta.
## 1. Järjestelmän päivitys – palvelimen peruskuntoon saattaminen
Heti VM:lle kirjautumisen jälkeen suoritin järjestelmän päivityksen. Tämä vaihe on kriittinen, koska pilvipalvelimissa käytettävät kuvat voivat olla useita viikkoja tai kuukausia vanhoja. Päivitys varmistaa:

- uusimmat tietoturvakorjaukset

- ajantasaiset paketit

- yhteensopivuuden tulevien konfiguraatioiden kanssa

Päivityksen jälkeen palvelin on turvallinen ja valmis vastaanottamaan uusia palveluita kuten SSH‑avaintunnistautumisen ja Apache‑webpalvelimen.
## 2. SSH‑palvelin ja avaintunnistautuminen – turvallisen etäyhteyden varmistaminen
SSH‑palvelimen tila
Tarkistin SSH‑palvelimen tilan varmistaakseni, että etäyhteys toimii luotettavasti. Azure‑VM:ssä SSH on yleensä valmiiksi käynnissä, koska se on ensisijainen hallintakanava. Palvelimen tila kertoo:

- onko palvelu aktiivinen

- kuunteleeko se porttia 22

- onko konfiguraatiossa virheitä

Tämä vaihe varmistaa, että palvelin on hallittavissa myös jatkossa, vaikka salasana poistettaisiin käytöstä.

<img width="957" height="776" alt="ssh-status" src="https://github.com/user-attachments/assets/9370e642-07fe-43b1-80c2-d64288d19928" />

### Avaintunnistautumisen käyttöönotto
Avaintunnistautuminen korvaa salasanapohjaisen kirjautumisen ja nostaa tietoturvan tasoa merkittävästi. Avaimet ovat:

- vahvempi suojaus kuin salasanat

- vastustuskykyisiä brute‑force‑hyökkäyksille

- käytännöllisiä automaattisiin yhteyksiin

Prosessi koostuu kolmesta vaiheesta:

- Avainten luonti omalla koneella

- Julkisen avaimen siirto palvelimelle

- Testi, jossa varmistetaan, että kirjautuminen toimii ilman salasanaa

Kun avaintunnistautuminen toimii, palvelin voidaan koventaa poistamalla salasanakirjautuminen kokonaan.

<img width="1187" height="856" alt="ssh-avain luonti ilman" src="https://github.com/user-attachments/assets/9bc17928-098d-4ddf-b7ad-cf47a1f6133f" />

## 3. Apache‑palvelimen asennus ja testaus – web‑palvelun käyttöönotto
Apache on yksi maailman käytetyimmistä web‑palvelimista. Sen asennus Azure‑VM:lle luo pohjan tuleville tehtäville, kuten TLS‑sertifikaattien käyttöönotolle.

Oletussivun muokkaus
Muokkasin Apache‑palvelimen oletussivun varmistaakseni, että:

- palvelin toimii

- dokumenttijuuri on oikea

- oikeudet ovat kunnossa

- selain ja curl palauttavat saman sisällön
Tämä vaihe osoittaa, että HTTP‑liikenne kulkee Azure‑palomuurin ja UFW:n läpi oikein.

<img width="955" height="235" alt="curl-testi" src="https://github.com/user-attachments/assets/ae1892c3-ff50-4b6c-a086-2bfc63b4bdcd" />
<img width="932" height="170" alt="Oletussivun muokkaus" src="https://github.com/user-attachments/assets/90c5f961-9a61-4ce2-9ed0-12fa715187f0" />

## 4. UFW‑palomuuri – palvelimen suojauskerroksen rakentaminen
UFW (Uncomplicated Firewall) tarjoaa selkeän tavan hallita Linux‑palvelimen palomuuria. Azure‑VM:ssä palomuuri toimii kahdessa kerroksessa:

Azure Network Security Group (NSG) – hallitsee liikennettä pilviverkon tasolla

UFW – hallitsee liikennettä itse VM:n sisällä

Konfiguroin UFW:n sallimaan vain välttämättömät portit:

SSH (22) – hallintayhteys

HTTP (80) – web‑palvelu

HTTPS (443) – salattu web‑palvelu

<img width="935" height="362" alt="UFW-asetuket" src="https://github.com/user-attachments/assets/e166b12c-e129-4543-83b6-e80dc107b0cf" />

## 5. Networking – IP‑osoitteet ja reititys Azure‑ympäristössä
IP‑osoitteiden tarkastelu
Azure‑VM:ssä näkyy vain yksityinen IP‑osoite, koska Azure käyttää NAT‑tekniikkaa. Julkinen IP näkyy vain Azure‑portaalissa tai ulkoisella palvelulla. Tämä on tärkeä havainto, koska:

ping VM:n julkiseen IP:hen ei tule VM:ltä itseltään

reititystaulu kertoo vain sisäisestä verkosta

diagnostiikka täytyy tehdä oikealla rajapinnalla (eth0)

<img width="902" height="325" alt="ip-osoite" src="https://github.com/user-attachments/assets/4b2cbf80-6798-49b6-b0c1-cd904efb4487" />

### Reititystaulu
Reititystaulu osoittaa, miten VM kommunikoi ulkomaailman kanssa. Azure‑ympäristössä:

- default gateway osoittaa Azure‑verkon sisäiseen reitittimeen

- kaikki ulkoliikenne kulkee tämän kautta

- sisäverkko on eristetty muista asiakkaista

# 6. Packet Inspection – HTTP, SSH ja ICMP liikenteen analyysi
Tässä vaiheessa tarkastelin liikennettä reaaliajassa kahdella työkalulla: ngrep ja tcpdump. Molemmat tarjoavat syvällisen näkymän verkon toimintaan.

HTTP‑liikenne (portti 80)
HTTP on selkokielinen protokolla, joten liikenteessä näkyy:

- HTTP‑pyynnöt (GET)

- otsikkotiedot (Host, User-Agent)

- palvelimen vastaus (200 OK)

- sivun sisältö

Tämä osoittaa, että Apache toimii ja että liikenne kulkee ilman salausta.

SSH‑liikenne (portti 22)
SSH‑liikenne näkyy ngrepissä vain binääridatana. Tämä on odotettua, koska SSH:

- salaa koko istunnon

- estää sisällön tarkastelun

- suojaa komennot, salasanat ja tiedonsiirron

Tämä vahvistaa, että SSH‑avaintunnistautuminen toimii turvallisesti.

CMP (ping)
ICMP‑liikenteessä näkyy:

- Echo Request

- Echo Reply

- TTL‑arvot

- pakettien koko

- ICMP on kriittinen protokolla verkon diagnostiikassa, koska se:

- kertoo yhteyden toimivuudesta

- paljastaa viiveet

- auttaa reititysongelmien selvittämisessä.

tcpdump‑vertailu
tcpdump tarjoaa teknisemmän näkymän kuin ngrep. Se näyttää:

- pakettien rakenteen

- tarkat otsikkotiedot

- checksum‑arvot

- TTL‑arvot

Tämä työkalu soveltuu syvälliseen verkkoanalyysiin ja ongelmanratkaisuun.

<img width="947" height="986" alt="ngrep HTTP" src="https://github.com/user-attachments/assets/4d1c505a-4559-40ac-9533-73a835f7ace7" />

<img width="797" height="467" alt="ngrep asennus" src="https://github.com/user-attachments/assets/78dd6105-389f-4626-b528-e2176d6da020" />

<img width="817" height="515" alt="tcpdump" src="https://github.com/user-attachments/assets/05875dc6-01bb-4722-b20f-e8a433c72730" />

# Yhteenveto
Module 4:n aikana rakensin Azure‑VM:lle toimivan ja turvallisen Linux‑palvelimen. Tehtävät kattoivat:

- järjestelmän päivityksen

- SSH‑avaintunnistautumisen

- Apache‑webpalvelimen käyttöönoton

- UFW‑palomuurin konfiguroinnin

- verkon rakenteen ja liikenteen analyysin.

Kokonaisuutena palvelin on nyt:

- turvallinen

- hallittavissa avaintunnistautumisella

- valmis TLS‑sertifikaattien käyttöönottoon (Module 5)

- varustettu toimivalla web‑palvelulla.



