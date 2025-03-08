# h5 Nimekäs

## a) Nimen vuokraus palvelimelle
Päätin valita nimipalveluksi Namecheapin, koska sen sai ilmaiseksi osana GitHub Student Packia:

![image](https://github.com/user-attachments/assets/b925f5a1-c840-4250-b362-4b357241225e)

Valitsin yllä näkyvästä kuvasta "Offer #1", jonka jälkeen seurasin linkkiä ja kirjauduin GitHub tunnuksilla Namecheapin sivuille. Seuraava näkymä näytti tältä:

![image](https://github.com/user-attachments/assets/4bc2f793-27af-4b85-88ea-6fef3b501eb7)

Tämän jälkeen kirjoitin kenttään haluamani domainin "leeviv.me", jonka jälkeen uudelleenohjauduin sivulle jossa pystyin "ostamaan" domainin:

![image](https://github.com/user-attachments/assets/c41d8ac0-0f96-4fee-b64d-5a78d2ca5851)

Painettua "Complete Order" uudelleenohjauduin sivulle, jossa sanottiin tilauksen onnistuneen.
Onnistuneen tilauksen jälkeen jouduin vielä erikseen kirjautumaan "namecheap.com" sivulle jotta näin oman tilini ja domainini, yllä olevat screenshotit ovat erilliseltä sivulta "nc.me", jonka kautta ilmainen domaini piti hankkia. 
Jouduin myös vahvistamaan vielä tilini sähköpostini kautta, jonka jälkeen pääsin kirjautumaan seuraavan näköiselle sivulle:

![image](https://github.com/user-attachments/assets/e64f7b0c-1a76-4198-93da-d3635ae8ab5b)

## b) Name Based Virtual Hosting uudella nimellä

Name based virtual hosting oli omalla serverilläni jo valmiiksi konfiguroitu aikaisemmista tehtävistä, mutta nimen ohjaaminen kyseiselle serverille vielä puuttui. Serveriä pääsi tässä vaiheessa katsomaan vain IP osoitteen kautta. 
Tässä vaiheessa en oikein muistanut mitä kuului tehdä, joten konsultoin Namecheapin dokumentaatiota. Etsin netistä "how to set up a namecheap domain", ja seurasin näitä ohjeita: 

https://www.namecheap.com/support/knowledgebase/article.aspx/9837/46/how-to-connect-a-domain-to-a-server-or-hosting/

Navigoin domainin "Advanced DNS" sivulle, jonne lisäsin ohjeiden mukaan serverin IP osoitteen tähän tapaan:

![image](https://github.com/user-attachments/assets/9ce2fab4-effb-44cd-9c76-a0614fd22047)

Dokumentaatiossa luki, että asetuksien vaihtamisen jälkeen voi kestää jonkin aikaa että muutoksia tapahtuu, joten asetuksien tallentamisen jälkeen odottelin ja päivittelin leeviv.me sivua hermostuneena, kunnes n. 10 min kuluttua näkyi serverin kotisivu!

![image](https://github.com/user-attachments/assets/87a0b206-661a-458f-8513-3ce978005637)

## c) Lisää sivuja

Seuvaavaksi kävin lisäämässä pari sivua serverille. Koska Name Based Virtual hosting oli jo pystyssä, ei tarvinnut tehdä muuta kuin lisätä muutama html tiedosto käyttäjän kotihakemistoon. Ensin täytyi tietenkin yhdistää serveriin SSH:lla, kuten aikaisemmissa tehtävissä. Tämän jälkeen loin kaksi html sivua lisää samaan hakemistoon, jossa käynnissä olevan serverin index.html oli. Sivut sai linkitettyä toisiinsa mm. `<nav>` html-elementin avulla.
Sivujen rakenne:

![image](https://github.com/user-attachments/assets/e778ba72-541d-4508-8971-bc5709bf3b80)

> Screenshotissa käytössä "tmux" ohjelma, jolla saa kätevästi terminaalin jaettua useampaan ikkunaan (molemmissa ikkunoissa auki tekstieditori)

Huomasin ettei serveriä tarvinnut edes käynnistää uudelleen, vaan uudet sivut näkyivät samantien:

![image](https://github.com/user-attachments/assets/287974d9-1fd8-43c7-90a1-3163b8004a03)

## d) Alidomain

Alidomainien lisäämistä varten suuntasin takaisin Namecheapin sivuille. Oman domainin alla osiossa "Advanced DNS" lisäsin 2 tietuetta:

![image](https://github.com/user-attachments/assets/a5431848-e765-4f9c-9f12-c0c8b9fad1ad)

![image](https://github.com/user-attachments/assets/ea0a51ff-f626-4b42-8a74-89f6d874c76b)

## e) DNS-tietojen tutkiminen

DNS-tietojen tutkimiseen tarvitsin komennot `host` ja `dig`. `host` oli valmiiksi asennettuna, mutta `dig` ei ollut. Haulla "debian dig command" sain nopeasti selville että ohjelman sai käyttöön asentamalla `dnsutils` paketin, joten asensin sen komennolla `sudo apt install dnsutils`.

![image](https://github.com/user-attachments/assets/47bcbd2c-b7a7-4387-8ec7-2cb2d4b7cb04)

Komennon `host` tuloste on suhteellisen suoraviivainen. Alidomainia tutkiessa komento tulostaa pelkästään serverin IP osoitteen, mutta päädomainia tutkiessa rivejä liittyen sähköpostiin. Ymmärtääkseni rivit kertovat mihin osoitteeseen serverille tuleva sähköposti uudelleenohjataan. 

![image](https://github.com/user-attachments/assets/e254f50f-844d-40ba-a6d0-2da59cc6ac57)

Komennon `dig` tuloste on kattavampi. `dig` komento tulostaa tietoa DNS-palvelusta, josta on hyötyä mm. toimivuuden testaamisessa. Alidomainia tutkiessa tuloste on melkein sama kuin päädomainin kohdalla, mutta muutamia eroja löytyy. Esimerkiksi HEADER ja "cookie" ID:t ovat erilaisia. Nämä vaihtuvat myös joka kerta kun komennon ajaa, eikä johdu vain eri domain nimistä. Tulosteesta näkee myös, että alidomainin on kooltaan hieman suurempi kuin päädomainin (tulosteen alaosassa "message size received"). Muuta tulosteesta nähtävää tietoa on esim. palvelimen IP-osoite, sekä DNS-palvelimen IP-osoite. Tulosteesta näkyy myös mm. milloin haku on tehty. 

![image](https://github.com/user-attachments/assets/3728184b-8111-4a50-a3ab-389c7557e6a0)

Käyttämällä `dig` komennon atribuuttia `any` domain nimen jälkeen, `dig` pyrkii hakemaan enemmän tietoa palvelimesta. Kuvassa tuloste wikipedia.org domainista. "Answer section" osiossa nähdään että wikipedialla on usea "NS" eli name server. "Additional section" osiossa nähdään myös näiden serverien IP osoitteet. Ylhäällä Headerissä näkyy myös tällä kertaa että vastauksia tuli 3 kpl.

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
- Tero Karvinen, 2025: Linux Palvelimet, h5 Nimekäs
  - https://terokarvinen.com/linux-palvelimet/#h5-nimekas
- Namecheap: How to Connect a Domain to a Server or Hosting
  - https://www.namecheap.com/support/knowledgebase/article.aspx/9837/46/how-to-connect-a-domain-to-a-server-or-hosting/
- Wikipedia: dig (command)
  - https://en.wikipedia.org/wiki/Dig_(command)
