# Johdanto
Tässä harjoituksessa rakensin toimivan HTTPS-palvelun omalle domainille tls-test000.linuxkurssi.xyZ.
Tavoitteena oli saada Apache2 toimimaan sekä HTTP- että HTTPS-liikenteellä, asentaa let's Encryptin sertifikaatti, varmistaa wwww-aliasin toimivuus ja toteuttaa HTTP -HTTPS- ohjaus.
Lisäksi tehtävässä piti testata yhteyksiä curl-komennolla ja lopuksi selittää, mitä TLS tekee ja miksi se on tärkeä. Halusin oppia, miten oikea palvelin konfiguroidaan, miten virheitä etsitään ja miten HTTPS oikeasti toimii käytännössä.
## Apache-konfiguraation korjaaminen
sudo apachectl configtest
Heti tuli virheitä vastaan. DocumentRoot oli kirjoitettu väärin, polku osoitti käyttäjälle linuxkurssi, vaikka oikea käyttäjä oli linuxuser. Apache ei käynnisty, jos DocumentRoot ei ole olemassa, joten korjasinpolun.
Toinen virhe oli kirjoitusvirhe CustoLog -> CustomLog.
Apache ei hyväksy tuntemattomia direktiivejä, joten tämäkin kaatoi palvelimen.
Kun nämä korjattiin. Apache käynnistyi normaalisti.

<img width="862" height="588" alt="Näyttökuva 2026-09-21 103815" src="https://github.com/user-attachments/assets/e641362c-1c96-488e-a691-45ff2d09fe1e" />

<img width="875" height="187" alt="Näyttökuva 2026-09-21 104648" src="https://github.com/user-attachments/assets/b54b5516-f264-4b94-859e-421c513849a0" />


## HTTPS-yhteyden käyttöönotto
curl -v https://tls-test000.linuxkurssi.xyz
Yhteys ei auennut, vaan curl antoi timeoutin.
DNS toimi, mutta portti 443 ei vastannut

<img width="855" height="262" alt="portti 443 ei toimi" src="https://github.com/user-attachments/assets/70aa2370-e51e-47ca-a4cd-160a301164ae" />

Avasin palomuurin: sudo ufw 443/tcp
Tämän jälkeen HTTPS alkoi toimia heti.
Tämä osoitti, että vaikka konfiguraatio olisi kunnossa, palomuuri voi silti estää kaiken.

<img width="816" height="505" alt="palomuuri" src="https://github.com/user-attachments/assets/f86290d0-3e2c-4a9a-8774-c07599f93a9e" />

## WWW-aliasin korjaaminen
Kun päädomain  toimi, testasin
https://www.tls-test000.linuxkurssi.xyz
sivu antoi 403 Forbidden.
Tämä tarkoittaa, että Apache löysi VirtualHostin, mutta ei antanut lupaa käyttää hakemistoa.
Lisäsin VirtualHoastiin ServerAlias www.tls-testoo.linukurssi.xyz
sekä oikean hakemisto-osion.

<img width="817" height="532" alt="virtualHost" src="https://github.com/user-attachments/assets/ff8c72f3-5723-4fd9-84f6-07089baa075a" />





