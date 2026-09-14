
# Palvelimen perusasetukset
## Tämä dokumentti kuvaa Azure‑virtuaalikoneen peruskonfiguraation, jossa rakennetaan turvallinen ja toimiva Linux‑palvelin. Raportti etenee vaiheittain järjestelmän päivityksestä verkon diagnostiikkaan ja käyttäjähallintaan. Jokainen vaihe sisältää teknisen analyysin ja havainnot, jotka osoittavat ymmärryksen palvelimen toiminnasta ja sen hallinnasta.
## 1. Järjestelmän päivitys – palvelimen peruskuntoon saattaminen
Heti VM:lle kirjautumisen jälkeen suoritin järjestelmän päivityksen. Tämä vaihe on kriittinen, koska pilvipalvelimissa käytettävät kuvat voivat olla useita viikkoja tai kuukausia vanhoja. Päivitys varmistaa:

- uusimmat tietoturvakorjaukset

- ajantasaiset paketit

- yhteensopivuuden tulevien konfiguraatioiden kanssa

Päivityksen jälkeen palvelin on turvallinen ja valmis vastaanottamaan uusia palveluita kuten SSH‑avaintunnistautumisen ja Apache‑webpalvelimen.
