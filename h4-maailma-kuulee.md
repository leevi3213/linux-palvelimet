# h4 Maailma Kuulee

## x) Lue ja Tiivistä
### [Teoriasta käytäntöön pilvipalvelimien avulla](https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/)
a) Pilvipalvelimen vuokraus ja asennus
- Hyödynnetään GitHub Education packia, joka mahdollistaa ilmaiset palvelin- ja domain-resurssit kurssin ajaksi.
- Rekisteröidään DigitalOceaniin, jossa henkilöllisyys varmistetaan pienen maksun (esim. 1 dollarin varmistusmaksu) kautta.
- Luodaan uusi Droplet käyttäen Debian 11 x64 -järjestelmää. Valitaan edullinen peruspaketti (1 GB RAM, 25 GB SSD, 1000 GB data) ja EU:n datakeskus (esim. Amsterdam) suorituskyvyn sekä GDPR-vaatimusten vuoksi.
- Rekisteröidään domain Namecheapissä. DNS-asetuksissa poistetaan vanhat A- ja CNAME-tiedostot ja lisätään uusi A-tiedosto, joka ohjaa domainin DigitalOceanin palvelimen IP-osoitteeseen.

d) Palvelimen suojaus palomuurilla
- Kirjaudutaan palvelimelle SSH:lla (esim. `ssh root@<IP-osoite>`).
- Haetaan pakettitiedot komennolla `sudo apt-get update`.
- Asennetaan UFW-palomuuri komennolla `sudo apt-get install ufw`.
- Avataan SSH-portti (22/tcp) komennolla `sudo ufw allow 22/tcp`.
- Otetaan palomuuri käyttöön komennolla `sudo ufw enable`.

e) Kotisivut palvelimelle
- Luodaan uusi käyttäjä (esim. "suska") ja annetaan käyttäjälle sudo-oikeudet.
- Asennetaan Apache-webpalvelin ja avataan HTTP-portti (80/tcp) palomuurissa.
- Korvataan Apachen oletustestisivu komennolla `echo Hello world! | sudo tee /var/www/html/index.html`.
- Otetaan Apache:n userdir-moduuli käyttöön ja luodaan käyttäjän kotisivut kansioon `public_html` hänen kotihakemistossaan.
- Muokataan ja tallennetaan käyttäjän oma index.html-tiedosto, jonka toimivuus varmistetaan selaimella.

f) Palvelimen ohjelmien päivitys
- Kirjaudutaan palvelimelle pääkäyttäjänä SSH:n kautta.
- Suoritetaan päivitykset komennolla `sudo apt-get update` ja `sudo apt-get upgrade`.
- Suoritetaan jakelupäivitys komennolla `sudo apt-get dist-upgrade` varmistaen, että kaikki ohjelmistot ovat ajan tasalla.

### [First Steps on a New Virtual Private Server](https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/)

- Luo DigitalOcean-tili, lisää maksutiedot/promo-koodi sekä luo uusi Ubuntu(/Debian) -pohjainen virtuaalipalvelin.
- Tarkista palvelimen IP-osoite ja kirjaudu ensimmäisellä kerralla sisään root-käyttäjänä (esim. `ssh root@<IP-osoite>`) ja aseta uusi, hyvä salasana.
- Avaa SSH-yhteyden portti palomuurissa (`sudo ufw allow 22/tcp`) ja ota UFW käyttöön (`sudo ufw enable`).
- Luo uusi käyttäjä (esim. "tero"), lisää sudo-, adm- ja admin-oikeudet ja testaa käyttäjätilin toimivuus ennen root-käyttäjän lukitsemista.
- Lukitse root-käyttäjän salasana (komennolla `sudo usermod --lock root`) ja muokkaa SSH-asetuksia niin, että root-kirjautuminen ei ole enää sallittua.
- Suorita ohjelmistojen päivitykset komennoilla `sudo apt-get update` ja `sudo apt-get upgrade`.
- Asenna julkinen palvelin (esim. Apache) ja muista avata HTTP-portti (`sudo ufw allow 80/tcp`).
- Määritä DNS-nimi NameCheapissä, jotta palvelin saavutetaan nimen kautta.

