# H5 - CSI Kerava
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h5 CSI Kerava'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD, Vagrant versio 2.4.0,  VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse. Tein tehtäviä pääosin itsenäisesti. Teimme hetken [Joonas Hautaviidan](https://github.com/hautadata) sekä [Maksim Heikkilän](https://github.com/MaksimHeikkila) kanssa tehtävää Teams-puhelin välityksellä.

## x) Lue ja tiivistä.
Karvinen 2018: [Apache User Homepages Automatically – Salt Package-File-Service Example](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/)

- Pakettitiedostopalvelu (Package-File-Service) on yleisin tapa määrittää demonit.
- Asenna aluksi kaikki käsin, koska voit automatisoida asioita vain jotka tiedät.
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

Skriptissä näkyvä  '$?'-muuttuja kertoo edellisen komennon paluuarvon ([Arnaud Le Blanc 2011](https://stackoverflow.com/questions/7248031/meaning-of-dollar-question-mark-in-shell-scripts)), ja jos paluuarvo on 0, niin se tarkoittaa ettei päivityksiä ollut saatavilla. Tämä tulostaa "Ei päivitettävää tai parannettavaa!". Muuten tulostetaan "Päivitykset ovat asennettu!".

Annoin skriptille suoritusoikeudet komennolla ``$ chmod +x update_and_upgrade.sh`` ja tarkistin oikeudet komennolla ``$ ls -la``

![chmod-ja-listaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/b907f59c-301c-476d-b499-76fe46c9d392)

Nyt ajan tämän skriptin masterilla. Katsotaan toimiiko! Eli ajan komennon ``$ . update_and_upgrade.sh``

![skriptin-ajo-masterilla](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/de8d3f20-052d-4ddc-aafe-25686a1bc02f)

Näyttäisi toimivan! Tuloste toimii myös, kuten kuvasta pystytään huomaamaan!

Siirrytään nyt saltin hakemistoon, eli /srv/salt/, komennolla ``$ cd /srv/salt/``. Luon aikaisempien tehtävien tapaan uuden hakemiston nimeltä update_and_upgrade. Tämä tapahtuu komennolla ``$ sudo mkdir update_and_upgrade``

![mkdir](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c3229c1c-ec11-426b-b239-859f836370e0)

Listasin vielä tämän hakemiston sisällön, jolloin pystyin varmistamaan, että uusi hakemisto todella luotiin onnistuneesti. Siirryin tähän hakemistoon komennolla ``$ cd update_and_upgrade/``. Tässä kohtaa katsoin ensimmäistä kertaa apua Hautaviidan [raportista](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h5-csikervo.md). Hän oli luonut vielä samanlaisen skriptitiedoston hänen /salt/onkovklp/ -hakemistoon, eli luon Joonaksen tavoin tiedoston omaan hakemistooni. Kopioin aiemmin tehdyn update_and_upgrade.sh -tiedoston tähän työkansioon komennolla ``$ sudo cp /usr/local/bin/update_and_upgrade.sh /srv/salt/update_and_upgrade/``. Tämä kopioitava tiedosto on nyt .sh päätteellä tässä hakemistossa, joten muutan sitä komennolla ``$ sudo nano update_and_upgrade.sh`` ja tallennan tämän tiedoston ilman .sh -päätettä.

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

Tässä tehtävässä käytän Karvisen [artikkelissa](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/) löytyviä ohjeita sekä komentopätkää.

Aloitan tehtävän siirtymällä artikkelissakin löytyvään /srv/salt/ -hakemistoon komennolla ``$ cd /srv/salt``. Luon uuden kansion nimeltä 'apassi', koska tästä hakemistosta löytyy jo aiemmassa tehtävässä luotu 'apache2'-hakemisto. Luon uuden hakemiston komennolla ``$ sudo mkdir apassi``. Listaan tämän jälkeen /salt-hakemiston sisällön komennolla ``$ ls``, jotta varmistun kansion luomisesta.

![mkdir-apassi](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ce1cc1c1-3514-44e7-95c7-b70d87e979c6)

Luon tänne apassi.html -tiedoston komennolla ``$ sudo nano apassi.html``, jota käytetään index.html -tiedoston lähteenä, kun ajamme Saltin tilaa. Kopioin W3Schoolsin Tryit basic HTML document -[sivulta](https://www.w3schools.com/html/tryit.asp?filename=tryhtml_basic_document) html-mallipohjan tälle tiedostolle ja muokkaan sitä vähän. 

![apassi-html-sisältö](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/905ae9b1-07e8-4bde-b61c-e9e930c33148)

Tässä kohtaa tajusin, että loin tämän 'apassi.html' -tiedoston /salt-hakemistoon, enkä /salt/apassi -hakemistoon. Eli siirretään tiedosto oikeaan paikkaan komennolla ``$ sudo mv apassi.html /srv/salt/apassi``. Varmistin vielä, että tiedosto siirtyi:

![apassi-html-mv](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f325a889-f630-4388-9fbf-396fcc282a0e)


Onnistuneen tiedoston siirtämisen jälkeen luon uuden init.sls -tiedoston tähän työhakemistoon. Tämä onnistuu komennolla ``$ sudo nano init.sls``. Tähän tiedostoon kopioin koko Karvisen [artikkelin](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/) kohdan 'Better Way with Files' komennon. Vaihdoin komentoon tuon oman 'apassi.html' -tiedoston. Valmis init.sls -tiedosto näyttää nyt tältä:

![apassi-init](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2d2c0762-83a5-41d7-b886-5cc1312a54c2)

Nyt kun init-tiedosto on tehtynä, sormet ristiin, että homma lähtee rokkaamaan! Ajetaan komento ``$ sudo salt '*' state.apply apassi``

![state-apassi-failed](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/07d2ad50-a17e-41dd-b9a2-7948e7e4eceb)

![state-apassi-failed2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7d6695a8-fdcb-4d58-a59c-2ac74a41521d)

Punasta näyttää... Aijai. Joku on pielessä. Puretaan vähän kuvia. Apache2 asennus tarkistettiin ja sen paketit löytyivät jo. Uudet userdir.conf ja userdir.load -tiedostot luotiin, sekä apache2 palvelu uudelleenkäynnistettiin. Kuitenkaan lähdetiedostoa ei löytynyt salt://apache/apassi.html saltev 'base':sta. Mietin hetken, että mikä mättää, mutta sitten tajusin. tuosta salt://apache:sta puuttuu numero kaksi. eli pitäisi olla salt://apache2/apassi. Muutetaan init.sls-tiedostoa ja katsotaan saadaanko lopputulos toiseksi! Eli ajetaan komento ``$ sudo nano init.sls`` ja lisätään numero kaksi tuohon tiedostoon!

![apassi-init-korjattu](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4640fd36-ab18-49d2-9c4e-2872e26341d1)

Tallennetaan tiedosto ja ajetaan äskeinen Saltin komento uudelleen. Eli ``$ sudo salt '*' state.apply apassi``. Fingers crossed, että nyt menee läpi ilman epäonnistumista!

![state-apassi-failed-uudelleen1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7e0d4acf-9966-4330-ac1a-b2e1e8dd42a7)

![state-apassi-failed-uudelleen2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c67d418a-b6dc-4b41-9aa2-4815207faa1b)

Ei vieläkään. Käydäänpäs katsomassa orjakoneiden /var/www/html -hakemistot.

![orjakoneiden-html-tiedostot](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a8ee588e-fbd6-4e55-b664-d10bdf4bd48a)

Noniin! Olen muuttanut sen index.html -tiedoston näissä koneissa aiemmin, joten näillä koneilla on sen sijaan newfile.html -tiedostot. Kirjaudutaan takaisin tmasterille ja mennään korjaamaan init.sls -tiedostoa! Korjaan tässä tiedostossa olevan index.html -tiedoston newfile.html nimiseksi.

![newfile-html](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/5c6166a9-b804-41b7-9667-e60bd608c78b)

Tallennetaan ja ajetaan vielä kerran ``$ sudo salt '*' state.apply apassi``. Jos virhe tulee vieläkin, niin en todella tiedä missä mättää... Kokeillaan:

![state-apassi-ei-vieläkään](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/73554467-0a1b-4098-936a-819e33ff83f4)

Ei toimi edelleenkään. Olenko edes asentanut Apache2:sta tmasterille???

![apache2-puuttuu](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/0bacd0f0-8716-4d15-b22a-b599aaeb3730)

No enpä ollut. Nyt on kieltämättä iso tonnin seteli. Mutta... Asennus ei onnistu... Tässä on nyt ensimmäinen ongelma, joka ei tunnu ratkeavan.

![apache-asennus-ongelma](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2fd7fa8d-93ab-436c-b71d-5811e577a685)

Sammutan Vagrantin koneet komennolla ``vagrant halt`` ja käynnistän ne uudelleen ``vagrant up``. Odotellaan kunnes koneet käynnistyvät uudelleen ja toivotaan, että uudelleenkäynnistämisen jälkeen apache2 asentuu ongelmitta tmasterille...

![vagrant-reboot](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/877cbf8f-52fb-4e5a-bc6a-faa27a80b8cd)

Kirjaudutaan tmasterille ja ajetaan uudelleen komento ``$ sudo apt install apache2``. Nyt...

![apache2-asennus-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/794c533d-6fe9-44cc-9002-8a45406f9bd7)

Noniin! Apache2 on tulossa herralle!

Ajetaan ``$ sudo salt '*' state.apply apassi``. Eikä toimi vieläkään. Lopputulema on täysin sama kuin äsken... Mikä on nyt ongelma... Avataanpas vielä tuo init.sls -tiedosto ja käydään parvekkeella haukkaamassa happea....

Parveke auttoi. tuossa init.sls -tiedostossa on väärä lähdepolku... Miten en ollut huomannut sitä aiemmin?!?!

Muutetaan tämä. Alla olevassa kuvassa muutos ympyröitynä (vaihdoin myös index.html takaisin):

![init-toimii](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/fd3207ba-d7d6-4a85-9d8c-66e0b871635d)

Nyt jos ei toimi, niin hyvää yötä. Ajetaan taas komento ``$ sudo salt '*' state.apply apassi``.

![state-apassi-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a85578d0-1b06-4101-8d66-643dcaf79f9f)

VIHDOIN NÄKYY VIHREÄÄ!!! Olipahan show. Virhe oli pieni, mutta sain ajattua Saltin tilan lopulta onnistuneesti!

Nyt on aika kirjautua t001-orjakoneelle ja testata näkyykö tuo apassi.html tiedosto apachen kotisivuna. Katkaistaan tmasterin SSH-yhteys komennolla ``$ exit`` ja otetaan SSH-yhteys t001-koneeseen komennolla ``vagrant ssh t001``.

![t001-vaihto](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f7be322e-b979-49e6-8354-f2d867bc72c9)

Juttelimme tästä kohdasta Hautaviidan kanssa Teamsissa. Eli asennetaan nyt W3M-sovellus tälle orjakoneelle komennolla ``$ sudo apt install w3m``. W3M on siis teksipohjainen weppiselain. Käytimme tätä sovellusta aiemmalla Linux-palvelimet kurssilla.

![w3m-install](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/b2efc728-9d88-42d4-a06b-bdd5e5b7800c)

Avataan apachen kotisivu W3M komennolla ``$ w3m -v http://localhost``:

![w3m-localhost](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c3bd8423-359b-484e-b733-13e3b1ff922b)

![w3m-apassi-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/325e9a10-6768-44b5-8baa-90f2dad85a30)

Tadaa! Tehtävä onnistui. Vihdoin. Viimeisen pakollisen tehtävän kimppuun...

## Ämpärillinen. Tee Salt-tila, joka asentaa järjestelmään kansiollisen komentoja.

Aloitetaan tehtävä tmasterilta ja menemällä /srv/salt -hakemistoon. Eli ``$ exit`` ja ``vagrant ssh tmaster`` ja ``$ cd /srv/salt``.

![ämpärillinen](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f7ecdd79-8c90-4e25-8252-91ac9f80b2a3)

Luon tähän työhakemistoon uuden kansion nimeltä 'sangollinen'. Tämä tapahtuu komennolla ``$ sudo mkdir sangollinen``. Siirrytään tähän hakemistoon komennolla ``$ cd sangollinen/`` Luodaan tähän hakemistoon vielä kolme eri tyhjää tiedostoa komennolla ``$ sudo touch ahvenia haukia lahnoja``

![touch](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/990bd81a-4b4d-4d59-b9a4-2e8844f6927d)

Seuraavana loin init.sls tiedoston samalla tavoin kuin aiemmassa tehtävässä, eli ``sudo nano init.sls`` ja loin sisällön seuraavan näköiseksi:

![sangollinen-init](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ccdf3e33-d002-4214-92a0-d3869ab5f96f)

Nyt oli vuorossa Salt-tilan ajaminen molemmille orjille. Eli suoritetaan komento ``$ sudo salt '*' state.apply sangollinen``:

![state-apply-sangollinen1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/6ec3e0e7-935a-4d39-985c-7db4160974e9)

![state-apply-sangollinen2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/48bd2717-27c7-4d12-97ff-749358450746)

Ja niinkuin kuvista voidaan huomata, tila ajettiin onnistuneesti läpi. Succeeded: 3 (changed=3). Nyt tehtävän lopussa varmistan vielä, että tiedostot todella lisättiin orjakoneille. Ajan saman komennon kuin Hautaviita tässä tehtävässä, mutta molemmille koneille ``sudo salt '*' cmd.run 'ls /usr/local/bin'``, joka listaa kyseisen polun sisällön:

![salt-ls-sangollinen](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/0387e7ec-071f-4156-9d2a-3f78e307d6a6)

Tiedostot löytyivät! Eli pystyin ajamaan yhdellä Salt-tilalla komennon joka asentaa useamman tiedoston orjakoneille. Tässä tehtävässä oli taas omat ongelmansa, mutta toistojen kautta nämä asiat tulevat tutuiksi. Nämä eniten työtä vaativat kurssit opettavat kyllä eniten. Kiitos, ja nyt hetki huilia näistä.

# Lähdeluettelo

- Hautaviita, J. 26.11.2023. H5 - CSI Kerava. Luettavissa: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h5-csikervo.md. Luettu 27.11.2023.
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 26.11.2023.
- Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu 26.11.2023.
- Le Blanc, A. 30.8.2011. Meaning of $? (dollar question mark) in shell scripts. Luettavissa: https://stackoverflow.com/questions/7248031/meaning-of-dollar-question-mark-in-shell-scripts. Luettu 26.11.2023.
- W3Schools. s.a. Tryit basic HTML document. Luettavissa: https://www.w3schools.com/html/tryit.asp?filename=tryhtml_basic_document. Luettu 27.11.2023.
