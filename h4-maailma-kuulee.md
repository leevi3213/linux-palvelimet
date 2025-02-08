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

## b) Alkutoimet

## c) Palvelimen asennus

## d) Vapaaehtoinen: Name Based Virtual Host

## Lähteet

- Susanna Lehto, 2022: Teoriasta käytäntöön pilvipalvelimen avulla
  - https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/
- Tero Karvinen, 2017: First Steps on a New Virtual Private Server
  - https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/
