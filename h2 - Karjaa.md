# H2 - Karjaa

Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h2 Karjaa'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse.

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

  Slater 2017: [What is the definition of "cattle not pets"?](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654). (Vain tuo yksi vastaus DevOps Stack Exchangen kysymykseen)
  
    - Randy Bias kuvailee "lemmikeiksi" palvelinta tai palvelinparia, joita käsitellään korvaamattomana tai uniikkina järjestelmänä joka ei voi ikinä mennä alas. Usein manuaalisesti rakennettu.
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
    
a) Asenna Vagrant. (Toiminee parhaiten isäntäkäyttöjärjestelmässä, siis siinä, joka pyörii raudalla)

  Aloitin Vagrantin asentamisen koneelleni [Täältä](https://developer.hashicorp.com/vagrant/downloads). Latasin Vagrantista AMD64 2.4.0 -version. 

  ![vagrant-nettisivu](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/419241b9-99bb-4e9e-8687-5b28f943325e)

  Asennus onnistui ongelmitta.
  
  ![vagrant-asennus-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/5d12900f-71b0-4377-87eb-3b7da4718280)

  Tietokoneen uudelleenkäynnistämisen jälkeen varmistin Windowsin komentokehotteesta, että Vagrant on asentunut oikein vagrant -v' -komennolla.

  ![vagrant-cmd-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3c6c02d8-a7aa-4d01-977a-95d0720be3e9)
  

  
## Lähdeluettelo
- https://terokarvinen.com/2023/configuration-management-2023-autumn/
- https://terokarvinen.com/2023/salt-vagrant/
- https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/
- https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654
- http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/
- https://developer.hashicorp.com/vagrant/downloads
