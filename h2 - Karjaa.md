# H2 - Karjaa

Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h2 Karjaa'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD, VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse.

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

  Slater 2017: [What is the definition of "cattle not pets"?](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654).
  
    - Randy Bias kuvailee "lemmikeiksi" palvelinta tai palvelinparia, joita käsitellään korvaamattomana tai uniikkina järjestelmänä joka ei voi ikinä mennä alas. Usein manuaalisesti rakennettu.
    - Hän kuvailee "karjan" olevan enemmän kuin kahden palvelimen kokoelma, joka on rakennettu käyttäen automaatiotyökaluja. Suunniteltu kestämään useamman palvelimen kaatumiset. Ihmisen puuttumista ei tyypillisesti tarvita vikatapahtumissa.
    
  Karvinen 2017: [Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/)
  
    - Lyhyt ja nopea ohje siitä kuinka:
      - Asennettetaan Vagrant ja VirtualBox Linuxilla
      - Käynnistetään uusi virtuaalikone
      - Otetaan yhteys SSH:lla.
      - Virtuaalikone ja sen tiedostot tuhotaan
      
  "Karvinen 2023: [Salt Vagrant - automatically provision one master and two slaves](https://terokarvinen.com/2023/salt-vagrant/)"
    - Artikkeli kertoo kuinka käyttää valmiiksitehtyä verkkoa, jonka sisällä on kolme virtuaalikonetta (yksi master-kone ja kaksi slave(minionia)-konetta) Saltin kanssa.
    
    - Artikkeli näyttää komentojen kera mm. kuinka:
      - Asennetaan virtuaaliympäristö
      - Kuinka virtuaalikoneet käynnistetään Vagrantissa
      - Kuinka hyväksytään orjakoneiden avaimet masterila
      - Kuinka orjakoneista voi kerätä tietoja
      - Esimerkkejä idempotenttikomennoista
      - Kuinka tuhota virtuaalikoneet
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

a) Asenna Vagrant. (Toiminee parhaiten isäntäkäyttöjärjestelmässä, siis siinä, joka pyörii raudalla)

  Aloitin Vagrantin asentamisen koneelleni [Täältä](https://developer.hashicorp.com/vagrant/downloads). Latasin Vagrantista AMD64 2.4.0 -version. Kuva alla:

  ![vagrant-nettisivu](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/419241b9-99bb-4e9e-8687-5b28f943325e)

  Asennus onnistui ongelmitta.
  
  ![vagrant-asennus-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/5d12900f-71b0-4377-87eb-3b7da4718280)

  Tietokoneen uudelleenkäynnistämisen jälkeen varmistin Windowsin komentokehotteesta, että Vagrant on asentunut oikein komennolla ``vagrant -v``.

  ![vagrant-cmd-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3c6c02d8-a7aa-4d01-977a-95d0720be3e9)
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
b) Yksi maankiertäjä. Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.

Tässä tehtävässä käytän apuna Tero Karvisen "[Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/)" -artikkelia. Aloitetaan ensimmäisen virtuaalikoneen luominen Vagrantissa. Windowsin komentokehotteessa komento ``vagrant init debian/bullseye64`` aloittaa luontiprosessin. Tämä komento luo myös tiedoston nimeltä VagrantFile. Tämän jälkeen vuorossa on käynnistää äsken määritelty virtuaalikone komennolla ``vagrant up``. Virtuaalikoneen luonnissa meni aikaa todella vain noin 30 sekuntia. Virtuaalikoneen olemassaolo voidaan varmistaa mm. VirtualBoxista, kuva alla:
  
  ![vb-running](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/626ed1eb-7577-4b8b-b7ab-767b220feed1)

Virtuaalikoneen ollessa päällä, otetaan siihen ssh-yhteys komennolla ``vagrant ssh``.

![vagrant-ssh](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4d34a603-4ebf-4bb1-bc86-ab69325378ac)

