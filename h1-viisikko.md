# h1 - Viisikko

Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin ensimmäisiin kotitehtäviin 'h1 - Viisikko'. Osa tehtävistä on tehty yhdessä Joonas Hautaviidan kanssa koululla kahtena eri päivänä. Työkoneena toimi HP:n Elitebook 830 G5. Asensin Debianin ( Debian 12 aka Bookworm) Oraclen VirtualBoxiin. Kaikki kuvankaappaukset ovat minun ottamia.

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

   Karvinen 2023: [Create a Web Page Using Github](https://terokarvinen.com/2023/create-a-web-page-using-github/)
   
      - Artikkelissa yksinkertaiset ohjeet oman weppisivun julkaisemiseen GitHubissa
      - Opastetaan kuinka tehdä uusi repository (säilytyspaikka tai säiliö) sekä uusi tiedosto md. -tiedosto (MarkDown)
      - Näytetään esimerkillä kuinka MarkDown toimii
      - Artikkelin lopussa kuvankaappaus julkaistusta weppisivusta GitHubissa

  Karvinen 2023: [Run Salt Command Locally](https://terokarvinen.com/2021/salt-run-command-locally/)
  
      - Artikkeli kertoo Saltin paikallisista komennoista
      - Näytetään tärkeimmät tilafunktiot esimerkkikomennoilla:
        - pkg.installed
        - file.managed
        - service.running
        - user.present
        - cmd.run
      - Artikkelin lopussa komento ohjeille "Instructions"
      
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
a) Asenna Salt (salt-minion) koneellesi

Salt-Minion asennetaan koneelle komennolla sudo apt-get -y install salt-minion

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
b) Viisi tärkeintä. Näytä esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset. Käytin esimerkkikomentoja jotka löytyvät Karvinen 2023: [Run Salt Command Locally](https://terokarvinen.com/2021/salt-run-command-locally/) -sivulta.


1. pkg.installed
   
![h1_pkginstalled](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/8116baea-993f-4ae9-8220-d035d4912a9e)
   
Kuvasta näkee kuinka pkg.installed-funktio asentaa paikalliseen koneeseen (koneeseen jota käytän) tree-palvelun. Huomioitavaa on kuvassa mm. kohta Changes, joka näyttää tree:n uuden ja vanhan version. Eli kun vanhaa versiota ei löytynyt, niin sitä ei ollut asennettu komentoa ajaessa. Uusi versio kertoo sen minkä version tree:stä funktio asensi. Toinen huomioitava kohta on Succeeded: 1, eli funktio onnistui.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------   
pkg.removed

![h1_pkgremoved](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/575702a4-c1a0-4a8a-81e3-4b8d5fe3f41c)

Kuvasta näkee kuinka pkg.removed-funktio poistaa paikallisen koneen (koneen jota käytän) tree-palvelun. Huomioitavaa on kuvassa mm. kohta Changes, joka näyttää tree:n uuden ja vanhan version. Eli kun "new"-kohdassa ei löytynyt enää tree:n versiota, niin tree:tä ei ole enää koneessa. Vanhan version kohdalla näkyy se versio tree:stä joka poistui fuktiota suorittaessa. Toinen huomioitava kohta on Succeeded: 1, eli funktio onnistui.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2. file.managed

![h1_filemanaged](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/aeac6528-f0ac-4abd-adf4-48d9cbcd1313)

Kuvasta näkee kuinka file.managed funktio luo uuden tiedoston /tmp/ -hakemistoon nimeltä hellotero. Tiedosto on tyhjä.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   file.managed sisällöllä
   
![h1-filemanaged_sisalto](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/0c18acea-c9ce-4f45-a2d8-d98e4e7c2388)

Kuvasta näkee kuinka file.managed-funktio "contents"-parametrilla luo uuden tiedoston nimeltä moitero /tmp/ -hakemistoon. Tiedosto sisältää sanan foo.

/tmp/ -hakemistosta pystyy tarkastamaan, että tiedostot ovat oikeasti luotu.

![h1-k9](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/c1a511f6-9335-4383-a39a-e29103e5d86d)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  file.absent
  
  ![h1_fileabsent](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/66126efe-e571-405f-83ee-6b4cf29471db)

  Kuvasta näkee kuinka hellotero-tiedosto poistuu kyseisellä funktiolla.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  service.running enable=true
  
  ![h1_apache2failed](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/3abc6de9-28a3-4492-91aa-62fac57d98f2)

  
  Kuvasta näkyy kuinka service.running enable=true -funktio yrittää laittaa apache2-palvelua käyntiin. Tässä tuli kuitenkin ensimmäinen virhe, sillä apache2-palvelua ei oltu   asennettu koneelle. Virhe kuitenkin korjaantui asentamalla apache2-palvelun komennolla sudo apt install apache2. 

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  service.running enable=True
  
  ![h1_servicerunning](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/794955b5-32b3-44af-9c1b-2d53e2e4de1e)

  Kuvasta pystyy näkemään, että apache2-palvelu käynnistetään onnistuneesti (Succeeded: 1).
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  service.dead enable=False
  
  ![h1_servicedead](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/903e0165-c743-46f9-b032-4db04a7bb481)

  Kuvasta näkyy kuinka apache2-palvelu disabloidaan ja "tapetaan".

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  user.present

  ![h1_userpresent](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/15a14b0c-998d-4775-b2aa-61b1eb362aa9)

  Kuvasta näkyy kuinka kyseinen funktio luo uuden käyttäjän nimeltä terote08 onnistuneesti.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  user.absent
  
  ![h1_userabsent](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/7ea9e5dd-5da7-42f4-8267-8098394f1ab9)

  Kuvasta näkyy kuinka kyseinen funktio poistaa käyttäjän terote08 onnistuneesti.
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  cmd.run
  
  ![h1_idempotenttitestaus1](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/e55f0102-c87e-40a8-9795-0282ac8eb604)

  Kuvasta näkyy, että kyseinen funktio luo idempotenttitestaus-tiedoston /tmp/-hakemistoon, koska sen nimistä tiedostoa ei ollut jo olemassa.
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
c) Idempotentti. Anna esimerkki idempotenssista. Aja 'salt-call --local' komentoja, analysoi tulokset, selitä miten idempotenssi ilmenee. Ajan 'sudo salt-call --local -l info state.single cmd.run 'touch /tmp/idempotenttitestaus' creates=/tmp/idempotenttitestaus"' -komennon. Käytän tässä ensimmäisenä samaa kuvaa kun aiemmassa cmd.run -kohdassa.

![h1_idempotenttitestaus1](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/226f6f9a-6195-468b-a959-99973910954c)

Niin kuin aiemmin kerroin jo, kyseinen funktio loi idempotenttitestaus-tiedoston /tmp/-hakemistoon, sillä sen nimistä tiedostoa ei ollut aiemmin olemassa.


![h1_idempotenttitestaus2](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/51bca58a-b75b-4597-b1b5-1be5882b4566)

Tästä kuvasta näkyy hyvin, että komento suoritetaan, mutta se ei luo uutta idempotenttitestaus-tiedostoa, sillä sellainen jo löytyy. Mielestäni tämä on hyvä esimerkki idempotentistä.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
d) Tietoa koneesta. Kerää tietoja koneesta Saltin grains.items -tekniikalla. Poimi kolme kiinnostavaa kohtaa, näytä tulokset ('grains.item osfinger virtual') ja analysoi ne

![h1_grains1](https://github.com/ThomasHelminen/Palvelinten-hallinta--course/assets/148875548/58331e08-91da-45cb-82bd-78e8ac3cbae8)

Ajoin komennon 'sudo salt-call --local grains.items | more'. Otin kuvankaappauksen mielestäni kohdasta, josta näkyy kiinnostavia tietoja. Kuvasta selviää keskusmuistin määrä, prosessorien ja näytönohjaimien määrät sekä mm. Debian käyttöjärjestelmän kutsumanimi "bookworm".

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## References
- https://www.debian.org/releases/bookworm/
- https://docs.saltproject.io/en/getstarted/config/functions.html
- https://docs.saltproject.io/en/latest/ref/states/all/salt.states.cmd.html
- https://terokarvinen.com/2006/raportin-kirjoittaminen-4/?fromSearch=raportin
- https://terokarvinen.com/2021/install-debian-on-virtualbox/
- https://terokarvinen.com/2021/salt-run-command-locally/
- https://terokarvinen.com/2023/create-a-web-page-using-github/
- https://ubuntu.com/tutorials/install-and-configure-apache#2-installing-apache



