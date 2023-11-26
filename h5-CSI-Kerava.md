# H5 - CSI Kerava
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h5 CSI Kerava'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD, Vagrant versio 2.4.0,  VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse. Tein tehtäviä pääosin itsenäisesti. Teimme hetken [Joonas Hautaviidan](https://github.com/hautadata) sekä [Maksim Heikkilän](https://github.com/MaksimHeikkila) kanssa tehtävää Teams-puhelin välityksellä.

## x) Lue ja tiivistä.
Karvinen 2018: [Apache User Homepages Automatically – Salt Package-File-Service Example](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/)

- Pakettitiedostopalvelu (Package-File-Service) on yleisin tapa määrittää demonit.
- Aluksi asenna kaikki käsin, koska voit vain automatisoida asioita jotka tiedät.
- Tiedostoja voi muokata ja katsoa sitten mitä tiedostoja muokattiin.
- Komento ``$ find -printf "%T+ %p\n"|sort`` tulostaa muokkausajat, ja komennossa
     "%T+" on muokkausaika
     "%p" on tiedostonimi ja sen polku
     "\n" on rivinvaihto
- Kun käyttää komentoa tilassa (state), on komennosta tehtävä idempotentti. Artikkelissa on esimerkki "creates" parametrista, joka suorittaa komennon vain, jos tiedostoa ei ole olemassa.

## a) CSI Kerava. Näytä 'find' avulla viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostasi. Selitä kaikki käyttämäsi parametrit ja format string 'man find' avulla.

Aloitin tehtävän käynnistämällä VirtualBoxin. Päätin avata virtuaalikoneen, jossa on Ubuntu 22.04.3. Loin tämän koneen Linux-palvelin -kurssilla. En ole avannut tätä tietokonetta vähään aikaan.

![ubuntu-up](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ab2fb51c-fd53-41ef-966a-03427c6699cb)

Olen luonut aiemmin mainitulla Linux-palvelimet -kurssilla yksinkertaisin skriptin (update_and_upgrade.sh) ja ajoin sen komennolla ``$ bash update_and_upgrade.sh``, jotta uusimmat paketit asentuvat ja päivittyvät järjestelmään. Käynnistin tietokoneen uudelleen komennon onnistuneen suorittamisen jälkeen. 

![update-upgrade-bash](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/440a4f71-bb78-466e-96dd-6004901ec45e)

Skripti on hyvin yksinkertainen, ja sen sisältö näyttää tältä.

![cat-skripti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/28271745-eb94-4e65-b3f3-1ba151ff79a0)

Nyt itse tehtävän kimppuun. Avasin virtuaalikoneen selaimen ja menin Teron [kurssisivulle](https://terokarvinen.com/2023/configuration-management-2023-autumn/), jotta pystyn kopioimaan komentoja helpommin terminaaliin. Siirryin /etc/ -hakemistoon komennolla ``$ cd /etc``. Ajoin ensiksi komennon ``$ find -printf '%T+ %p\n'``, joka tulosti seuraavan:

![print-ilman-sort](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/b138adfb-560e-4101-8019-9905c1b2b95b)

Ihmettelin tässä, että miksi tulosteessa on päivämäärät ihan sekaisin. Viimeisten kahden rivin muokkaukset ovat tältä vuodelta ja kolmanneksi alimman rivin muokkaus vuodelta 2021. Kävin katsomassa vielä tuota tämän tehtävän [artikkelia](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/), josta huomasin, että tuosta ``find -printf '%T+ %p\n'`` puuttuu ``| sort``. Eli ajetaan komento uudelleen, mutta lisätään tuo sorttaus. Eli ajan komennon ``$ find -printf '%T+ %p\n' | sort``.

![print](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fc5b33c6-b86b-4a24-80d4-6f4286f45e94)

Nyt komento listasi uusimmat muokkaukset alimpina. Tosiaan tulosteessa näkyy päivämäärä sekä tiedostopolku.

!HUOM! Ajoin saman komennon kotihakemistossani vasta kun olin tehnyt b) tehtävän, siksi päivämäärä on vähän pidemmällä kun aikaisemmassa kohdassa...

![kotihakemisto-printf](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/cef63eb5-20ba-4149-b472-09be247f4477)

Selitin käyttämäni parametrit kohdassa x) Lue ja tiivistä. Katsoin vielä manuaalisivuilta ``find`` selityksen. Eli ajoin komennon ``$ man find``.

