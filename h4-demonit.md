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
    Tässä kohdassa näytettiin esimerkit kuinka luoda SSH-tilatiedosto sekä Apache-tilatiedosto.

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

Tajusin, että unohdin lisätä sudon komentooni. Yritetään uudelleen. Eli avataan tiedosto, laitetaan sama teksti ja tallennetaan ja suljetaan tiedosto. Onnistui! Tarkistetaan vielä, että muutokset todella tallentuivat komennolla ``$ cat init.sls.

![init-päivitetty](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f1967857-cfa3-408a-a4a0-a2b440c3d53c)

Nyt on vuorossa tilan toiminnan testaus. Ajetaan komento ``sudo salt '*' state. apply hello``.

![init virhe](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/99635d9b-4930-47c6-b366-f92265a04fb5)

No eipä toimi. Avataanpas tuo init.sls tiedosto uudelleen. Virhe löytyi: kaksoispiste puuttui cmd.run jälkeen. Sen pystyy huomata nyt aikaisemmista kuvista. Lisätään kaksoispiste ja katsotaan lähteekö toimimaan sitten.

![init-korjaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ab23c8ba-ea2c-43a3-9b78-21231d30b672)

Ja virhe. Ei toimi vielläkään. Avataan uudestaan... Hetken pähkäilyn jälkeen tajusin, että myös tuosta helloworld jälkeen puuttuu kaksoispiste... Tehdään sama muutos vielä ja yritetään uudelleen...


![init-korjaus2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a0a6212f-6373-4bc2-bea3-7e108a33b909)

Noniin! Vihdoin toimii. Alla olevasta kuvasta pystyy myös huomata, että tilan ajaminen onnistui molemmilla orjakoneilla.

![onnistunut-ajo](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2d48679f-4a51-47e1-8464-3559ecf8829b)

Seuraavan tehtävän kimppuun!

## b) Top. Tee top.sls niin, että tilat ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply".



## Lähdeluettelo

-  Hautaviita, J. updated 18.11.2023. H4 - Demonit. Luettavissa: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md. Luettu 19.11.2023.
-  Helminen, T. 6.11.2023. H2 - Karjaa. Luettavissa: https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/blob/main/h2-karjaa.md. Luettu 20.11.2023.
-  Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 19.11.2023.
-  Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu 19.11.2023.
-  RedHat. 3.3.2023. What is YAML? Luettavissa: https://www.redhat.com/en/topics/automation/what-is-yaml. Luettu 19.11.2023.
-  Salt Project, s.a.. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu 19.11.2023.
-  Salt Project, s.a.. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules. Luettu 19.11.2023.
