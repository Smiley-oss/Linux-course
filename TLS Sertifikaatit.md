# Johdanto
Tässä harjoituksessa rakensin toimivan HTTPS-palvelun omalle domainille tls-test000.linuxkurssi.xyZ.
Tavoitteena oli saada Apache2 toimimaan sekä HTTP- että HTTPS-liikenteellä, asentaa let's Encryptin sertifikaatti, varmistaa wwww-aliasin toimivuus ja toteuttaa HTTP -HTTPS- ohjaus.
Lisäksi tehtävässä piti testata yhteyksiä curl-komennolla ja 
selittää, mitä TLS tekee ja miksi se on tärkeä osa modernia verkkoturvallisuutta.

Harjoituksen tarkoitus ei ollut vain saada sivu toimimaan, vaan ymmärtää koko prosessi, konfiguraatio rakentaminen, virheiden etsiminen, palomuurin hallinta, VirtualHoastein logiikka ja lopulta turvallisen yhteyden varmistaminen.

## Apache-konfiguraation korjaaminen
sudo apachectl configtest
Heti tuli virheitä vastaan. DocumentRoot oli kirjoitettu väärin, polku osoitti käyttäjälle linuxkurssi, vaikka oikea käyttäjä oli linuxuser. Apache ei käynnisty, jos DocumentRoot ei ole olemassa, joten korjasinpolun.
Toinen virhe oli kirjoitusvirhe CustoLog -> CustomLog.
Apache ei hyväksy tuntemattomia direktiivejä, joten tämäkin kaatoi palvelimen.
Kun nämä korjattiin. Apache käynnistyi normaalisti.


<img width="875" height="187" alt="Näyttökuva 2026-09-21 104648" src="https://github.com/user-attachments/assets/b54b5516-f264-4b94-859e-421c513849a0" />


## HTTPS-yhteyden käyttöönotto
Yksi harjoituksen keskeisimmistä vaiheista oli HTTP->HTTPS -ohjauksen toteuttaminen. Sen tarkoitus on varmistaa, että käyttäjä ei koskaan jää salaamattoman HTTP-yhteyden varaan, vaan ohjautuu automaattisesti turvalliseen HTTPS-versioon. 
HTTP-liikenne kulkee selkokielisenä ja kuka tahansa verkon välissä voi lukea tai muuttaa sitä. HTTPS taas suojaa liikenteen TLS-salauksella.

Aloitin tutkimalla portti 80 VirtualHoast-tiedostoa . HTTP toimi, mutta se ei ohjannut automaattisesti HTTPS-versioon.
Tämä näkyi myös curl-testissä : curl -v https://tls-test000.linuxkurssi.xyz palautti tavallisen HTTP-vastauksen ilman redirect-headeria. Tehtävänannon mukaan ohjaus piti toteuttaa itse, joten lisäsin VirtualHostiin RewriteEngine-säännöt, jotka tarkistavat domainin ja ohjaavat kaiken liikenteen HTTPS-versioon.

<img width="817" height="507" alt="sudo nano VirtualHoast 80" src="https://github.com/user-attachments/assets/7d6d9a72-3797-428d-b094-072ee3336e29" />


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