## a) Palvelimen vuokraus
Alkuperäinen suunnitelma oli käyttää DigitalOceanin ilmaisia kredittejä GitHub Education paketin kautta, mutta DigitalOceanin rekisteröitymisvaiheessa ilmeni kryptisiä ongelmia (tarkemmin ottaen maksutavan lisäämisessä). Selailin GitHub Education paketin muita vaihtoehtoja ja Microsoft Azure vaikutti olevan ainoa joka tarjosi myös ilmaisia kredittejä serverin hostaamiseen, joten päädyin kokeilemaan sitä.

![image](https://github.com/user-attachments/assets/bc5c4794-2dce-42c9-b09f-9509183b024d)

"Azure for Students" paketin käyttöönotto oli suhteellisen suoraviivainen. Seurasin linkkiä GitHub Student Developer Pack sivulla, joka johti Microsoftin sivuille:

![image](https://github.com/user-attachments/assets/9a07ce3a-2365-4a9b-a7cf-2febe65b9086)

Painoin "Start Free", jonka jälkeen ohjauduin sivulle jossa pyydettiin kirjautumaan yhdistämällä kouluni sähköposti ja kirjaamaan muutama muu kenttä, kuten koulu, osoite, jne. Maksutapaa ei tarvinnut. Kun kirjautuminen oli vahvistettu, ohjauduin suoraan Azure kotisivulle:

![image](https://github.com/user-attachments/assets/19458fdf-5e79-47a6-bf1a-287e57a75abf)

Seuraavaksi surffasin "Free Services" osioon, josta löytyi kohta "Linux Virtual Machine":

![image](https://github.com/user-attachments/assets/3f1c95e0-c50e-42cf-91e0-b3ebc1d90dc7)

Aloitin virtuaalikoneen luomisen. Tässä vaiheessa piti tehdä muutamia oleellisia valintoja, kuten distron ja speksien valinta, sekä valita virtuaalikoneen sijainti. Valitsin lähellä olevan serverin, kohtuullisilla ilmaisilla spekseillä (vaihtoehtoja oli vain kaksi, joko yksi tai kaksi ydintä, molemmat 1GB RAM muistilla).

![image](https://github.com/user-attachments/assets/513c481d-eb2c-45d7-8674-43b088038927)

Seuraavaksi piti konfiguroida autentikointitapa. Muistaakseni tunnilla käydyssä esimerkissä luotiin ssh-avain, jota käytettiin virtuaalikonetta luodessa. Siispä tein myös niin. Ensin generoin omalla virtuaalikoneellani (ei vielä Azure koneella) ssh-avamen. Tarkistin ensin myös että openssh oli asennettuna.

![image](https://github.com/user-attachments/assets/870274f9-f9a3-47e8-87ea-ffd7c499e231)

Kävin kopioimassa julkisen avaimen ja lisäämässä sen konfiguraatioon:

![image](https://github.com/user-attachments/assets/4644d1dd-592d-4c33-9f0f-780e065cecc6)

Lopuksi oli vielä portteihin liittyvä asetus, jonka kohdalla en ollut täysin varma sopivimman vaihtoehdon suhteen. Käytin kuvassa olevaa oletusta:

![image](https://github.com/user-attachments/assets/0dbacd8d-e832-4956-95e1-3e83775718f4)

Seuraavaksi painoin Review + Create, ja virtuaalikoneen luominen onnistui

![image](https://github.com/user-attachments/assets/81489ef6-aa88-44c6-8784-18d27327d959)

Virtuaalikoneen resurssi-sivulta sai IP osoitteen kopioitua, jonka jälkeen kokeilin yhdistää siihen omalta koneelta. Ajattelin ensin yhdistää roottina kuten esimerkeissä, mutta se näytti olevan valmiiksi lukittu. Yhdistäytyminen onnistui kuitenkin käyttäjänä:

![image](https://github.com/user-attachments/assets/d0bf9451-7140-4217-a540-b59a5eaaaaa7)


## b) Alkutoimet

Seuraavaksi tein muutamat alkutoimet. Root käyttäjä olikin jo valmiiksi lukittu, joka ilmeni siitä, etten pystynyt yhdistämään siihen ssh:lla. Pystyin tarkistamaan tämän vielä komennolla:

    $ sudo passwd --status root

![image](https://github.com/user-attachments/assets/982724d0-9077-4ec0-93ef-89bc52c03e47)

Tulosteessa "L" tarkoittaa että salasana on lukittu.

"leevi" käyttäjä kuului myös valmiiksi sudo ryhmään, joten jatkoin palomuurin asentamisella ja ohjelmien päivittämisellä. Ajoin seuraavat komennot:

    $ sudo apt install ufw
    $ sudo ufw enable 22/tcp
    $ sudo ufw enable 80/tcp
    $ sudo ufw enable

- `ufw enable` komennoilla tein tarvittavat reiät palomuuriin

Tämän jälkeen päivitin ohjelmat ja latailin pari elämänlaatua parantavaa ohjelmaa mm. editorin.

    $ sudo apt update && sudo apt upgrade
    $ sudo apt install vim bash-completion htop

## c) Palvelimen asennus

Apachen asennus ja default sivun korvaaminen seuraavaksi. Apachen asennus, käynnistys ja default sivun korvaaminen onnistui suoraviivaisesti seuraavilla komennoilla:

    $ sudo apt install apache2
    $ echo "Hello World" | sudo tee /var/www/html/index.html

- korvataan perussivun sisältö tekstillä "Hello World"
Tämän jälkeen käynnistin Apachen uudestaan ja katsoin toimiiko ensin lokaalisti `curl` komennolla:

![image](https://github.com/user-attachments/assets/2337ef43-8120-4b16-8c0d-2b1dda21662d)

Apache siis toimii, mutta tässä vaiheessa ilmeni pieni ongelma. En pystynyt yhdistämään serveriin selaimella toiselta koneelta koneen IP osoitteella, vaikka olin jo avannut tulimuurin reiät ja serveri pyöri. Pienen mietintätuokion jälkeen ongelma onneksi ratkesi. Azuren sivuilta piti käydä myös antamassa lupa ulkopuolisille yhdistää serveriin 80 portin kautta. 

![image](https://github.com/user-attachments/assets/97bf6ef2-fcd1-48b6-85b6-e07ca1084a78)

- Oman virtuaalikoneen "Networking" osion alta löytyvät yllä olevat asetukset. Sivulla täytyy painaa "Create port rule" ja lisätä TCP portti 80.

Tämän jälkeen kun Apachen käynnisti vielä kerran uudelleen, näkyi Hello World!

![image](https://github.com/user-attachments/assets/018393e7-e5f2-4c62-baf4-f604b552a0ab)

## d) Vapaaehtoinen: Name Based Virtual Host

Tein vielä päätteeksi Name Based Virtual Hostin seuraten omia ohjeita viime läksyistä. Tuloksena html-sivu käyttäjän kotihakemistossa, jonka kautta voin muokata sivua. Lisäsin vähän sivun sisältöä.

![image](https://github.com/user-attachments/assets/da75fb7a-d9a1-43de-bc43-fa9347317ee9)


![image](https://github.com/user-attachments/assets/8f19441b-6399-4e49-aa55-22c377a785a3)


## Lähteet

- Susanna Lehto, 2022: Teoriasta käytäntöön pilvipalvelimen avulla
  - https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/
- Tero Karvinen, 2017: First Steps on a New Virtual Private Server
  - https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/
- Tero Karvinen, 2025: Maailma Kuulee, Vinkit
  - https://terokarvinen.com/linux-palvelimet/#h4-maailma-kuulee
