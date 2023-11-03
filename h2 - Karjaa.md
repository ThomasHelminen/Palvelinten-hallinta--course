# H2 - Karjaa

Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h2 Karjaa'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse.

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

  Slater 2017: [What is the definition of "cattle not pets"?](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654). (Vain tuo yksi vastaus DevOps Stack Exchangen kysymykseen)
  
    - Randy Bias [kuvailee] (http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/) "lemmikeiksi" palvelinta tai palvelinparia, joita käsitellään korvaamattomina tai uniikkina järjestelmänä joka ei voi ikinä mennä alas. Usein manuaalisesti rakennettu.
    - Hän kuvailee "karjan" olevan enemmän kuin kahden palvelimen kokoelma, joka on rakennettu käyttäen automaatiotyökaluja. Suunniteltu kestämään useamman palvelimen kaatumiset. Ihmisen puuttumista ei tyypillisesti tarvita vikatapahtumissa.
    
  Karvinen 2017: [Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/) (Suosittelen koneeksi 'vagrant init debian/bullseye64')
  
    - Lyhyt ja nopea ohje siitä kuinka:
      - Asennettetaan Vagrant ja VirtualBox Linuxilla
      - Käynnistetään uusi virtuaalikone
      - Otetaan yhteys SSH:lla.
      - Virtuaalikone ja sen tiedostot tuhotaan
      
  Karvinen 2023: [Salt Vagrant - automatically provision one master and two slaves](https://terokarvinen.com/2023/salt-vagrant/)
    - Artikkeli kertoo kuinka käyttää valmiiksitehtyä verkkoa, jonka sisällä on kolme virtuaalikonetta (yksi master-kone ja kaksi slave(minionia)-konetta) Saltin kanssa.
    
    - Artikkeli näyttää komentojen kera mm. kuinka:
      - Asennetaan virtuaaliympäristö
      - Kuinka virtuaalikoneet käynnistetään Vagrantissa
      - Kuinka hyväksytään orjakoneiden avaimet masterila
      - Kuinka orjakoneista voi kerätä tietoja
      - Esimerkkejä idempotenttikomennoista
      - Kuinka tuhota virtuaalikoneet
    



## Lähdeluettelo
- https://terokarvinen.com/2023/configuration-management-2023-autumn/
- https://terokarvinen.com/2023/salt-vagrant/
- https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/
- https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654
- http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/