![man-find](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/70eacc97-f480-48dc-8508-046ee195992b)

Manuaalisivut kertovat, että find etsii tiedostoja hakemistohierarkiasta.

## b) Gui2fs. Muokkaa asetuksia jostain graafisen käyttöliittymän (GUI) ohjelmasta käyttäen ohjelman valikoita ja dialogeja. Etsi tämä asetus tiedostojärjestelmästä.

Aloitin tehtävän avaamalla tekstieditorin Ubuntun sovellusvalikosta:

![text-editor-avaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/706752e8-e72e-41ad-8798-197643e921b4)

Kirjoitin tekstitiedostoon sisältöä ja tallensin tiedoston työpöydälle nimellä 'GUI_testi'.

![text-editor-teksti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/96a43a6c-0e7b-49a3-8541-08c3595c7bd3)

![gui-testi-tallennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/e8d1b82f-7041-465c-ae0d-6a5d5430ff74)

Tämän jälkeen suuntasin terminaliin ja ajoin saman komennon kun aiemmassa tehtävässä eli ``$ find -printf '%T+ %p\n' | sort``

![gui-testi-print1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/77c0be43-f7a7-4546-b9bc-b86b16a84e8d)

Tuloste näytti tismalleen samalta kun aiemmin. Ihmettelin hetken, että mistä tämä johtui. Sitten tajusin. Ajoin komennon /etc -hakemistossa. Eli nyt komento ``$ cd`` ja sitten ajatetaan tuo aikaisempi komento uudelleen, eli ``$ find -printf '%T+ %p\n' | sort``

![gui-testi-print2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/88534ffd-87a4-4a22-bd68-bc4a3e730c21)

Ylläolevasta kuvasta voidaan huomata, että GUI:lla tehty tekstitiedosto löytyy tulostuksesta. Etsitään vielä kyseinen tiedosto vielä terminalin kautta. Aluksi siirryin työpöydälle kotihakemistostani komennolla ``$ cd Desktop/``. Listasin työpöydän sisällön komennolla ``$ ls`` ja näytin tiedoston sisällön ``$ cat GUI_testi``

![cat-gui-testi](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c4704a10-4cd8-4f94-b374-6c5d848c4179)

Tehtävä onnistui!

## c) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

Aloitan tehtävän laittamalla Vagrantin virtuaalikoneet käyntiin Windowsin komentoriviltä siirtymällä aiemmin luomaani Vagrant-kansioon nimeltä 'saltdemo´´. Eli ``cd saltdemo`` ja täällä ollessa käynnistin virtuaalikoneet komennolla ``vagrant up``

#![vagrant-up](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/eb6c0b90-e95a-47ad-b7ad-f36a1b3612c6)

Kun koneet käynnistyivät, otin SSH-yhteyden tmasteriin komennolla ``vagrant ssh tmaster``.

![ssh-tmaster](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/77938eea-146d-4e4e-a6f4-cb0c8d7f0f9f)

Onnistuneen yhteydenoton jälkeen suuntasin /usr/local/bin -hakemistoon komennolla ``cd /usr/local/bin``.

![bin-ls](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/e98a9984-e385-49a6-a131-e61f2cc115cb)


Listasin vielä tämän sisällön komennolla ``$ ls``. Hakemisto on tyhjä. Ajatukseni on tehdä samanlainen, yksinkertainen skriptikomento, jonka ajoin a) tehtävän alussa (eli sudo apt update ja sudo apt upgrade). Haluan lisätä tähän sellaisen twistin, että jossei päivityksiä ole saatavilla, niin skripti tulostaa "Ei päivitettävää tai parannettavaa". Muuten skripti tulostaa "Päivitykset ovat asennettu". Aloitin skriptin tekemisen komennolla ``$ sudo nano update_and_upgrade.sh``. Skripti näyttää seuraavalta:

![update-skripti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/da16515f-b84d-4c47-b77c-5657ae72bc6f)

Skriptissä näkyvä  '$?'-muuttuja kertoo edellisen komennon paluuarvon, ja jos paluuarvo on 0, niin se tarkoittaa ettei päivityksiä ollut saatavilla. Tämä tulostaa "Ei päivitettävää tai parannettavaa!". Muuten tulostetaan "Päivitykset ovat asennettu!".

Annoin skriptille suoritusoikeudet komennolla ``$ chmod +x update_and_upgrade.sh`` ja tarkistin oikeudet komennolla ``$ ls -la``

