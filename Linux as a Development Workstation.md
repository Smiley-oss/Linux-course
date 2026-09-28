# 1. Johdanto
Rakensin Debian‑pohjaisen VirtualBox‑kehitystyöaseman kurssin Module 6 ‑tehtävää varten. Tavoitteena oli luoda ympäristö, jossa voin tehdä DevOps‑harjoituksia, konttiteknologioita, versionhallintaa ja Infrastructure‑as‑Code‑työskentelyä. Asensin ja konfiguroin kaikki työkalut itse, ja korjasin virheet matkan varrella. Tämä raportti kuvaa työaseman, virheet, korjaukset ja opit.

(Lisää tähän kuvakaappaus VirtualBox‑VM:stä)

# 2. Git & GitHub – Versionhallinta ammattilaisen tavalla
## 2.1 GitHub‑asetukset
Asetin GitHubissa sähköpostin yksityiseksi ja otin käyttöön noreply‑osoitteen. Tämä estää oikean sähköpostin näkymisen commit‑historiassa.

❌ Virhe: GitHub näytti oikean sähköpostini
Aluksi commit‑historia paljasti oikean sähköpostini.

✔ Korjaus
Vaihdoin GitHubissa asetuksen “Keep my email address private”.

Päivitin Git‑asetukset:

<img width="822" height="447" alt="git asennus" src="https://github.com/user-attachments/assets/9f47beb5-9a40-440e-804f-0c814e14f564" />

* Oppi
GitHubin yksityisyysasetukset vaikuttavat suoraan Git‑identiteettiin.
Noreply‑osoite on pakollinen, jos haluaa suojata oman sähköpostin.


## 2.2 SSH‑avaimet ja Git‑konfiguraatio
Loin GitHubia varten erillisen SSH‑avaimen:

Koodi
ssh-keygen -t ed25519 -f ~/.ssh/github_key
Konfiguroin SSH‑yhteyden:

Koodi
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_key
Virhe: Käytin väärää SSH‑avainta
Oletusavain ei toiminut GitHubiin → permission denied.

Korjaus:
Loin uuden avaimen ja lisäsin sen GitHubiin.

Oppi:
GitHub‑yhteyksissä kannattaa käyttää erillistä avainta, ei oletusavainta.
SSH‑config tekee työskentelystä nopeampaa ja luotettavampaa.

<img width="805" height="462" alt="configurin git hub" src="https://github.com/user-attachments/assets/d65ade79-3059-4660-a67c-3010b328f619" />


## 2.3 Testirepo
Kloonasin kurssin testirepon SSH:llä:

Koodi
git clone git@github.com:linuxkurssi/git-testing.git
Lisäsin oman Linux‑vinkin, commitoin ja puskin sen GitHubiin.
ismail_tip.txt

3. Docker & Docker Compose – Konttialusta kehitystyöhön
3.1 Dockerin asennus ja testaus
Asensin Dockerin virallisilla ohjeilla ja testasin:

Koodi
sudo docker run hello-world
Ajoin useita imageja: nginx, python, mysql, ubuntu, vscode‑test.

(Lisää tähän kuvakaappaus docker ps ‑listasta)

3.2 Virheet ja korjaukset
❌ Virhe: Docker ei pystynyt poistamaan hello-world imagea
Virhe:

Koodi
unable to delete hello-world:latest (must be forced)
✔ Korjaus
Poistin kontit:

Koodi
docker rm -f $(docker ps -aq)
docker rmi -f hello-world
⭐ Oppi
Docker ei voi poistaa imagea, jos kontti käyttää sitä.
Konttien siivous on tärkeä osa DevOps‑työskentelyä.

❌ Virhe: Portti 8080 oli varattu
Terraform antoi virheen:

Koodi
Bind for 0.0.0.0:8080 failed: port is already allocated
✔ Korjaus
Poistin kaikki kontit.

Vaihdoin Terraformissa portin 8080 → 9090.

⭐ Oppi
Porttikonfliktit ovat yleisiä Dockerissa.
Opin hallitsemaan portteja ja tarkistamaan konttien tilan ennen Terraform‑apply‑komentoa.

(Lisää tähän kuvakaappaus porttivirheestä)

3.3 Docker Compose
Rakensin monikonttiympäristön:

Python‑web‑palvelu (5000)

MySQL‑tietokanta (3306)

Compose‑verkko ja volyymi

4. Terraform – Infrastructure as Code (IaC)
4.1 Terraformin asennus ja Docker‑provider
Asensin Terraformin ja rakensin Docker‑infrastruktuurin Terraformilla:

Docker‑provider

Nginx‑kontti portissa 9090

Docker‑verkko

Docker‑volyymi

(Lisää tähän kuvakaappaus Terraform‑apply‑komennosta)

4.2 Virheet ja korjaukset
❌ Virhe: Terraform ei löytänyt konfiguraatiotiedostoja
Virhe:

Koodi
No configuration files
✔ Korjaus
Olin väärässä hakemistossa → siirryin oikeaan:

Koodi
cd ~/terraform-docker
⭐ Oppi
Terraform toimii vain hakemistossa, jossa main.tf sijaitsee.

❌ Virhe: Docker‑providerin resurssit olivat ristiriidassa
Terraform yritti tuhota imagea, jota kontti käytti.

✔ Korjaus
Poistin kontit ennen Terraform‑apply‑komentoa.

⭐ Oppi
Terraformin state pitää resurssit synkronissa.
Opin hallitsemaan Docker‑resursseja Terraformin kautta.

5. Kehitystyökalut – Ammattilaisen työkalupakki
Asensin työasemaan seuraavat työkalut:

htop – prosessien seuranta

tmux – terminaalipaneelit ja sessiot

neovim – moderni editori

kubectl – Kubernetes‑hallinta

Go – DevOps‑ohjelmointikieli

Ansible – automaatio ja konfiguraatiohallinta

terraform-docs – Terraform‑moduulien dokumentointi

(Lisää tähän kuvakaappaus tmux‑paneeleista tai neovimista)

6. Optimoinnit ja konfiguraatiot
SSH‑avainten hallinta

Dockerin ja Compose‑pluginin asennus

Terraformin provider‑konfiguraatiot

Porttien hallinta (8080 → 9090)

Konttien siivous automatisoidusti

VS Code ‑laajennusten optimointi

VirtualBoxin verkkoasetusten säätö

7. Mitä opin kokonaisuudesta
⭐ Opin hallitsemaan:
GitHub SSH‑avaimet

Docker‑konttien elinkaari

Compose‑stackit

Terraformin state‑hallinta

DevOps‑työkalujen asennus

Virheiden analysointi ja korjaaminen

⭐ Opin myös:
Että virheet ovat osa oppimista

Että porttien hallinta on kriittistä

Että IaC vaatii tarkkuutta hakemistorakenteissa

Että tmux ja neovim nopeuttavat työskentelyä merkittävästi

8. Yhteenveto
Rakensin modernin Linux‑kehitystyöaseman, joka sisältää:

Git + GitHub SSH

Docker + Compose

Terraform + terraform-docs

VS Code + Neovim

Python

htop

tmux

kubectl

Go

Ansible

Työasema tukee ohjelmistokehitystä, konttiteknologioita, IaC‑työskentelyä ja pilvipalveluja.
Se on valmis jatkamaan kurssin seuraavia osioita (Azure, moduulit, arkkitehtuuri, state).
