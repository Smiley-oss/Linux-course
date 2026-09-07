# 1 Johdanto
Tässä harjoituksessa mun tavoitteena oli laittaa pystyyn Apache2-web-palvelin Ubuntu-ympäristössä ja opetella hallitsemaan useampaa eri sivustoa samalla koneella nimitpohjaisten virtuaalipalvelinten (Name-based Virtual Host) avulla. Ajatuksena oli saada samasta IP-osoitteesta ja portista 80 auki eri sivustot eli localhost, site1.com ja site2.com.
Toinen tärkeä juttu tässä tehtävässä oli oppia pyörittämään nettisivujen tiedostoja suoraan omasta kotihakemistosta tavallisena käyttäjänä, jotta ei tarvitsisi joka välissä säätää pääkäyttäjän sudo-oikeuksilla. Lisäksi testailtiin palomuurin vaikutusta lokaaliliikenteeseen, seurattiin järjestelmän lokitiedostoja ja ratkottiin vastaan tulleita konfiguraatio- ja kirjoitusvirheitä.
<img width="952" height="447" alt="kuva1_Apache" src="https://github.com/user-attachments/assets/b67cee87-57b4-4c17-9c08-81c3e83274a5" />

# 2 Apachen asennus ja oletussivun muokkaus
Aloitin tehtävän asentamalla Apache2-palvelimen järjestelmään pakettienhallinnan kautta. Kun asennus oli valmis, testasin palvelimen toimintaa avaamalla osoitteen localhost sekä graafisella verkkoselaimella että suoraan päätteestä curl-komennolla. Mulla aukesi ruudulle Apachen normaali Ubuntu-oletussivu, mikä varmisti sen, että palvelinpyörähti kerralla käyntiin.
Seuraavaksi oli tarkoitus muokata tätä oletussivua ja korvata se omalla tekstillä. Tein tämän syöttämällä echo-komennon ja ohjaamalla sen suoraan pääkäyttäjän oikeuksilla toimivalle tee-komennolle osoitteeseen /var/www/html/index.html. Tämä oli myös tehtävänannon mukaan ainoa kohta, jossa nettisivun muokkaamiseen tarvittiin sudo-oikeuksia.

<img width="947" height="1020" alt="kuva2 Apache" src="https://github.com/user-attachments/assets/5babeb99-27df-473f-8a2a-573378dcf954" />


### Teoriakysymysten pohdintaa ja vastauksia:
####Mitä komennossa tapahtuu vaihe vaiheelta?
Ensin echo-komento tulostaa halutun tekstin puskuriin. Sen jälkeen pystyviiva eli putki (pipe) ottaa tämän tulosteen ja syöttää sen suoraan tee-komennolle. Koska tee-komennon edessä käytettiin sudoa, itse tiedostoon kirjoittaminen tapahtuu pääkäyttäjän oikeuksilla. Tämä on tarpeen, koska tavallisella käyttäjällä ei ole kirjoitusoikeutta /var/www/html/-hakemistoon.
#### Mitä muita tapoja olisi saavuttaa sama lopputulos?
Saman asian olisi voinut hoitaa avaamalla kyseisen tiedoston suoraan teksti-editorilla (kuten Nanolla) suoritettuna sudo- komennolla, tai avaamalla pääkäyttäjän subshellin bash-komennolla ja tekemällä tavallisen tulostuksen uudelleenohjauksen suoraan tiedostoon.
#### Miksi tavallinen sudo echo -viritelmä uudelleenohjauksella ei toimi?
Jos yrittää ajaa komennon muodossa "sudo echo teksti > tiedosto", komento epäonnistuu. Tämä johtuu siitä, että Linux-kuori tekee nimenomaan tuon nuoliputken eli uudelleenohjauksen nykyisen kirjautuneen käyttäjän oikeuksilla ennen kuin koko sudo-komento edes ehtii käynnistyä. Koska tavallisella käyttäjällä ei ole oikeutta kirjoittaa järjestelmähakemistoon, tulee ruudulle heti ilmoitus "Permission denied".
# 3 Nimenselvitys ja UFW-palomuurin testaus
Sitä varten, että saisin site1.com ja site2.com osoitteet toimimaan omalla koneellani ilman oikeita rekisteröityjä verkkotunnuksia tai ulkoisia DNS-palvelimia, kävin muokkaamassa järjestelmän omaa /etc/hosts -tiedostoa. Lisäsin sinne rivit, jotka ohjaavat kyseiset verkkotunnukset suoraan oman koneen silmukkaosoitteeseen 127.0.0.1.
Seuraavaksi asensin ja kytkin päälle UFW-palomuurin tutkiakseni, miten se vaikuttaa paikalliseen liikenteeseen. Suljin HTTP-liikenteen eli portin 80 palomuurista ja kokeilin ottaa yhteyttä localhostiin selaimella sekä curlilla.

