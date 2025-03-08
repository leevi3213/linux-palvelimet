# h6 Salataampa

## x) Lue ja tiivistä
### [Let's Encrypt: How it works](https://letsencrypt.org/how-it-works/)
- Let's Encryptin ja ACME (Automatic Certificate Management Environment) protokollan tavoitteena on mahdollistaa HTTPS serverin pystyttäminen ja automatisoida sertifikaattien hallinnointi
- Let's Encrypt tunnistaa serverin adminin julkisen avaimen avulla
- Agentti luo avainparin ja osoittaa hallinnan suorittamalla CA:n antamia haasteita, kuten:
  - DNS-tietueen lisääminen
  - HTTP-tiedoston sijoittaminen tiettyyn URI:in
- Kun avainpari on valtuutettu, agentti voi pyytää sertifikaattia tekemällä sertifikaattipyynnön

### [Lego: Obtain a Certificate](https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server)
- Sertifikaatin hankinta Lego-työkalulla:
  - `$ lego --email="you@example.com" --domains="example.com" --http run`
  - tallentuu .lego/certificates/ kansioon
- Käyttämällä DNS-palveluntarjoajaa:
  - `GANDI_API_KEY=xxx lego --email "you@example.com" --dns gandi --domains "example.org" --domains "*.example.org" run`
- Käyttämällä valmista Certificate Signing Requestia (CSR):
  - `lego --email="you@example.com" --http --csr="/path/to/csr.pem" run`
- Jos portti 80 on jo käytössä, käytä `--http.webroot` asetusta:
  - `lego --email you@example.com --http --http.webroot /path/to/webroot --domains example.com run`

### [Apache Documentation: SSL/TLS Strong Encryption](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample)
- Yksinkertainen esimerkki SSL-konfiguraatiosta:
```
LoadModule ssl_module modules/mod_ssl.so

Listen 443
<VirtualHost *:443>
    ServerName www.example.com
    SSLEngine on
    SSLCertificateFile "/path/to/www.example.com.cert"
    SSLCertificateKeyFile "/path/to/www.example.com.key"
</VirtualHost>
```

### a) Hanki ja Asenna TLS-sertifikaatti
Aloitin suuntaamalla Let's Encryptin sivuille "Get Started" osioon (https://letsencrypt.org/getting-started/), ja seurasin ohjeita. Sivulla kerrottiin asennuksen tapahtuvan Certbot ACME clientin kautta, joten suuntasin sivulla olevan linkin kautta Certbotin "Instructions" sivulle (https://certbot.eff.org/instructions?ws=apache&os=pip). Aloitin valitsemalla serveriksi Apache ja käyttöjärjestelmäksi Linux (pip paketinhallinta järjestelmällä, ei snap). Ajoin seuraavat komennot seuraten Certobot dokumentaation ohjeita: 

Riippuvuuksien asennus:

    $ sudo apt install python3 python3-venv libaugeas0

Python Virtuaaliympäristön asennus Certbotin asennusta varten:

    $ sudo python3 -m venv /opt/certbot/
    $ sudo /opt/certbot/bin/pip install --upgrade pip

Certbotin asennus ja linkitys jotta ohjelman voi ajaa:

    $ sudo /opt/certbot/bin/pip install certbot certbot-apache
    $ sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot

Seuraavaksi dokumentaatio antoi muutamia vaihtoehtoja, päätin valita vaihtoehdon joka pistää automaattisesti HTTPS sertifikaatin pystyyn ilman että Apache konfiguraatiota tarvitsee muokata:

    $ sudo certbot --apache

Tässä vaiheessa ilmeni kuitenkin ongelma. Certbot kyllä löysi päällä olevan serverin, mutta antoi seuraavan virheilmoituksen:

