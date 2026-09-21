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



