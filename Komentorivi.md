# Linux komentorivi

## 1 Johdanto

Harjoitusten tavoitteena oli vahvistaa Linuxin komentorivin peruskäyttöä hakemistorakenteiden luonti, tiedostojen käsittely, grep/wc/pipe-komentojen hallinta, GPL-2 lisenssin analysointi sekä btop-järjestelmämonitorin käyttö. Tavoitteena oli oppia tekemään nämä toiminnot sujuvasti ja ymmärtää, miten Linuxin työkalut tukevat järjestelmän hallintaa.


## 2 Toteutus ja tulokset

2.1 Hakemistorakenne ja tiedostot
Loin practice-hakemiston alikansioineen (docs, images, backups, archive). Muokkasin ja nimesin ne uudelleen (animals.txt, vegetables.txt), tein varmuuskopiot ja palautin ne. Tar-arkisto ja gzip-pakkaus onnistuivat sekä erikseen että yhdellä komennolla. Purin arkiston test-hakemistoon.

Keskeiset komennot: mkdir, touch, mv, cp, rm, tar-cvzf, tar-xvzf.

2.2 Grep, wc ja pipe
Testasin grep-komentojen perusoptiot (i, n, v), wc-komennot (l, w, c) sekä pipe rakenteet (grep, sort, unig). Pipe osoitti hyvin, miten komentoja voi ketjuttaa.

Virhe: wc -1 ei validi optio, korjattu wc -l

2.3 GPL-2 Lisenssi
Tutkin GPL-2 lisenssitiedostoa komentoilla wc -l, grep, grep -c, grep -i.
Löisin rivimäärän, GNU- sanan esiintymät ja lisenssi-sanan rivit.

GPL-2 tiivistelmä:
- sallii kopioinnin, muokkauksen ja jakamisen
- muokatut versiot jaettava samalla lisenssillä
- lähdekoodi oltava saatavilla
- ei takuuta
- ehtojen rikkominen päättää käyttöoikeuden

2.4 btop
Asensin btopin, tarkistin binäärin jakonfiguraatioiden sijainnit, muokkasin asetuksia ja testasin kuormitusta (ping -i 0.1..., yes > /dev/null).
Btop näytti selkeästi CPU-kuorman nousun, prosessilistan muutokset ja verkon aktiivisuuden.

2.5 Oma komentoriviohjelma - htop
Asennus itselleni hyödyllisen komentoriviohjelman, htop, joka tarjoaa visuaalisen prosessilistan ja reaaliaikaisen CPU/muisti seurannan.
sudo apt-get install htop
htop
Kokemus ja havainnot:
- Htopin käyttö oli suoraviivaista, selkeä värikoodaus ja reaaliaikainen prosessien päivitys.
- Verrattuna btopin, htop on kevyempi ja nopeampi avata.
- En tehnyt konfiguraatiomuutoksia, koska oletusasetukset toimivat hyvin.
- Htop auttoi hahmottamaan prosessien prioriteetteja ja CPU-kuormaa kuormitustestien aikana.
  ## 3 Keskeiset havainnot ja pohdinta
  - Opin hallitsemaan Linuxin peruskomentoja varmemmin ja ymmärsin grep/wc/pipi logiikan syvemmin.
  - Tar-komennon syntaksi vaati pientä tarkistusta, mutta muuten tehtävät sujuivat hyvin.
  - btopin konfiguraatiovirhe (Truuuuue) osoitti, miten tarkka ohjelma on syntaksille.
  - Htop vahvisti ymmärrystä prosessien prioriteeteista ja järjestelmän kuormituksesta.
  - Järjestelmän kuormituksen seuraaminen konkretisoi, miten prosessit vaikuttavat CPU- ja verkkometriikoihin.
  ## 4 Yhteenveto
  Harjoitusten tavoitteet saavutettiin. Rakensin hakemistorakenteen, käsittelin tiedostoja, tein varmuuskopioita, käytin grep/wc/pipi-komentoja, analysoin GPL-2 lisenssin ja monitoroin järjestelmää btopilla ja htopilla. Lopputuloksena syntyi selkeä ymmärrys Linuxin peruskomennoista ja järjestelmän toiminnasta.
## Lähteet
Heinonen, J.(2006). Linux Exercise - 2
https://github.com/johannaheinonen/johanna-test-repo/blob/main/module_2md
Aristocratos (2006). btop - Modern resource monitor for Linux.
https://github.com/aristocratos/btop


