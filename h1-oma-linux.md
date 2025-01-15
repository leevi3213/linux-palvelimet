# h1 Oma Linux

__x)__ 
### [Raportin kirjoittaminen](https://terokarvinen.com/2006/raportin-kirjoittaminen-4/)
- Kerrotaan täsmällisesti mitä tehtiin, ja mitä sitten tapahtui.
- Raporttia kannattaa kirjoittaa samalla kun työstää tehtävää.
- Raportin kuuluu olla:
  - Toistettava
    - Esim. työympäristön kuvailu kuuluu raporttiin.
  - Täsmällinen
    - Esim. kellonajat, viat ja onnistumiset kuuluu raportoida.
  - Helppolukuinen
    - Selkeät väliotsikot.
    - Huolellisesti kirjoitettu.
- Viittaa lähteisiin oikein.
- Älä valehtele, vain tehtyä työtä saa raportoida.
- Älä plagioi, koskee muiden tuotoksia mukaanlukien kuvien kopiointia.

### [FSF Free Software Definition](https://www.gnu.org/philosophy/free-sw.html)
- Vapaalla ohelmistolla tarkoitetaan ohjelmaa, joka kunnioittaa käyttäjien vapautta ja yhteisöllisyyttä.
- Käytännössä tarkoittaa että käyttäjillä on vapaus ajaa, kopioida, jakaa, muokata ja parantaa kyseistä ohjelmistoa.
- Englannin kielinen ilmaisu "free" viittaa nimenomaan vapauteen.
  - "Free as in _freedom_" vs. "Free as in _free beer_"
- Vapauden neljä pilaria:
  - Vapaus käyttää ohjelmaa haluamallaan tavalla.
  - Vapaus opiskella miten ohjelma toimii, ja muokata sitä haluamakseen. Ohjelmiston lähdekoodin täytyy siis olla avoin.
  - Vapaus jakaa kopioita ohjelmasta eteenpäin.
  - Vapaus jakaa kopioita muokatusta versiosta ohjelmasta.
- Vapaiden ohjelmistojen tavoitteena on mm. yhteisöllinen hyvä.


__a)__
## Debian-virtuaalikoneen asennus raportti

Asennus tehty onnistuneesti 15.1.2025 klo. 19-20 seuraavanlaisella työympäristöllä:
  - Host OS: Arch Linux
  - Prosessori: AMD Ryzen 7 5700x (amd64 arkkitehtuuri)
  - Pöytäkone, ethernet yhteys.

[19:10] 

Aloitin työn lataamalla Debian live ISO:n osoitteesta https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/. Valitsin tiedoston "debian-live-12.9.0-amd64-xfce.iso". Lataus onnistui klikkaamalla tiedoston nimeä.

[19:15] 

Seuraavaksi asensin virtuaalikone alustan, johon valitsin VirtualBox ohjelmiston. Koska tein asennuksen Arch Linux käyttöjärjestelmällä, seurasin VirtualBoxin asennusta varten ohjeita sivulta https://wiki.archlinux.org/title/VirtualBox. Asennus onnistui seuraavalla komennolla:

    $ sudo pacman -S virtualbox virtualbox-host-modules-arch
Kuten [Arch Wiki:n](https://wiki.archlinux.org/title/VirtualBox#Load_the_VirtualBox_kernel_modules) ohjeissa lukee, täytyi VirtualBoxin kerneli moduulit vielä käynnistää joko manuaalisesti tai boottaamalla kone uudelleen. Boottasin tässä vaiheessa koneen uudelleen eikä ongelmia ilmennyt.

[19:25]

Jatkoin käynnistämällä VirtualBox ohjelman. Klikkasin "New" painiketta avatakseen ikkunan, jossa voin nimetä uuden virtuaalikoneen ja valita sitä varten lataamani .iso tiedoston. VirtualBox tunnisti automaattisesti käyttöjärjestelmän tyypiksi "Linux" ja versioksi "Debian". Jatkoin painamalla nappeja seuraaville sivuille, joissa valitsin koneen speksit. Annoin koneelle 6GB RAM muistia, neljä prosessoria ja 20GB virtuaalista tallennustilaa. 

Painoin "Finish" ja lopputulos näytti seruaavalta:

![image](https://github.com/user-attachments/assets/1bd808f0-e677-4b14-af1c-38cb3a8fcd62)

[19:35]

Käynnistin virtuaalikoneen klikkaamalla nappia "Start". Tässä vaiheessa ilmeni pieni ongelma, sillä VirtualBox valitti ettei löytänyt .iso tiedostoa, jonka olin sille syöttänyt. Ongelma ratkesi kuitenkin helposti, sillä VirtualBox avasi ikkunan, josta pystyin syöttämään sille lataamani tiedoston uudestaan. Tämän jälkeen kone käynnistyi. Seuraavaksi aukesi valikko, josta valitsin "Live System" vaihtoehdon. Tämän jälkeen aukesi XFCE työpöytäympäristö.

Tilanne näytti tässä vaiheessa seuraavalta:

![image](https://github.com/user-attachments/assets/1c57a538-d29f-43e4-bfe1-1b990d400430)

[19:45]

Seuraavaksi avasin ohjelman "Install Debian", joka löytyi työpöydältä. Seurasin graafisia asennusohjeita; valitsin sopivan aikavyöhykkeen, kielen ja loin yhden käyttäjän ja valitsin sopivan salasanan sekä host-nimen. Osiointi vaiheessa valitsin "Erase disk". Panoin "Finish" ja asennus kesti noin 5-10min.

Lopputulos näytti seuraavalta:

![image](https://github.com/user-attachments/assets/4af0f2c5-839a-4286-8822-571580b778dd)


__k)__
## Bonus: Suosikkiohjelma Linuxilla

Tykkään ohjelmasta nimeltä "fortune", jonka saa ladattua varmaankin kaikille linux-distroille. Terminaalin kun kirjoittaa:

    $ fortune

Saa vastaukseksi satunnaisen hassun tekstin tai viisauden. Pari esimerkkiä:

![image](https://github.com/user-attachments/assets/2514a16b-11b7-4654-a8ed-f01bcd2b1301)


