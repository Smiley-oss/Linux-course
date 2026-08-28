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

<img width="1637" height="797" alt="Näyttökuva 2026-08-29 002216" src="https://github.com/user-attachments/assets/50eeb12a-ff0c-424f-8d64-03c0f84603a7" />
<img width="840" height="742" alt="Näyttökuva 2026-08-29 002104" src="https://github.com/user-attachments/assets/0717e8f2-d33e-48b9-903c-8699195515da" />
<img width="1756" height="775" alt="Näyttökuva 2026-08-29 002655" src="https://github.com/user-attachments/assets/9a5d2746-c27a-44f7-832f-4b82a8b8fc87" />
<img width="1681" height="793" alt="Näyttökuva 2026-08-29 002607" src="https://github.com/user-attachments/assets/a1717924-500a-4372-822d-a42a0f462119" />
<img width="1187" height="905" alt="Näyttökuva 2026-08-29 002425" src="https://github.com/user-attachments/assets/b8a96c8f-aec6-4a29-9a81-1af053cb5416" />
<img width="1111" height="907" alt="Näyttökuva 2026-08-29 002331" src="https://github.com/user-attachments/assets/90d04bed-460a-4f37-8ed4-a00c97dd6ac0" />
<img width="950" height="857" alt="Näyttökuva 2026-08-29 002248" src="https://github.com/user-attachments/assets/b449c41d-5049-409b-be00-838115b7cb7b" />


