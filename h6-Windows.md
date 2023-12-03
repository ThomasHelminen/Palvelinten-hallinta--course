# H6 - Windows
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h5 CSI Kerava'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD, Vagrant versio 2.4.0,  VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse. Tein tehtäviä pääosin itsenäisesti.

## a) Asenna Windows virtuaalikoneeseen.

Aloitin Windowsin ISO-tiedoston lataamisen [Karvisen](https://terokarvinen.com/2023/configuration-management-2023-autumn/) vinkeistä löytyvän [linkin](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) kautta.

![win-lataus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4f3500c3-25a5-4bf4-bfe3-8a8e4a008304)


Tiedoston lataamiseen meni noin viisi minuuttia. Nyt on aika avata VirtualBox ja aloittaa sieltä uuden virtuaalikoneen asennus.

![virtualbox-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/06ff5139-443e-423a-8b1a-e0adaff1beca)

Laitän tälle virtuaalikoneelle 8192 megabittia muistia (RAM), kaksi prosessoria (CPUs) sekä 30 gigabittia kiintolevytilaa. 

![win-speksit](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/912a8349-ddbf-4895-83c1-9335286edd26)

Virtuaalikoneen asetusten määrittelyn jälkeen käynnistin tämän virtuaalikoneen. Windowsin asennusohjelma avautui. Valitsen käyttöjärjestelmän kieleksi Englannin ja aika, valuutta sekä näppäimistökieleksi Suomen. 

![win-asennus1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c880b56c-9f2f-4a6b-95f6-d4936b0a6286)

Hyväksyn seuraavaksi ehdot:

![win-asennus2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/38095a6b-f7af-4688-abf7-406edb102632)

Seuraavana vuorossa on valita paikka johon Windows asennetaan. Valitsen tuon kyseisen kiintolevyn, jonka "loin" virtuaalikoneen asennuksen alussa.

![win-asennus3](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/333d7a6e-6b69-4227-aff1-dac4675af920)

Nyt Windows alkaakin asentumaan! Asennuksen onnistumisen jälkeen asennusikkuna ilmoitti, että uudelleenkäynnistys vaaditaan:

![win-asennus4](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/efa1887a-9163-4671-8b4e-4a52287a269c)

Uudelleenkäynnistämisen jälkeen alettiin määrittämään Windowsin käyttäjätietoja. Valitsin 'Domain join instead', joka löytyi myös Halonen, Rajala ja Ollikainen 2023: [Installing Windows 10 on a virtual machine].(https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md) artikkelista.

![win-asennus5](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/233f12be-0a73-4425-b6d7-1b82bb7389c0)

Nyt on aika luoda käyttäjä, eli nimi sekä salasana... Käyttäjän luonnin jälkeen Windows alkoi vielä määrittämään tiedostoja... Niin kuin alla oleva kuva kehottaa: jätän nyt kaiken asentumisen heille!

![win-asennus6](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/d043f819-0551-4f45-bc78-c6b1116f653e)

Nyt olenkin jo työpöydällä! Windowsin asennus onnistui ongelmitta (ainakin tähän mennessä).

![win-desktop](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/429c09da-117b-4660-9dfe-f8fe6353a38b)

## b) Asenna Salt Windowsille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut.

Aloitan tämän tehtävän avaamalla virtuaalikonella Microsoft Edgeen ja sieltä Googlaamalla 'Salt Windows'. Valitsin Googlen ehdottaman [sivuston](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html). Täällä ollessa valitsin alla olevasta kuvasta näkyvän asennus .exe-tiedoston

![salt-install](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f58acac9-3381-45f2-91f7-1dea7a542643)

Sitten asentamaan Saltia.

![salt-install2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/923fbe8b-0e73-4d59-bd03-a5fd4c52ac08)

Jätän Minion Settingsit oletuksiksi, sillä tässä tehtävässä käytetään Saltia paikallisesti (-- local).

![salt-install3](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1fb8ba61-eb59-4e87-8011-bc800e50c236)

Aloitan asennuksen. Melkein heti asennuksen alussa tuli ilmoitus, että vcredist_2013 ei ole asennettu, ja että asennetaanko se? Valitsin kyllä.

![salt-install4](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/695a971e-12a3-47f4-b7b0-92013935f394)

Lopulta tuli ilmoitus, että Salt on asenettu onnistuneesti koneelleni. Valitsin vielä, että Salt-minion käynnistetään:

![salt-install5](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/e5c2aa4a-15a4-4ff1-b1e7-522e4ad245e7)

Nyt avaan PowerShellin järjestelmävalvojana ja ajan komennon ``salt-call --local --version``, jotta voin osoittaa Saltin onnistuneen asennuksen (asennusversio löytyy)


![salt-version](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/60941fce-01e9-4a2a-8861-8ad8fa8c89fa)

Sieltä löytyi! Seuraavan tehtävän kimppuun...

## c) Kerää Windows-koneesta tietoa grains.items -toiminnolla. Poimi 'grains.item' perään muutamia keskeisiä tietoja ja analysoi ne, eli selitä perusteellisesti mitä ne ovat. Kuvaile ja vertaile numeroita.

Aloitan tehtävän ajamalla komennon ``salt-call --local grains.items``

![salt-local-grains-items](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/36b65ab0-5037-402e-9315-0532b5657a71)

Tämä komento antoi paljon tietoa tästä Windows koneesta. Halusin kuitenkin rajata haun vain muutamaan eri kohtaan, ja ajoin komennon ``salt-call --local grains.item ipv4``, ``salt-call --local grains.item ipv6`` ja ``salt-call --local grains.item hostname``

![salt-local-items](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/02f33f0d-22d0-47d3-98c5-eadc2cfc46d1)

Yllä olevasta kuvasta pystyy näkemään tietokoneenv IPv4 ja IPv6 osoitteet. Mielestäni näiden osoitteiden tieto on tärkeää, etenkin, kun luodaan yhteyksiä muihin koneisiin ja jos tähän koneeseen otetaan yhteyttä, esimerkiksi SSH-yhteyden kautta. Hostnamen halusin tietää, sillä siitä näkee tietokoneen isännän nimen. Etenkin, jos hallittavia tietokoneita on paljon, niin niiden nimien tietäminen auttaa varmasti paljon.

## d) Kokeile Saltin file -toimintoa Windowsilla

Aloitan tämän tehtävän tekemisen PowerShellissä komennolla ``salt-call --local single.state.managed C:\users

## e) Kokeile jotain itsellesi uutta toimintoa Saltista Windowsilla. (Voit käyttää apuna edellisten vuosien kotitehtäväraporttia tai Karvinen 2018: [Control Windows with Salt](https://terokarvinen.com/2018/04/18/control-windows-with-salt/). Huomaa, että noissa muistiinpanoissani voi jo hieman ikä painaa, ja niissä on myös epärelevantteja kokeiluja.)



# Lähdeluettelo

- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 3.12.2023.
- https://terokarvinen.com/2018/04/18/control-windows-with-salt/
- https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise
- https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html
- https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md