![image](https://github.com/user-attachments/assets/9a7c75f3-fb30-449b-bf31-bef906296649)

Hetken kestäneen mietintätuokion jälkeen havahduin siihen, että virheilmoituksessa oleva domaini ei ole sama domaini jolla sivun näkee netissä. Virheilmoituksen domaini on se, mikä löytyy Apache konfiguraatiosta. Piti siis tehdä pieni muutos konfiguraatioon, jossa vaihdoin ServerName attribuutin vastaamaan oikeaa domainia (tämä ei jostain syystä tuottanut aikaisemmin ongelmia, joka ehkä johtui siitä että tämä on ainoa saatavilla oleva serveri).

Muutin ServerName attribuutin Apache konfiguraatiossa `leevi.supersite.com` > `leeviv.me`, jonka jälkeen komennon ajaminen onnistui:

![image](https://github.com/user-attachments/assets/03e95c51-d87d-4ea1-bf3b-f347d38eb033)

Tämä jees, mutta nettisivu ei vielä toiminut, koska palomuurin portit oli vielä konfiguroimatta. Siispä ajoin seuraavan komennon avatakseen 443 portin:

    $ sudo ufw allow 443/tcp

Ja koska käytän Azuren virtuaalikone palvelua, piti käydä portti lisäämässä myös Azuren palvelujen kautta, mikä kävi ilmi jo yhdessä aikaisemmassa tehtävässä.

Siispä, Azuren sivuilla navigoin oman virtuaalikoneeni sivulle, sivupalkista "Networking" osion alaosioon "Network Settings", jossa lisäsin portin napista "Create Port Rule". Porttia lisätessä piti valita "Inbound Rule", ja kirjoittaa "Destination Port" kohtaan 443, jonka jälkeen voi painaa "Add". Lopputulos kyseisellä sivulla näytti seuraavalta: 

![image](https://github.com/user-attachments/assets/68d1cb18-b7ba-4152-b85a-394838d7005a)

Tämän jälkeen kun Apache serverin käynnisti vielä uudelleen (`sudo systemctl restart apache2`), näkyi sivu ja osoitteen vieressä HTTPS lukko!

![image](https://github.com/user-attachments/assets/fc81db06-57be-4cc0-b406-4a9475692091)

## b) A-rating: Testaa sivua

Ajoin SSL Labsin TLS testin (https://www.ssllabs.com/ssltest/), ja tulos oli ilmeisesti ihan tyydyttävä:

![image](https://github.com/user-attachments/assets/edc9d705-3e51-43b5-a458-9295ab85b043)

Testi antaa lisätietoa sertifikaatista, esim. milloin se menee umpeen ja onko avain heikko:

![image](https://github.com/user-attachments/assets/23eb39fa-9760-42cf-b085-bdb75def7a0a)
> Ainoa ei-vihreä kohta on DNS CAA, joka ymmärtääkseni liittyy siihen millä sertifikaattien jakajilla on oikeus myöntää tietylle domainille sertifikaatteja. Eli käytännössä sertifikaatteja voisi sallia vain tietyiltä tahoilta, esim. Let's Encrypt eikä mistään muualta.
> Lisää: https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization

Sivulla löytyy myös tietoa ajetun testin konfiguraatiosta, mm. käytetyistä protokollista ja minkälaisella pyynnöllä sivua testattiin:

![image](https://github.com/user-attachments/assets/b210b0ea-f717-46a5-af86-89730e639f90)
![image](https://github.com/user-attachments/assets/21f5a0f4-d3b2-4f68-8900-8fef6b5d5134)


## Työympäristö

__Host:__
- OS: Arch linux
- CPU: Ryzen 7 5700X
- GPU: RTX 3060 12GB
- RAM: 32GB
- Ethernet yhteys

__Virtuaalikone, jolla tehtävät on tehty:__
- Alusta: QEMU/KVM
- OS: Debian 12
- CPU: Ryzen 7 5700X (2 ydintä)
- RAM: 4GB

## Lähteet
- Tero Karvinen, 2025: Linux Palvelimet, h6 Salataampa
  - https://terokarvinen.com/linux-palvelimet/#h6-salataampa
- Let's Encrypt, 2024: How It Works
  - https://letsencrypt.org/how-it-works/
- Let's Encrypt, 2024: Get Started
  - https://letsencrypt.org/getting-started/
- Certbot Documentation: Instructions
  - https://certbot.eff.org/instructions?ws=apache&os=pip
- Lange, 2024: Lego: Obtain a Certificate
  - https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server
- The Apache Software Foundation, 2025: Documentation: SSL/TLS Strong Encryption
  - https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample
- Wikipedia: DNS Certification Authority Authorization
  - https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization
