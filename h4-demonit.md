# H4 - Demonit
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h4 Demonit'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD, Vagrant versio 2.4.0,  VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse.

## x) Lue ja tiivistä.

Karvinen 2023: [Salt Vagrant - automatically provision one master and two slaves](https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file), vain kohdat:
- Infra as Code - Your wishes as a text file

Tässä kohdassa huomautetaan, että kohdan esimerkissä oleva syntaksi on YAML-kieltä. Mitä on YAML-kieli? YAML on ihmisten luettavissa oleva tietojen serialisointikieli, jota käytetään usein asetustiedostojen kirjoittamiseen ([RedHat 2023](https://www.redhat.com/en/topics/automation/what-is-yaml)). Ja sisennykset merkkaavat. Ei tabulaattorin käyttöä, vaan kaksi välilyöntiä.

  
- top.sls - What Slave Runs What States

Top tiedosto määrittää mitä tiloja (states)  suoritetaan milläkin orjakoneella.

Salt contributors: [Salt overview](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml), kohdat:

- Rules of YAML

  - Saltissa käytettävien tiedostojen oletusrenderöijä on YAML-renderöijä
  - YAML-merkintäkielessä on monia voimakkaita ominaisuuksia
  - YAML-renderöijän päätehtävä kääntää YAML-tietorakenne Python tietorakenteeksi Saltille
      - tiedot jäsennelty ``key: value`` pareihin
      - EI tabulaattorin käyttöä, vain välilyöntejä
      - Kommentit alkavat risuaidalla (hash) #

- YAML simple structure

YAML koostuu kolmesta pääelementistä:
  1. Skalaarit (Scalars)
  2. Listat (Lists)
  3. Sanakirjat (Dictionaries)

- Lists and dictionaries - YAML block structures
  - Sisennys täytyy tehdä, sillä se määrittää kontekstin.
      - Ominaisuudet pitää sisentää ja listata yhdellä tai useammalla välilyönnillä
  - Luettelo tai sanakirjalohko osoittaa jokaisen merkinnän yhdysviivalla ja välilyönnillä ("- ")
 
  Salt contributors: [Salt states](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules), kohdat

  - State modules
 
  Kun luodaan yksittäisiä tiloja (states), niin määritetään module.function tilamoduuleista. Tilamoduulit kutsuvat suoritusmoduulin vastakappaleita, ja ne lisävät tai rajoittavat suoritusmoduulin vaihtoehtoja tilallisia toimintoja varten.
 
  - The state SLS data structure
   Tilatiedoston tilan määritelmässä on seuraavat osat:
    - Tunniste (Identifyer)
    - Tila (State)
    - Funktio (Funktio)
    - Nimi (Name)
    - Argumentit (Arguments)
 
  - Organizing states
      - Organisoidaan Saltin tiloja siten, että tarvittaessa toinen kehittäjä (developer) näkee Saltin tilan tarkoituksen ja tilan työnkulun vaivattomasti. Tämä onnistuu esimerkiksi käyttämällä vain muutamia sisäkkäistasoja.
 
  - The top.sls file
      - top.sls tiedosto yhdistää Saltin tietyn tilan tietylle orjalle (minion/slave), jottei tarvitse ajaa jokaista tilaa erikseen ja kohdistaa ne oikeisiin orjakoneisiin joka kerta
 
  - Create the SSH state, Create the Apache state
      - Tässä kohdassa näytettiin esimerkit kuinka luoda SSH-tilatiedosto sekä Apache-tilatiedosto.
   
  Karvinen 2018: [Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port](https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh)
  
  - Tässä artikkelissa näytetään yksinkertainen Salt tila SSH portin muuttamiseksi.

## a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

Otin mallia tähän tehtävään Joonas Hautaviidan "[H4 Demonit](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md)" tehtävästä, sillä hän on tehnyt tämän mielestäni erittäin järkevästi, käyttäen aiemmassa kurssitehtävässä luotua herra-orja-arkkitehtuuria. 

Aloitin tämän tehtävän tekemisen avaamalla aiemmin tehdyn harjoituksen [h2 - karjaa](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/blob/main/h2-karjaa.md) herra-orja-arkkitehtuurin.

1. Avasin isäntäkoneellani komentorivin (command prompt).
2. Siirryin [h2-tehtävässä](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/blob/main/h2-karjaa.md) luomaani saltdemo-kansioon komennolla ``cd saltdemo``
3. kansiossa ollessani laitoin virtuaalikoneet päälle komennolla ``vagrant up``. Alla olevassa kuvassa näkyy, että kyseisiä virtuaalikoneita "herätellään"

![vagrant-up](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fb4b6597-a8fe-4254-b201-6b434980875d)

4. Virtuaalikoneiden käynnistymiseen meni aikaa noin minuutti. Kun virtuaalikoneet olivat käynnistyneet, otin masteriin SSH-yhteyden komennolla ``vagrant ssh``. Komento näkyy alla olevassa kuvassa.

![vagrant-ssh](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1e733901-5399-455a-ad65-56514fb916e4)

Siirryin [Hautaviidan](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md) tapaan [h2-tehtävässä](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/blob/main/h2-karjaa.md) luotuun /srv/salt/hello/ -hakemistoon komennolla ``$ cd /srv/salt/hello/`` ja listasin hakemiston sisällön. Sieltä löytyi aiemmin tehty init.sls -tiedosto.

![cd-hello](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f49c2d6c-e905-4a35-9924-33df756769d3)

Avasin init.sls -tiedoston Nanolla komennolla ``$ nano init.sls``. Otin kaiken pois ja lisäsin uutta tekstiä YAML-muodossa. Yritin tallentaa tiedoston, mutta virhe ilmestyi Nanon alareunaan. Permission denied.

![init-varoitus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a7cea04c-d28a-4739-b939-67a87a605d74)

Tajusin, että unohdin lisätä sudon komentooni. Yritetään uudelleen. Eli avataan tiedosto, laitetaan sama teksti ja tallennetaan ja suljetaan tiedosto. Onnistui! Tarkistetaan vielä, että muutokset todella tallentuivat komennolla ``$ cat init.sls.``

![init-päivitetty](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f1967857-cfa3-408a-a4a0-a2b440c3d53c)

Nyt on vuorossa tilan toiminnan testaus. Ajetaan komento ``$ sudo salt '*' state. apply hello``.

![init virhe](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/99635d9b-4930-47c6-b366-f92265a04fb5)

No eipä toimi. Avataanpas tuo init.sls tiedosto uudelleen. Virhe löytyi: kaksoispiste puuttui cmd.run jälkeen. Sen pystyy huomata nyt aikaisemmista kuvista. Lisätään kaksoispiste ja katsotaan lähteekö toimimaan sitten.

![init-korjaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ab23c8ba-ea2c-43a3-9b78-21231d30b672)

Ja virhe. Ei toimi vielläkään. Avataan uudestaan... Hetken pähkäilyn jälkeen tajusin, että myös tuosta helloworld jälkeen puuttuu kaksoispiste... Tehdään sama muutos vielä ja yritetään uudelleen...


![init-korjaus2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a0a6212f-6373-4bc2-bea3-7e108a33b909)

Noniin! Vihdoin toimii. Alla olevasta kuvasta pystyy myös huomata, että tilan ajaminen onnistui molemmilla orjakoneilla.

![onnistunut-ajo](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2d48679f-4a51-47e1-8464-3559ecf8829b)

Seuraavan tehtävän kimppuun!

## b) Top. Tee top.sls niin, että tilat ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply".

Minulla ei ollut top.sls -tiedostoa, joten katsoin apua sen tekemiseen [täältä](https://docs.saltproject.io/en/latest/ref/states/top.html). Loin top.sls -tiedoston /srv/salt/ -hakemistoon ollessani siellä komennolla ``$ sudo nano top.sls``. Katsoin äskeisestä linkistä vielä, että tämä tiedosto tarvitsee kolme komponenttia: ympäristön, kohteen ja tilatiedostot. Eli loin yksinkertaisen YAML-sisällön tähän top.sls -tiedostoon:

![top-sls](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fe6f44b1-14c6-4f38-baf4-00e7d492bc98)

Eli nyt pitäisi riittää kun ajan komennon ``$ sudo salt '*' state.apply``. Kokeillaan...


![top-sls-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/38abf268-0f33-4e43-9b8d-777c7c9f0a48)

Tehtävä onnistui! Nyt tässä kaikessa alkaa olemaan järkeä. Muistan kuinka hukassa olin alussa kun tutustuin ensimmäisen kerran Saltiin. Pikku hiljaa, toistojen kautta nämä järjestelmät ja työkalut tulevat tutuiksi. Mennään eteenpäin... Seuraavaan tehtävään!

## c) Apache. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy.

    Ensin käsin, vasta sitten automaattisesti.
    Kirjoita tila sls-tiedostoon.
    pkg-file-service
    Tässä ei tarvita service:ssä watch, koska index.html ei ole asetustiedosto

Aloitetaan tehtävä Apachen asentamisella orjakoneelle t002 käsin. Olen tällä hetkellä kirjautuneena tmasterina, joten vaihdetaan käyttäjää. Tai itseasiassa ei vaihdeta, vaan katkaistaan SSH-yhteys tmasteriin, ja otetaan uusi SSH-yhteys orjakoneeseen t001 ja asennetaan Apache-palvelu

  1. ``$ exit`` jolloin SSH-yhteys tmasteriin katkeaa

![tmaster-exit](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1ed2b9e1-b434-49ef-b8a1-2e31c310daf8)

  2. ``vagrant ssh t002`` Ottaa SSH-yhteyden orjakoneeseen t002

![t002-ssh-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/44c3d1c0-e448-4dc6-a2b2-f3d2a5e8e5a3)

  3. ``$ sudo apt install apache2`` asentaa orjakoneelle Apachen.

![t002-apache-install](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/b12e9c39-db22-41c4-ba46-489e3cf25e4e)

  4. Onnistuneen Apachen asentamisen jälkeen siirrytään /var/www/html -hakemistoon komennolla ``$ cd /var/www/html``. Täältä löytyy index.html tiedosto, joka korvataan uudella tiedostolla tehtävänannon mukaisesti.

![t002-apache-html-muutos](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/e187ab09-2def-438f-b69e-7df44dff2924)

Ylläolevasta kuvasta näkee kuinka ensiksi listasin hakemiston sisällön, poistin index.html -tiedoston, listasin sisällön uudelleen, jotta pystyttiin varmistamaan onnistunut poisto, loin uuden testi.html -tiedoston ja avasin sen sisällön. 

  5. Varmistetaan että Apache-demoni on käynnissä komennolla ``$ sudo systemctl status apache2``

![t002-apache-systemctl](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/00caa247-0bab-4f19-a671-c793cb1ac03c)

Tämän tehtävän seuraavassa vaiheessa luon uuden .sls-tiedoston, joka asentaa apache2:n, poistaa oletus .html-sivun ja korvaa sen uudella sekä tarkistaa Apache2 tilan. Tehtävä alkaa sillä, että otan yhteyden tmasteriin. Eli orjakoneelta ``$ exit`` jonka jälkeen isäntäkoneelta ``vagrant ssh tmaster``. 

![t002-exit](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fcf09001-1f7b-4522-b920-66a45fe1d5c6)

Nyt kun olemme masterilla, siirrytään /srv/salt -hakemistoon komennolla ``$ cd /srv/salt/``. Kun olemme tässä hakemistossa, niin luodaan uusi kansio nimeltä apache2 komennolla ``sudo mkdir apache2``.

![mkdir-apache2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/5bfb5aa1-51ea-425e-ad7e-fee24ef5c3de)

Siirrytään tähän hakemistoon komennolla ``cd apache2/``. Luodaan sitten uusi tiedosto nimeltä init.sls. Tämä tapahtuu komennolla ``$ sudo nano init.sls``, joka avaa tiedoston Nanoon.

![cd-apache2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/82114295-1bee-4753-9728-1ba393952e36)

Nyt on aika tehdä taikaa. Eli luodaan neljä eri funktiota, jotka tekevät tämän osatehtävän alussa mainitut asiat.

![apache2-init](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/00c48a1a-a585-4db3-adac-2c938d652f52)

Yllä olevan kuvan funktiot:

1. asentaa Apache2:n
2. poistaa oletuksena olevan index.html-tiedoston
3. lisää uuden html-tiedoston nimeltä newfile.html
4. tarkistaa apache2 tilan

Nyt vain tallennetaan tiedosto ja ajetaan se molemmille orjakoneille komennolla ``$ sudo salt '*' state.apply apache2``. Komento toimi! Perkaan tuloksia seuraavaksi. Ensimmäisenä vuorossa orjakone t002:

![apache2-ajo-t002](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/672ffed6-26ad-43c0-ba3a-1facb733c0cb)

Voimme huomata kuvasta, että kaikki funktiot ajettiin, mutta vain kaksi muuttui. Tämä johtuu siitä, ett asensin aiemmin tälle orjakoneelle Apache2:n ja poistin index.html -tiedoston. Sen takia, nämä funktiot eivät muuttuneet. Eli komento meni juuri niinkuin pitikin. Uusi newfile.html -tiedosto luotiin, sekä Apache2:n tilakomento ajettiin. Loistavaa!

Seuraavaksi vuoroon t001:

![t001-apache-ajo-valmis](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/756e6702-6764-490e-882c-70ddc6f623c9)

Kuvaa suurentamalla voidaan huomata, että kaikki funktiot ajettiin onnistuneesti sekä jokainen funktio tuotti muutoksen. Alussa Apache2 asennettiin, seuraavaksi index.hmtl -tiedosto poistettiin ja uusi newfile.html -tiedosto luotiin ja viimeisenä Apache2:n tilakomento ajettiin. Tehtävä plakkarissa. Tämän tiedoston onnistunut ajaminen tuotti tähän mennessä eniten iloa tällä kurssilla! Viimeisen tehtävän pariin...

## d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.     
    
    Tämä tehtävä on helpointa tehdä tavallisella virtuaalikoneella, jota Vagrant ei ohjaa.
    Löydät oikean asetuksen katsomalla SSH:n asetustiedostoa
    Nyt tarvitaan service-watch, jotta demoni käynnistetään uudelleen, jos asetustiedosto muuttuu masterilla

Koska tämä tehtävä tehdään ns. tavallisella virtuaalikoneella, niin nyt on aika sammuttaa Vagrantin virtuaalikoneet. Katkaistaan SSH-yhteys ja suoritetaan isäntäjärjestelmän komentoriviltä komento ``vagrant halt``.

Avataan VirtualBox ja käynnistetään virtuaalikone, jossa on Debian 12 asennettuna.

Kun virtuaalikoneen työpöytä avautuu, avaan terminaalin ja ajan päivityskomennot ``$ sudo apt update`` ja ``$ sudo apt upgrade -y``. Näiden komentojen ajamisen jälkeen latasin OpenSSH-serverin tälle virtuaalikoneelle. Se tapahtui komennolla ``$ sudo apt install openssh-server``.

![openssh-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2f4fb51f-496b-4192-8526-a4938628c512)

Asennuksen jälkeen siirryin /etc/ssh/ -hakemistoon ja avasin sshd_config -tiedoston, joka oli Teron vinkeissä. Jämähdin tähän hetkeksi ja turvauduin taas Hautaviidan [tehtävään](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md), josta hain apua. Ahaa! Tässä tiedostossa oli kommenttina porttinumero (#Port 22). Otin risuaidan pois ja vaihdoin siihen portin 1234, joka oli mainittu myös vihjeissä.

Hautaviidalla oli ongelma SSHd:n kuuntelussa. Hän ei ollut käynnistänyt sshd-palvelua uudelleen, jonka takia kuunteleminen ei onnistunut. Ajoin saman komennon kuin Hautaviita tässä tehtävässä, eli ``$ sudo systemctl restart sshd.service``. Tämän komennon ajamisen jälkeen testasin SSH-yhteyttä tähän koneeseen komennolla ``$ ssh -p1234 thomas@debian-thomas``. SSH-yhteys muodostui ja pääsin sisään!

![ssh-debian-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/373c19a0-2b6a-4c38-a30f-bc12bb935ee4)

Testasin lopuksi vielä SSHd:n tilaa komennolla ``$ sudo systemctl status sshd.service``, josta näkyy, että porttia 1234 kuunnellaan. Tässä näkyy myös, että salasana on hyväksytty SSH:n kautta. Tämä johtuu siitä, kun testasin aikaisemmin tätä SSH-yhteyttä!


![sshd-status](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/622c2124-bce5-4f9a-92b5-399c516fff26)


Mielestäni nämä viikkotehtävät antoivat minulle tähän mennessä eniten verrattuna aikaisempiin viikkotehtäviin. Ehkä siksi, että tehtävissä oli nyt jotain "vanhaa", eikä kaikki oppiminen ja aika mennyt pelkästään uuden oppimiseen, vaan aiemmin opittua pääsi jo vähän soveltamaan.

## Lähdeluettelo

-  Hautaviita, J. updated 18.11.2023. H4 - Demonit. Luettavissa: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md. Luettu 20.11.2023.
-  Helminen, T. 6.11.2023. H2 - Karjaa. Luettavissa: https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/blob/main/h2-karjaa.md. Luettu 20.11.2023.
-  Karvinen, T. 3.4.2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu 20.11.2023.
-  Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 19.11.2023.
-  Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu 19.11.2023.
-  RedHat. 3.3.2023. What is YAML? Luettavissa: https://www.redhat.com/en/topics/automation/what-is-yaml. Luettu 19.11.2023.
-  Salt Project. s.a.. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu 19.11.2023.
-  Salt Project. s.a.. The Top File. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/top.html. Luettu 20.11.2023.
-  Salt Project. s.a.. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules. Luettu 19.11.2023.