Testataan verkkoyhteys pingaamalla. Käytin komentoa ``ping haaga-helia.fi``, joka pingaa Haaga-Helian verkkosivuja. Pingaaminen tuotti tulosta, joten verkkoyhteys toimii.

![vagrant-ping](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fdb8bcc4-15da-4e3d-8be1-6cba247b3c58)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
c) Oma orjansa. Asenna Salt herra ja orja samalle koneelle.

Nyt kun olen jo virtuaalikoneessa kiinni ssh:lla, niin ei muuta kuin asentamaan Saltin herra sekä orja tälle koneelle. Kyseessä pitäisi olla helppo tehtävä. Aloitetaan herran (masterin) asennus komennolla ``sudo apt install salt-master``. Tässä tuli kuitenkin heti ongelma; virtuaalikone ei löytänyt pakettia salt-master.

![salt-master-ongelma](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/6fa0cbc2-564c-4f63-b769-c10f28537c1f)

Mietin hetken, että miksi tilanne on tämä ja tajusin, etten ole ajanut päivityskomentoja, eli komento ``sudo apt update`` ajoon:

![vagrant sudo update](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/279f357e-346b-415a-b8ac-b8249c486c17)

Päitettävää oli ja tämän jälkeen ajoin vielä parannuskomennon ``sudo apt upgrade -y``

![sudo apt upgrade](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/d0bfbd2e-122c-4947-8369-8c62cdcf5869)

Päivittämisen jälkeen ajoin uudelleen komennon ``sudo apt install salt-master``. Paketin haku ja asennus onnistui.

![salt-master-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/646d29df-16e3-4fb6-86f1-d5d23ed3eaf0)

Asensin vielä Saltin orjan komennolla ``sudo apt install salt-minion``. Tämäkin onnistui nyt ongelmitta.

![salt-minion-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/5f47c7f4-6174-458b-a8b8-43484e1c2e30)

Tarkistetaan nyt, että Saltin herra ja orja löytyvät tältä virtuaalikoneelta. Käytän komentoja ``salt-master --version`` ja ``salt-minion --version``.

![salt-versiot](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/dcd9aa3c-c5ed-4edf-a624-7a722f89da98)

Molemmat versiot löytyvät, joten voin tuhota nykyisen virtuaalikoneen komennolla ``vagrant destroy``  ja mennä aloittamaan seuraavaan tehtävään. 

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli.

