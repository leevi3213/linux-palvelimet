# h7 Maalisuora

## a) Hello World kolmella eri kielellä
Päätin kirjoittaa hei maailma ohjelman seuraavilla kielillä: C, Python ja Go. Aloitin luomalla kansion kotihakemistooni nimeltä "helloworld" jonne loin ohjelmat. 

Päätin aloittaa C kielellä, joten loin tekstieditorilla seuraavanlaisen ohjelman tiedostoon nimeltä `helloworld.c`:

```c
#include <stdio.h>

int main(void)
{
  printf("hello world\n");
  return 0;
}
```

C ohjelmointikielessä tiedosto täytyy ensin "kääntää" ajettavaksi binääriksi ennen kuin sen voi ajaa, joten tässä kohtaa koitin ajaa komennon `gcc -o helloworld_c helloworld.c`. Huomasin kuitenkin ettei GCC kääntäjää (GNU Compiler) ollut asennettuna, joten asensin sen ensin komennolla `$ sudo apt install gcc`. Tämän jälkeen sain ohjelman käännettyä, ja sen jälkeen ajettua:

![image](https://github.com/user-attachments/assets/9abb712e-a242-46a4-92f8-de65af5ec366)

Seuraavaksi loin samaan kansioon Python ohjelman nimeltä `helloworld.py`, joka näytti tältä:

```python
print("hello world")
```

Koska Python oli valmiiksi asennettuna, eikä Python ole käännettävä kieli, ei tarvinnut muuta tehdä kuin ajaa tiedosto:

![image](https://github.com/user-attachments/assets/4a06fdcd-557d-4ff8-a129-003eccf2c206)

Seuraavaksi loin vastaavan ohjelman Go kielellä. Loin tiedoston samaan kansioon nimeltä `helloworld.go`:

```go
package main

import "fmt"

func main() {
  fmt.Println("hello world")
}
```

Go ohjelman ajamiseksi täytyi myös ensin asentaa paketti, joka sisältää tarvittavat välineet ohjelman ajamiseksi. Asensin siis ensin `golang` paketin komennolla `$ sudo apt install golang`. Tämän jälkeen ohjelman pystyi ajaa komennolla `$ go run helloworld.go`:

![image](https://github.com/user-attachments/assets/c8682ddd-5479-4e8b-8e01-eec161f75daf)

> Go on siitä joustava kieli, että ohjelmat voi joko ajaa suoraan kuten kuvassa, tai kääntää ensin ajettaviksi ohjelmiksi kuten C kielessä komennolla `go build`.

## b)

Kävin tarkistamassa lähdeviitteet aikaisemmissa tehtävissä. Tero Karvisen tehtävänanto sivu (https://terokarvinen.com/linux-palvelimet/) puuttui muutamasta, joten kävin sen lisäämässä. Muuten olen mielestäni pitänyt huolta lähteistä, eli se oli sillä siisti.

## c) Uusi komento

Aloitin uuden komennon luomisen tekemällä kotihakemistoon tiedoston nimeltä `infoplajays.sh` seuraavalla sisällöllä:

```
!#/usr/bin/bash

echo "user:    $(whoami)"
echo "host:    $(hostname)"
echo "uptime:  $(uptime -p)"
```
> Skripti käyttää komentoja `whoami`, `hostname` ja `uptime` antaakseen käyttäjälle infopläjäyksen. `uptime` komennon `-p` flagi formatoi tulosteen tiiviimmäksi. 

Seuraavaksi tein skriptistä ajettavan komennolla `chmod +x infoplajays.sh`, jonka jälkeen skriptin pystyi ajaa:

![image](https://github.com/user-attachments/assets/974632e4-14e6-4301-bcc6-5305c693c427)

Halusin vielä tehdä ohjelmasta ajettavan missä vaan, sekä poistaa .sh päätteen, joten ajoin seuraavat komennot:

```
$ mv infoplajays.sh infoplajays
$ sudo mv infoplajays /usr/local/bin/
```

Tämän jälkeen ohjelma oli ajettavissa mistä vaan:

![image](https://github.com/user-attachments/assets/87e094c4-d918-486e-b304-dd588671643f)

## d) Ratkaise vanha arvioitava laboratorioharjoitus

Valitsin harjoitukseksi vuoden 2023 labraharjoituksen Tero Karvisen sivuilta (https://terokarvinen.com/2023/linux-palvelimet-2023-arvioitava-laboratorioharjoitus/?fromSearch=laboratorio). Harjoituksessa on pari kohtaa, jotka joko sovellan tai ohitan, sillä niitä ei ole käyty tällä kurssilla (esim. Django kehitysympäristö-kohta). Harjoituksessa pyydetään myös luomaan markdown tiedosto virtuaalikoneeseen, johon raportti kirjoitetaan, mutta teen sen sijaan raportoinnin tälle github sivulle. 

Harjoituksen ohjeista:

> *"Huomaa: tässä ei tarvitse raportoida kaikkia askelia, vain testit että asiat toimivat, tai maininta, että tätä ei ole tehty. Tämä on siis paljon lyhempi raportti kuin kotitehtävässä."*

Lähtökohtaisesti raportoin siis toimivan lopputuloksen joka kohdasta, enkä jokaista ajamaani komentoa kuten aiemmin.

### d. 'hey'

>  - Tee kaikkien käyttäjien käyttöön komento 'hey'
>  - Tulosta haluamaasi ajankohtaista tietoa, esim päivämäärä, koneen osoite tms
>  - Pelkkä "hei maailma" ei riitä
>  - Komennon tulee toimia kaikilla käyttäjillä työhakemistosta riippumatta

Sovellan tässä ja käytän aikaisemmassa kohdassani tehtyä ohjelmaa, sillä se vastaakin jo harjoituksen vaatimuksia. Teen komennosta kaikille käyttäjille saavutettavan siirtämällä ohjelman kansioon `/usr/bin/`:

![image](https://github.com/user-attachments/assets/ffdcf6db-dc85-46c3-928f-40d8783515b9)

### e. 1000x nano

>  - Asenna micro-editori ja sille jokin plugin (siis micron oma lisäke).

Micro-editori `filemanager` pluginilla:

![image](https://github.com/user-attachments/assets/37302e0b-d410-4290-ad12-ffd0ef10d1a2)

### f. Staattisesti sinun

>  - Asenna Apache-weppipalvelin
>  - Tee järjestelmään käyttäjä Erkki Esimerkki tunnuksella "erkki"
>  - Tee Erkille kotisivu, joka näkyy osoitteessa http://localhost/~erkki/

Erkin kotisivut, jotka näkyvät osoitteessa localhost/erkki/, ja joita vain Erkki voi muokata (etusivu Erkin kotihakemistossa):

![image](https://github.com/user-attachments/assets/c649d58a-abaa-4ef9-be3c-6a2cae51ea5e)

### g. Salattua hallintaa

>  - Asenna ssh-palvelin
>  - Tee uusi käyttäjä omalla nimelläsi, esim. minä tekisin "Tero Karvinen test", login name: "terote01"
>  - Automatisoi ssh-kirjautuminen julkisen avaimen menetelmällä, niin että et tarvitse salasanoja, kun kirjaudut sisään. Voit käyttää kirjautumiseen localhost-osoitetta
>  - Vaihda SSH-palvelin kuuntelemaan porttiin 1337/tcp

SSH-yhteys Erkin käyttäjältä Leevin käyttäjälle portin 1337 kautta ilman salasanaa:

![image](https://github.com/user-attachments/assets/e3f3eade-ba1d-4533-9c10-2218db73517c)

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

- Karvinen Tero, 2025: Linux Palvelimet, h7 Maalisuora
  - https://terokarvinen.com/linux-palvelimet/#h7-maalisuora
- Karvinen Tero, 2005: SSH Public Key Authentication
  - https://terokarvinen.com/ssh_public_key_authentication.html?fromSearch=ssh
- Karvinen Tero, 2023: Final Lab for Linux Palvelimet 2023
  - https://terokarvinen.com/2023/linux-palvelimet-2023-arvioitava-laboratorioharjoitus/?fromSearch=laboratorio
- Gcore, 2024: How to change SSH port on Linux
  - https://gcore.com/learning/how-to-change-ssh-port/
