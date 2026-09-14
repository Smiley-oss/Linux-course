
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