<img width="957" height="1017" alt="kuva3 Apache" src="https://github.com/user-attachments/assets/99e7830a-038f-4117-877e-8f17b70c7674" />


Tästä testistä huomasin sen, että jos UFW-palomuuriin tekee yleisen estosäännön portille 80 ilman tarkempia rajauksia, se blokkaa myös koneen sisäisen loopback-liikenteen. Eli vaikka yhteys ei tule verkosta vaan koneelta itseltään osoitteeseen 127.0.0.1, palomuuri ottaa siihen kiinni ja estää sivun latautumisen. Testin jälkeen avasin portin 80 uudelleen palomuurista, jotta pääsin jatkamaan harjoitusta.
# 4 Ensimmäinen virtuaalipalvelin (site1.com) kotihakemistosta
Tehtävän ajatuksena oli kokeilla sivujen ajamista kotihakemistosta käsin. Luoin omalle käyttäjälleni oman hakemiston public_html/site1.com ja tein sinne yksinkertaisen index.html-tiedoston.
Tässä kohtaa piti olla tarkkana kansioiden oikeuksien kanssa: Apachen taustaprosessi eli www-data-käyttäjä tarvitsee lukualueen koko polulle kotihakemistoon asti. Asetin kotihakemistolle ja luomilleni kansioille lukuoikeudet kuntoon.
Sen jälkeen siirryin tekemään Apachen virtuaalipalvelimen konfiguraatiota. Luoin uuden tiedoston site1.com.conf Apachen sites-available -hakemistoon. Määritin sinne ServerName-arvoksi site1.com ja DocumentRoot-poluksi oman kotihakemistoni kansion. Lisäksi piti lisätä Directory-osio, jossa annettiin Apachelle lupa lukea kyseistä kotihakemiston kansiota (Require all granted).
Otin uuden sivuston käyttöön a2ensite-komennolla ja latasin Apachen asetukset uudelleen.

Kuva 3: 

# 5 Lokitiedostot ja tahallinen virheen aiheuttaminen
Päästäkseni näkemään miten Apache reagoi ongelmiin ja miltä se näyttää järjestelmässä, seurasin lokitiedostoja päätteessä tail- ja journalctl-komennoilla. Pidin auki sekä Apachen omaa error.log- ja access.log-tiedostoa että systemd:n journalctl-lokia.
Tein kokeilumielessä konfiguraatioon tahallisen virheen: muutin site1.com-tiedostossa DocumentRoot-polun osoittamaan sellaiseen kansioon, jota ei ollut olemassa.

Kuva 4: Virheilmoitusten ja lokimerkintöjen tarkastelu päätteessä.

Kun tämän jälkeen yritin hakea sivua curlilla, selain/pääte ilmoitti virheestä. Virhelokissa (error.log) näkyi heti punaisella ja tarkalla aikaleimalla varustettu ilmoitus siitä, että Apache ei löytänyt pyydettyä hakemistoa tai tiedostoa. Tämä oli todella hyödyllinen testi, sillä se osoitti miten nopeasti oikean virheen syy löytyy suoraan lokia lukemalla ilman arvailemista. Korjasin polun takaisin oikeaksi ja latasin palvelimen uudelleen.
# 6 Haasteosuus: Toinen virtuaalipalvelin (site2.com) ja sekaannusten ratkaisu
Haasteosuudessa tarkoituksena oli luoda toinen täysin erillinen virtuaalipalvelin eli site2.com toimimaan täysin samassa IP-osoitteessa. Luoin kotihakemistooni toisen kansion site2.com-sivustolle ja tein sinne oman index.html-tiedostoni eri sisällöllä.
Luoin uuden konfiguraatiotiedoston site2.com.conf ja kytkin sen päälle. Tässä kohtaa törmäsin kuitenkin käytännön ongelmaan: kun tein pyynnön osoitteeseen localhost, se palauttikin minulle site1.com-sivuston sisällön! Sivustot menivät siis tavallaan päällekkäin.
Tämä johtui siitä, että Apache tarjoaa aina oletuksena aakkosjärjestyksessä ensimmäisen löytämänsä virtuaalipalvelimen, jos pyydetylle osoitteelle (kuten localhost) ei ole määritelty omaa selkeää konfiguraatiota.

