# h2 Komentaja Pingviini

__x)__
### [Command Line Basics](https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited)
- Navigointi
  
  - pwd
    - Printtaa tämänhetkisen hakemiston
  - ls
    - Printtaa tiedostot ja hakemistot tämänhetkisessä hakemistossa 
  - cd
    - Siirtää toiseen hakemistoon
  - less
    - Näyttää tekstitiedoston sisällön
- Tiedostojen manipulointi
  - nano foo.txt
    - Avaa tiedoston haluamassaan tekstieditorissa, esim. nano, micro, vim
  - mkdir
    - Luo uuden hakemiston
  - mv
    - Siirtää tiedoston tai hakemiston, tai nimeää sen uudelleen
  - cp
    - Kopioi tiedoston tai hakemiston
  - rmdir
    - Poistaa tyhjän hakemiston
  - rm
    - Poistaa tiedoston, huom: "rm -r" poistaa hakemiston ja sen sisällön, "rm -rf" poistaa kaikkien alihakemistojen sisällöt rekursiivisesti
- Apua
  - man
    - Näyttää komennon manuaali sivun, esim. "man ls"
- Tärkeät hakemistot
  - /
    - Juurihakemisto
  - /home
    - Kaikkien käyttäjien kotihakemistot, esim. "/home/tero"
  - /etc
    - Järjestelmän kaikki asetukset
  - /media
    - Poistettavat mediat esim. USB-tikut
  - /var/log
    - Järjestelmän logit
- Pakettien hallinta (apt)
  - sudo apt update
    - Päivittää saatavilla olevat paketit, huom: "sudo apt upgrade" päivittää kaikki asennetut paketit
  - apt search
    - Etsii paketteja hakusanoilla, esim. "apt search dungeon adventure"
  - sudo apt install
    - Asentaa paketteja
  - sudo apt purge
    - Poistaa paketteja, mukaanlukien paketteihin liittyvät asetukset
  
  (Lisäys: "apg-get" ja "apt-cache" komentoja ei erikseen tarvitse käyttää jokapäiväisessä pakettien asentelussa, pelkkä "apt" sisältää samat toiminnallisuudet.)

__a)__

Asensin micro editorin komennolla:

    $ sudo apt install micro

![image](https://github.com/user-attachments/assets/bdca158a-7465-4a9d-8f48-c33a80203d0f)


__b)__

Kolme itselleni uutta komentoriviohjelmaa olivat __tldr__, __ansiweather__ ja __fzf__. Tldr toimii samalla perjaattella kuin __man__ komento, mutta tiivistää tulosteen keskeimpiin ohjeisiin. Ansiweather näyttää säätiedotteen terminaalissa. Fzf eli "fuzzy finder" avaa etsittäväksi listan syötteestä.

Asensin ohjelmat seuraavalla komennolla:

    $ sudo apt install tldr ansiweather fzf

Esimerkkikäyttö (tldr ansiweather komennosta ja itse ansiweather komento):

![image](https://github.com/user-attachments/assets/67d3bf3f-431b-4f52-877e-3c9ae2a05424)

Pelkkä __fzf__ komento näyttää vain tyhjän valikon, mutta esim. yhdistämällä komennot:

    $ ls | micro $(fzf)

Aukeaa seuraavanlainen valikko:

![image](https://github.com/user-attachments/assets/c003382a-719a-43bd-a4a3-2232622f6676)

Ja kun etsii haluamansa tiedoston listalta, esim. foo.txt, se aukeaa micro editorissa.


__c)__

Juuri- ja kotihakemistojen sisältö:

![image](https://github.com/user-attachments/assets/fafa8db5-fec4-4176-a8af-8e184d6b24e1)

Juurihakemistosta löytyy esim. /bin kansio, joka sisältää kaikki ajettavat ohjelmat eli binääritiedostot.

/etc ja /media hakemistojen sisältö:

![image](https://github.com/user-attachments/assets/7c5bdfd2-20d6-45bf-91c8-60052bfaf036)

/etc hakemistosta löytyy esim. timezone tiedosto, joka sisältää aikavyöhykkeen:

![image](https://github.com/user-attachments/assets/e3404d56-83fd-42e5-bf06-81730c94de2f)

/var/log sisältää logeja. Esim. /var/log/dpkg.log näyttää mitä ohjelmia on asentanut:

    $ cat /var/log/dpkg.log

![image](https://github.com/user-attachments/assets/08ffdba2-4f77-4940-b3fa-8f7fade2ad90)

__d)__

### Grep esimerkkejä
Näytetään aikaisemman dpkg.log tiedoston sisältö, mutta vain rivit joilla lukee "ansiweather":

![image](https://github.com/user-attachments/assets/c002e8a2-d6d8-48f2-9569-f6d316bde13e)

Näytetään kolme riviä jokaisen rivin jälkeen joissa lukee "tldr":

![image](https://github.com/user-attachments/assets/205b2395-9d1c-42ca-bb9a-23496efe21da)

__e)__

Putkitetaan komennon 

    $ ls /etc

tuloste grep komennolle, ja näytetään vain osumat sanalla "host":

![image](https://github.com/user-attachments/assets/6fafac86-0043-4e2b-8670-a2e5ac249dda)

__f)__

![image](https://github.com/user-attachments/assets/dbefdf3b-aced-4b88-9369-dc847b87cf02)

Komennon lshw tuloste näyttää tietoa virtuaalikoneen spekseistä. Tulosteesta voi päätellä, että host käyttöjärjestelmän prosessori on ryzen 7 5700X, RAM muistia on allokoitu 4GB, ja tallennustilaa on 20GB. 

Laitteet /dev/vda 1-3 viittaavat virtuaalisen tallennustilan eri osioihin. Enp1s0 viittaa ethernet yhteyteen. Tulosteesta näkyy myös, että virtualisointi alustana toimii QEMU.

## Lähteet

- Karvinen Tero, 2020: Command Line Basics Revisited
  - https://terokarvinen.com/2020/command-line-basics-revisited
- Alicia Sykes, 2023: CLI tools you won't be able to live without
  - https://dev.to/lissy93/cli-tools-you-cant-live-without-57f6
