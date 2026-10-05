# 1. Johdanto
Moduuli 7:n tavoitteena oli oppia Bash‑shellin toiminta,
.bashrc‑tiedoston muokkaaminen ja shell‑skriptien kirjoittaminen.
Tehtävissä tutkittiin Bashin asetuksia, lisättiin alias‑komentoja,
muokattiin komentohistoriaa ja toteutettiin oma shell‑skripti.
Tavoitteena oli ymmärtää, miten omaa Linux‑ympäristöä voi parantaa ja
miten skriptit automatisoivat toistuvia tehtäviä.

# 2. Toteutus ja tulokset
2.1 .bashrc‑tiedoston tutkiminen ja varmuuskopio
Tutkin ensin oman .bashrc‑tiedoston komennolla:

Koodi
nano ~/.bashrc
Ennen muutoksia otin varmuuskopion:

Koodi
cp ~/.bashrc ~/.bashrc.backup
Tämä varmistaa, että voin palauttaa alkuperäisen tiedoston,
jos muokkauksissa tulee virheitä.

<img width="940" height="896" alt="nano bashrc" src="https://github.com/user-attachments/assets/d0827913-8799-40df-98c3-32cbdf92db25" />

2.2 Welcome bannerin lisääminen
Lisäsin .bashrc‑tiedoston loppuun:

bash
echo "Wazuuup Homiee"
Tämä banneri näkyy aina kun avaan uuden terminaalin.
Banneri toimii sekä testinä että henkilökohtaisena tervehdyksenä.

<img width="792" height="817" alt="terminal2" src="https://github.com/user-attachments/assets/422ba14c-3fce-428f-94f7-51defd50795a" />


2.3 Aliasien lisääminen
Lisäsin kaksi aliasia, joita käytän usein:

bash
alias ll='ls -l'
alias gs='git status'
Perustelut:

ll nopeuttaa hakemiston sisällön tarkistamista

gs nopeuttaa GitHub‑raporttien tekemistä

aliasit vähentävät kirjoitusvirheitä ja nopeuttavat työskentelyä

<img width="892" height="897" alt="aliasin toimintz" src="https://github.com/user-attachments/assets/e8e02b2f-9f9b-43fe-850b-219558bc808e" />


2.4 HISTSIZE ja HISTFILESIZE
Muokkasin komentohistorian asetuksia:

bash
HISTSIZE=5000
HISTFILESIZE=10000
HISTCONTROL=ignoredups
Perustelut:

Bash muistaa enemmän komentoja → helpompi hakea vanhoja komentoja

ignoredups poistaa duplikaatit → historia pysyy siistinä

tämä parantaa tehokkuutta erityisesti pitkissä konfiguraatioissa

<img width="892" height="897" alt="komento history" src="https://github.com/user-attachments/assets/231cfb0b-16e9-4705-8109-d119864077e3" />


2.5 Haaste: Oma .bashrc ja perustelut
Tässä on oma, tehtävän mukainen .bashrc, joka sopii minun työskentelyyn:

bash
# Welcome banner
echo "Hello, Linuxuser"

# Aliases
alias ll='ls -l'
alias la='ls -la'
alias gs='git status'
alias ..='cd ..'

# History settings
HISTSIZE=5000
HISTFILESIZE=10000
HISTCONTROL=ignoredups

# Prompt
PS1="\u@\h:\w$ "

# Color support
alias ls='ls --color=auto'

# Add $HOME/bin to PATH if it exists
if [ -d "$HOME/bin" ]; then
    PATH="$PATH:$HOME/bin"
fi
Perustelut:
Banneri → testaa että .bashrc toimii

Aliasit → nopeuttavat työskentelyä

Historia → helpottaa komentojen hakua

Promptti → selkeä ja yksinkertainen

Värillinen ls → helpottaa tiedostojen erottamista

PATH‑lisäys → mahdollistaa omien skriptien ajamisen helposti

<img width="911" height="402" alt="kuinka monta komentoja bash muista" src="https://github.com/user-attachments/assets/f9983af1-2bfd-4eb2-ba19-63847d1aad13" />


2.6 Shell‑skripti
Valitsin tehtävän b) Haasteversion, koska se sisältää inputin, hakemistojen tarkistuksen ja loopin.

Skriptin sisältö
Koodi
#!/bin/bash

read -p "Directory name: " dirname

if [ -d "$dirname" ]; then
  echo "Directory already exists"
else
  mkdir "$dirname"
  for i in {1..5}; do
    echo "File $i" > "$dirname/file$i.txt"
  done
  echo "Created 5 files in $dirname"
fi
Mitä skripti tekee?
Kysyy hakemiston nimen

Tarkistaa, onko hakemisto olemassa

Jos hakemisto on olemassa → ilmoittaa siitä

Jos ei ole → luo hakemiston

Luo 5 tiedostoa loopilla

Tulostaa vahvistusviestin

<img width="890" height="680" alt="kysyy hakemistonimi" src="https://github.com/user-attachments/assets/793fa3e4-38ca-4809-94d2-497a717e11ec" />


<img width="822" height="540" alt="Näyttökuva 2026-10-05 222352" src="https://github.com/user-attachments/assets/1465f2c6-d2f3-47ee-bf93-f70b7c9e8cc8" />


# 3. Keskeiset havainnot ja pohdinta
Mitä opin?
Bashin käynnistyslogiikan

.bashrc‑tiedoston merkityksen

aliasien käytön

komentohistorian hallinnan

shell‑skriptien rakenteen (input, if‑else, loopit)

skriptien automatisoinnin hyödyt

Mikä oli haastavaa?
.bashrc‑tiedoston muokkaaminen vaatii varovaisuutta

historia-asetusten vaikutus näkyy vasta käytön myötä

skriptin logiikan suunnittelu ennen kirjoittamista

Mikä toimi helposti?
aliasien lisääminen

skriptin perusrakenne

loopin käyttö tiedostojen luomiseen

Miten osaaminen kehittyi?
ymmärrän nyt Bashin toimintaa syvemmin

osaan rakentaa oman tehokkaan työskentely-ympäristön

pystyn tekemään automaatioskriptejä itsenäisesti

# 4. Yhteenveto

Sain rakennettua toimivan .bashrc‑kokonaisuuden,
opin aliasien ja historian hallinnan ja
toteutin toimivan shell‑skriptin,
joka automatisoi hakemistojen ja tiedostojen luomisen.
Kokonaisuutena moduuli vahvisti Bash‑osaamistani ja paransi työskentelyä Linux‑ympäristössä.

5. Lähteet
DigitalOcean: Bashrc File in Linux

Haaga‑Helia raportointiohjeet

Linux man pages (bash, history, mkdir, nano)
