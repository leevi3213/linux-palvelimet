# h3 Hello Web Server

__x) Tiivistelmät:__

### [Name Based Virtual Host Support](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)
- Name-based vs. IP-based
  - IP-pohjaiset vaativat jokaiselle hostille oman IP-osoitteen
  - Nimiin perustuvat käyttävät HTTP-headerissä olevaa nimeä, joka mahdollistaa monta hostia yhdelle IP:lle
  - Nimiin perustuva on ensisijainen, ellei joku laita vaadi IP-hostaamista
- Miten host valitaan
  - Virtual host valitaan ensin IP-osoitteen ja portin perusteella, jonka jälkeen tarkennetaan ServerName ja ServerAlias määrityksen avulla
  - Ensimmäinen listattu virtual host toimii oletuksena, jos tarkkaa vastausta ei löydy
- Käyttö:
  - Jokaisessa VirtualHost lohkossa on oltava vähintään ServerName ja DocumentRoot
  - ServerAlias mahdollistaa useiden nimien ohjaamisen samaan virtual hostiin
  - Konfiguraatiota voi säätää lisäämällä muita asetuksia VirtualHost lohkon sisään

### [Multiple Websites to Single IP](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/)
- Usein käytetään yhtä IP:tä useille domain nimille
- Ohjeet:
  - Asenna ja konfiguroi serveri
  - Lisää uusi Name Based Virtual Host
    - ServerName attribuutin lisäksi käytä ServerAlias attribuuttia toiselle nimelle samassa konfiguraatiossa
  - Luo html sivu
  - Testaa molempia

        $ curl -H 'Host: pyora.example.com' localhost
        $ curl localhost

  - Nimi palvelua voi simuloida lokaalisti /etc/hosts tiedoston avulla

        $ cat /etc/hosts
        127.0.0.1 localhost
        127.0.1.1 xubuntu
        127.0.0.1 pyora.example.com

__a) Toimiva serveri (localhost)__

![image](https://github.com/user-attachments/assets/b1bde780-9de5-4174-8910-fb1fc4bd4014)

__b) Oman sivun lokit__

Oman sivun lokien seuranta onnistui komennolla

    $ sudo tail -f /var/log/apache2/other_vhosts_access.log

ja tuloste kun sivun lataa näyttää seuraavalta:

![image](https://github.com/user-attachments/assets/ddcd1ade-b6ad-4ef7-b419-f76897a3486b)

Lokien yksi rivi koostuu rakenteesta
- virtuaalipalvelin, IP-osoite, päivämäärä ja aika, HTTP-pyyntö, HTTP-vastauskoodi, vastauksen koko, lähetteen URL, käyttäjäagentti

Tulosteesta saa selville esim.
- leevi.example.com:80
  - Pyyntö kohdistuu palvelimeen leevi.example.com, porttiin 80.
- 127.0.0.1
  - Pyyntö on tullut lokaalilta koneelta.
- GET / HTTP/1.1
  - GET pyyntö palvelimen juureen "/" käyttäen HTTP1.1 protokollaa.
- 200, 404, 304
  - HTTP vastauskoodeja
    - 200 onnistunut pyyntö, 404 sivua ei löytynyt, 304 not modified eli sisältö ei muuttunut joten palvelin ei lähetättänyt uutta versiota sivusta.
- 245, luku vastauskoodin jälkeen
  - Vastauksen koko tavuina.
- "-"
  - Lähetteen URL puttuu.
- curl/7.88.1
  - Käyttäjäagentti, tässä tapauksessa komentorivityökalu "curl" jolla pyyntö on tehty. Toisilla riveillä esim. "Mozilla" (Firefox selain).