![chmod-ja-listaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/b907f59c-301c-476d-b499-76fe46c9d392)

Nyt ajan tämän skriptin masterilla. Katsotaan toimiiko! Eli ajan komennon ``$ . update_and_upgrade.sh``

![skriptin-ajo-masterilla](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/de8d3f20-052d-4ddc-aafe-25686a1bc02f)

Näyttäisi toimivan! Tuloste toimii myös, kuten kuvasta pystytään huomaamaan!

Siirrytään nyt saltin hakemistoon, eli /srv/salt/, komennolla ``cd /srv/salt/``. Luon aikaisempien tehtävien tapaan uuden hakemiston nimeltä update_and_upgrade. Tämä tapahtuu komennolla ``$ sudo mkdir update_and_upgrade``

![mkdir](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c3229c1c-ec11-426b-b239-859f836370e0)

Listasin vielä tämän hakemiston sisällön, jolloin pystyin varmistamaan, että uusi hakemisto todella luotiin onnistuneesti. Siirryin tähän hakemistoon komennolla ``cd update_and_upgrade/``. Tässä kohtaa katsoin ensimmäistä kertaa apua Hautaviidan [raportista](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h5-csikervo.md). Hän oli luonut vielä samanlaisen skriptitiedoston hänen /salt/onkovklp/ -hakemistoon, eli luon Joonaksen tavoin tiedoston omaan hakemistooni. Kopioin aiemmin tehdyn updaet_and_upgrade.sh -tiedoston tähän työkansioon komennolla ``sudo cp /usr/local/bin/update_and_upgrade.sh /sr/salt/update_and_upgrade/``. Tämä kopioitava tiedosto on nyt .sh päätteellä tässä hakemistossa, joten muutan sitä komennolla ``$ sudo nano update_and_upgrade.sh`` ja tallennan tämän tiedoston ilman .sh -päätettä.

![sh-kopiointi](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/9f716d0c-6021-4dce-80af-9ad922d95dc7)

Nyt luon 'init.sls' -tiedoston, johon laitan skriptin sijainnin ja oikeudet. Tästä oli esimerkki luennolla. Luon tiedoston komennolla ``$ sudo nano init.sls`` ja laitan sisällöksi seuraavan:

![update-init-sls](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/30c57ffb-5afc-4731-90d1-d84d0a9cb2ac)

Tallensin tiedoston ja ajoin komennon ``$ sudo salt '*' state.apply update_and_upgrade``. Tämän pitäisi luoda orjakoneiden /usr/local/bin -hakemistoon aiemmin luomani skriptitiedoston.

![salt-state-apply-update](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7e536986-60cf-415a-9a93-4b0f66235600)

Onnistui! Kuvasta voidaan huomata, että niin t002 kuin t001 -orjakoneille luotiin uusi tiedosto 0755 oikeuksilla /usr/local/bin -hakemistoon. Ja tilat ovat Succeeded:1 (changed=1).

Seuraavaksi ajetaan kyseinen skripti komennolla ``$ sudo salt '*' cmd.run update_and_upgrade`` ja toivotaan, että skripti ja tuloste tulee sillä tavoin kun olen sen halunnut tulevan:

![cmd-run-update](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3caf8181-d236-4691-8e06-261a754d0686)

Kuten kuvasta voidaan huomata, t002 koneella ei ollut päivitettävää tai parannettavaa. Ja tuloste tuli oikein. Kuitenkin t001 koneelta huomaa, että update-vaiheessa haettiin ja päivitettiin tiedostoja. Kuitenkaan mitään upgradetettavaa ei ollut, joten sen takia skripti tulosti "Ei päivitettävää tai parannettavaa!" tulosteen? Olisin ajatellut skriptin tulostavan sen "Päivitykset ovat asennettu!", jos edes updatessa löytyi jotain uutta. Selvästikkään koodi ei ollut ihan täysin oikein. Joka tapauksessa sain onnistuneesti asennettua komennon Saltin kautta! Seuraavan tehtävän pariin...

## d) Apassi. Tee Salt-tila, joka asentaa Apachen näyttämään kotihakemistoja.



# Lähdeluettelo

- Hautaviita, J. 26.11.2023. H5 - CSI Kerava. Luettavissa: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h5-csikervo.md. Luettu 27.11.2023.
-  Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 26.11.2023.
-  Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu 26.11.2023.
-  