Tässä tehtävässä mennään tiukasti Tero Karvisen [Salt Vagrant - automatically provision one master and two slaves](https://terokarvinen.com/2023/salt-vagrant/) -artikkelin ohjeiden mukaisesti.

Aloitin tehtävän luomalla uuden kansion nimeltä saltdemo minun isäntäkoneen C:/käyttäjät/käyttäjänimi/ -hakemistoon. Siirryin komentokehotteella tähän kansioon komennolla ``cd saltdemo``. Tässä hakemistossa ajoin komennon ``vagrant init debian/bullseye64``, joka loi VagrantFilen kyseiseen saltdemo-kansioon. Siirryin tänne ja kopioin Karvisen [artikkelista](https://terokarvinen.com/2023/salt-vagrant/) esimerkin mukaisen VagrantFile-tiedoston sisällön ja korvasin vanhan tiedostonsisällön sillä ja tallensin tiedoston.

![saltdemo-vagrantfile](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fb807d85-bbf7-4e20-b975-e183523e565f)

Tämän jälkeen oli aika käynnistää nämä kolme virtuaalikonetta komennolla ``vagrant up``. Komennon ajamisessa meni aikaa noin viisi minuuttia. Onnistunut lopputulos näytti tältä:

![saltdemo-asennus-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/269809ad-a183-42bd-9424-2ba57daa20cc)

Voimme huomata virtuaalikoneiden olemassaolon myös VirtualBoxista

![saltdemo-virtualbox](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/199731af-08b1-49ac-9042-7233572bcf3f)

Tämän jälkeen kirjauduin Saltin herralle komennolla ``vagrant ssh tmaster``

![saltdemo-ssh-tmaster](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/24e9dbf9-9f37-4e78-a30f-477004fa2a07)

Yhdistämisen jälkeen seurasin Karvisen ohjeita ja hyväksyin orjien avaimet koemnnolla ``sudo salt-key -A``

![saltdemo-tmaster-avaimet](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/927a5471-5339-4ec9-8f7b-11de41f69956)

Kun sain hyväksyttyä avaimet, niin seuraavana testasin herran ja orjien yhteyttä komennolla ``sudo salt '*' test.ping`` ja pingaus onnistui, koska vastaukseksi tuli: True.

![tmaster-ping-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/32182cdb-afc1-42b9-bf07-6ec7567a83f5)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

e) Aja useita idempotentteja (state.single) komentoja verkon yli.

Aloitin tämän tehtävän kokeilemalla idempotentin ajoa asentamalla net-tools -paketin orjakoneille. Tähän käytin jo aiemmasta tehtävästä tuttua pkg.installed -funktiota. Eli ajoin komennon ``sudo salt '*' state.single pkg.installed net-tools``. Komennon ajo onnistui ja seuraavaksi kuvat komennon yhteenvedoista molemmilla orjakoneilla (t001 ja t002):

![t001-net-tools](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/17db7206-ff4b-4909-9c34-d3c2528cc039)

![t002-net-tools](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/928f0ee0-44ae-4775-9656-17d31de3dd34)

Kuten kuvista voidaan huomata, Changes-kohdassa näkyy net-tools:n uusi versio ja Succeeded: 1 (changed=1), eli komento onnistui ja net-tools asentui molemmille orjakoneille. Ajan seuraavaksi saman komennon uusiksi, ja lopputulos pitäisi olla, että komento ajettiin onnistuneesti, mutta muutoksia ei tehty, sillä net-tools löytyi jo molemmista koneista... katsotaan:

![net-tools-idempotentti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4f14c66e-a4ad-46e3-8343-b720e24342ed)

Eli tässä kävi juuri niinkuin toivoinkin. Komento ajettiin, mutta muutoksia ei tehty. Eli idempotentti saavutettu. Haluan käydä varmistamassa vielä, että net-tools löytyy orjakoneella. Komento ``exit`` sulkee ssh-yhteyden tmasterilta. Otin uuden ssh-yhteyden orjakoneeseen (t002) komennolla ``vagrant ssh t002``.

![ssh-t002](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/78b182f4-a83c-49fc-8f63-327ceb2e55c1)

Onnistuneen ssh-yhteydenoton jälkeen tarkistin asennetun net-toolsin tilan komennolla ``dpkg -s net-tools``. Paketti löytyi, eli masterin kautta suoritettu asennus todellakin onnistui. Etsin lisätietoa dkpg-komennon käytöstä [täältä](https://www.linux.fi/wiki/Dpkg).

![dpkg net-tools](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1b371855-d416-49e7-a76d-b34c0412a69f)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
f) Kerää teknistä tietoa orjista verkon yli (grains.item)

Ensiksi ajoin komennon ``sudo salt '*' grains.items``, joka listasi todella paljon erilaista tietoa orjakoneista. Kävin tietoja läpi ja halusin tarkkailla näiden koneiden IP-osoitteita, sillä ne ovat mielestäni erittäin tärkeä tietää. Käytin tähän komentoa ``sudo salt '*' grains.item ip_interfaces``.

![grains item](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4cd87261-30d9-41da-9138-529c8d671338).

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
g) Aja shell-komento orjalla verkon yli.

Koska tietoturva kiinnostaa minua, niin kiinnostaa myös laitteiden ja järjestelmien päivitys. Muistaakseni ensimmäinen komento jonka opin Linuxista oli Linux-palvelimet -kurssilla ``sudo apt update``. Haluan saada tämän komennon ajettua orjalla masterin toimesta. Eli ajetaan komento ``sudo salt '*' state.single cmd.run sudo apt update``.

