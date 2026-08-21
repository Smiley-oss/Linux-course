
## Johdanto
Moduulin tavoitteena oli asentaa Linux‑käyttöjärjestelmä omalle koneelle ja luoda GitHub‑repo kurssin harjoitustehtävien raportointia varten. Lisäksi tehtävään kuului artikkelin “What is Open Source Software and Why Use OSS” tiivistelmä sekä pohdinta siitä, miten avoimen lähdekoodin ohjelmistot vaikuttavat nykyaikaiseen teknologiaan ja digitaaliseen infrastruktuuriin.

Halusin oppia käytännössä, miten Linux asennetaan, miten virtuaalikone toimii, miten GitHub‑repo luodaan ja miksi avoin lähdekoodi on niin keskeinen osa modernia IT‑maailmaa.

## 1 GitHub‑repositorion luominen
# Tavoite
Luoda oma GitHub‑repo, johon kurssin raportit tallennetaan Markdown‑muodossa.

# Vaiheet
- Kirjauduin GitHubiin omalla tililläni.

- Valitsin New Repository.

- Annoin repolle nimen (esim. linux-raportit-isse).

- Valitsin repositorion tyypiksi public (tai private, jos haluan rajata näkyvyyttä).

- Lisäsin README.md‑tiedoston.

- Testasin repoa tekemällä ensimmäisen commitin.

# Havainnot
- GitHubin käyttöliittymä on selkeä.

- Markdown on helppo tapa kirjoittaa siistejä raportteja.

- Repo toimii hyvänä portfoliona työnantajille.

## 2 Linuxin asennus omalle koneelle
Asensin Linuxin VirtualBox‑virtuaalikoneeseen kurssin ohjeiden mukaisesti. Käytin Debian 13.x ‑jakelua, koska se on vakaa ja kevyt.

Asennusympäristö
- VirtualBox 7.2.16.vbox-extpack

Debian 13.6.0-amd64-xfce.Iso

4 GB RAM, 2 CPU, 20 GB levytilaa

Vaihe 1: Virtuaalikoneen luonti
- Luo uusi VM → Linux → Debian (64‑bit)

- Määritä muisti ja levytila

- Liitä Debianin ISO‑tiedosto

Vaihe 2: Debianin asennus
Käynnistin VM:n ISO‑tiedostolla

- Valitsin Graphical Install

- Kieli: Finnish / Finnish classic

- Asensin peruspaketit ja järjestelmän

Vaihe 3: Ensimmäinen käynnistys
Suoritin päivitykset:

Koodi
- sudo apt update
- sudo apt upgrade
Asensin hyödyllisiä työkaluja:

# Mahdolliset virheilmoitukset ja ratkaisut
- Guest Additions ei toiminut heti:  
Ratkaisu: käytin kurssin workaround‑ohjetta ja asensin lisäosat manuaalisesti.

- Näytön resoluutio oli aluksi pieni:  
Ratkaisu: VirtualBox → Display → Scale → 125%.

Kuvakaappaukset (lisää omat GitHubiin/PDF:ään)
VirtualBox VM‑asetukset

Debianin asennusvaiheet

Ensimmäinen käynnistys

Päivityskomennot terminaalissa
