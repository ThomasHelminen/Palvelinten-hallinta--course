# H3 - Versio
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




## Lähdeluettelo

-  Hautaviita, J. updated 18.11.2023. H4 - Demonit. Luettavissa: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md. Luettu 19.11.2023.
-  Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 19.11.2023.
-  Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu 19.11.2023
-  RedHat. 3.3.2023. What is YAML? Luettavissa: https://www.redhat.com/en/topics/automation/what-is-yaml. Luettu 19.11.2023.
-  Salt Project, s.a.. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu 19.11.2023.
-  Salt Project, s.a.. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules. Luettu 19.11.2023.