![cmd-run-update-ongelma](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a9f573cb-aba3-4709-a271-d914b3b8905a)

Muutoksia ei tapahtunut. Tässä kohtaan oli sormi suussa hetken. Ajoin komennon uudelleen, mutta lisäsin ''-väkäset tuohon. Eli komento näytti lopulta tältä ``sudo salt '*' state.single cmd.run 'sudo apt update'``.

![cmd-run-update-valmis](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/0dcb55d5-5f0e-4e98-8b87-0e66de4aa797)

Komento onnistui lopulta ja päivitykset saadaan laitettua orjakoneille nyt helposti tuolla ylläolevalla komennolla. Tämä säästää todella paljon aikaa, sillä nyt ei tarvitse asentaa joka koneella päivityksiä manuaalisesti.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
h) Hello, IaC. Tee infraa koodina kirjoittamalla /srv/salt/hello/init.sls. Aja tila jollekin orjalle. Tila voi esimerkiksi tehdä esimerkkitiedoston johonkin hakemistoon. Testaa toisella komennolla, että pyytämäsi muutos on todella tehty.

Aloitin tämän tehtävän luomalla hakemiston Karvisen [ohjeilla](https://terokarvinen.com/2023/salt-vagrant/) kohdassa "Infra as Code - Your wishes as a text file". Eli suoritin ensiksi komennon ``sudo mkdir -p /srv/salt/hello``, joka loi kyseisen hello-kansion hakemistoon. Seuraavaksi ajoin ohjeissa olevan komennon ``sudoedit /srv/salt/hello/init.sls``, joka loi init.sls -tiedoston. Lisäsin tiedostoon artikkelissa näkyvän sisällön, eli 

![nano-init](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a011b3ac-9575-4bb9-8c16-368db80dae8d)

Tämän jälkeen ajoin komennon ``sudo salt-call '*' state.apply hello``. Jos päättelin oikein, niin tämä ajaa tuon init.sls-tiedoston suoraan tuolta hello-kansiosta? Ja luo sitten uuden tiedoston nimeltä infra-as-code orjakoneille.

![apply-hello-valmis](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/45a007eb-8fd1-434a-ab5c-7f72bae06d46)

Seuraavaksi käyn tarkistamassa t002 orjakoneelta, että kyseinen infra-as-code -tiedosto löytyy /tmp/ -hakemistosta.

![t002 infra](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ad8a897d-01d0-4f0f-a277-2c22636d780c)

Ja löytyhän se. Nyt ajan saman komennon vielä uusiksi masterilla, jotta nähdään onko komento idemptotentti:

![hello-idempotentti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/67946365-5c97-48c8-9506-c2fb1226b733)

Kuvasta voidaan huomata, että kyseinen infra-as-code -tiedosto on jo olemassa oikeilla oikeuksilla, jonka takia muutoksia ei tehty. Komento ajettiin kuitenkin onnistuneesti!


## Lähdeluettelo
- Bias, R. 29.9.2016. The History of Pets vs Cattle and How to Use the Analogy Properly. Luettavissa: http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/. Luettu 4.11.2023.
- HashiCorp 2023. Install Vagrant. Luettavissa: https://developer.hashicorp.com/vagrant/downloads. Luettu 4.11.2023.
- Linux.fi 2020. dpkg. Luettavissa: https://www.linux.fi/wiki/Dpkg. Luettu: 6.11.2023
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 6.11.2023.
- Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/. Luettu 6.11.2023.
- Karvinen, T. 11.4.2017. Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds. Luettavissa: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/. Luettu 6.11.2023.
- Slater, R. 25.3.2017. What is the definition of "cattle not pets"? Luettavissa: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654. Luettu 4.11.2023.
