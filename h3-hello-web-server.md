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

    