Kuva 5: Ongelmatilanne, jossa localhost ja site1 menivät päällekkäin, sekä Nanolla sattunut kirjoitusvirhe.
# Tämän vaiheen ongelmat ja niiden ratkaisut:
Sivustojen sekoittuminen:

Muokkasin Apachen oletuskonfiguraatiota ja varmistin, että jokaisella sivustolla (site1.com, site2.com sekä localhost) on omat selkeät konfiguraatiotiedostot ja jokaisessa on määritelty oikea ServerName-rivi.

Apachen AH00558 FQDN-varoitus:

Kun ajoin Apachen konfiguraatiotestin (apache2ctl configtest), ruudulle tuli keltainen varoitus siitä, ettei palvelimen globaalia ServerName-nimeä ole asetettu. Vaikka syntaksi oli muuten kunnossa, korjasin tämän tekemällä uuden servername.conf-tiedoston Apachen conf-available-hakemistoon ja kytkemällä sen a2enconf-komennolla päälle. Tämän jälkeen configtest antoi pelkkää puhdasta "Syntax OK" -ilmoitusta.

Kirjoitusvirhe Nanolla polkua avattaessa:

Eräässä vaiheessa Nano-editori valitti ruudun alalaidassa punaisella, että hakemistoa ei ole olemassa. Huomasin komennostani, että olin kirjoittanut kansion nimen väärin (apaeche2 ja site-available). Poistuin Nanolta ja kirjoitin komennon uudelleen oikealla polulla /etc/apache2/sites-available/.

Kun kaikki korjaukset oli tehty ja Apache ladattu uudelleen, testasin kaikki kolme osoitetta läpi curlin avulla.

Kuva 6: Lopullinen varmistus päätteessä – kaikki kolme osoitetta toimivat erillään.
# 7 Keskeiset havainnot ja pohdinta
Mitä opin teknisesti?

Opimpa kunnolla sen, miten nimitpohjainen virtuaalipalvelin toimii käytännössä. Ymmärsin, että vaikka kaikki pyynnöt tulevat samaan IP-osoitteeseen (127.0.0.1) ja samaan porttiin 80, Apache osaa lukea selaimen/curlin lähettämästä HTTP Host -otsakkeesta, mitä sivustoa käyttäjä hakee ja tarjoaa sen perusteella oikean kansion sisällön.

Mikä oli haastavaa ja mikä helppoa?

Haastavinta oli hahmottaa aluksi se, miksi localhost näytti site1.com-sivua ja miten Apache valitsee oletussivuston silloin kun täsmäävää nimeä ei löydy. Myös hakemistojen oikeuksien säätäminen kotihakemistosta käsin vaati huolellisuutta. Helppoa taas oli itse konfiguraatiotiedostojen kirjoittaminen ja sivustojen kytkeminen päälle Apachen omilla a2en-työkaluilla.
### Miten osaaminen kehittyi?
Terminaalissa työskentely ja virheiden vianmääritys paranivat selvästi. Tuli todella tutuksi lukematon määrä Apachen hallintakomentoja, ja oppi siihen ettei panikoi virheilmoituksista, vaan lukee ne lokista tai ruudulta ja korjaa polut tai asetukset niiden mukaan.
#8 Yhteenveto
Tehtävälle asetetut tavoitteet saavutettiin täydellisesti. Apache2-palvelin saatiin pyörimään ongelmattomasti, palomuuriasetusten vaikutus todettiin käytännössä ja järjestelmään saatiin määriteltyä kaksi eri kotihakemistosta pyörivää virtuaalipalvelinta (site1.com ja site2.com) sekä erillinen localhost. Kaikki matkan varrella tulleet konfiguraatio- ja syntaksivirheet saatiin korjattua, ja lopputulos testattiin toimivaksi.
# Lähteet
The Apache Software Foundation. Apache HTTP Server Documentation Version 2.4: Apache Name-Based Virtual Host Support. Saatavilla: https://httpd.apache.org/docs/2.4/vhosts/name-based.html

The Apache Software Foundation. Apache HTTP Server Documentation Version 2.4: VirtualHost Examples. Saatavilla: https://httpd.apache.org/docs/2.4/vhosts/examples.html

Canonical Ltd. / Ubuntu Documentation. Ubuntu Server Guide – Apache2 Web Server Configuration. Saatavilla: https://ubuntu.com/server/docs/web-servers-apache





