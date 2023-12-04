# H6 - Windows
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h6 Windows'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22631, Intel Core i5-8350U, 16GB RAM, 256GB SSD, VirtualBox versio 7.0.12). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse. Tein kaikki pakolliset tehtävät sekä muutaman vapaaehtoisen tehtävän.

## a) Asenna Windows virtuaalikoneeseen.

Aloitin Windowsin ISO-tiedoston lataamisen [Karvisen](https://terokarvinen.com/2023/configuration-management-2023-autumn/) tämän tämän kurssitehtävän vinkeistä löytyvän [linkin](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) kautta. Valitsen 64-bittisen tiedoston.

![win-lataus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4f3500c3-25a5-4bf4-bfe3-8a8e4a008304)


Tiedoston lataamiseen meni noin viisi minuuttia. Nyt on aika avata VirtualBox ja aloittaa sieltä uuden virtuaalikoneen asennus.

![virtualbox-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/06ff5139-443e-423a-8b1a-e0adaff1beca)

Määritän tälle virtuaalikoneelle 8192 megatavua muistia (RAM), kaksi prosessoria (CPUs) sekä 30 gigatavua kiintolevytilaa. 

![win-speksit](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/912a8349-ddbf-4895-83c1-9335286edd26)

Virtuaalikoneen asetusten määrittelyn jälkeen käynnistin tämän virtuaalikoneen. Windowsin asennusohjelma avautui. Valitsen käyttöjärjestelmän kieleksi Englannin ja aika, valuutta sekä näppäimistökieleksi Suomen. 

![win-asennus1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c880b56c-9f2f-4a6b-95f6-d4936b0a6286)

Hyväksyn seuraavaksi ehdot:

![win-asennus2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/38095a6b-f7af-4688-abf7-406edb102632)

Seuraavana vuorossa on valita paikka johon Windows asennetaan. Valitsen tuon kyseisen kiintolevyn, jonka "loin" virtuaalikoneen asennuksen alussa.

![win-asennus3](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/333d7a6e-6b69-4227-aff1-dac4675af920)

Nyt Windows alkaakin asentumaan! Asennuksen onnistumisen jälkeen asennusikkuna ilmoitti, että uudelleenkäynnistys vaaditaan:

![win-asennus4](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/efa1887a-9163-4671-8b4e-4a52287a269c)

Uudelleenkäynnistämisen jälkeen alettiin määrittämään Windowsin käyttäjätietoja. Valitsin 'Domain join instead', joka löytyi myös Halosen, Rajalan ja Ollikaisen 2023 artikkelista: [Installing Windows 10 on a virtual machine](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md) (tämä artikkeli löytyi viikkotehtävän vapaaehtoisesta tehtävästä).

![win-asennus5](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/233f12be-0a73-4425-b6d7-1b82bb7389c0)

Nyt on aika luoda käyttäjä, eli nimi sekä salasana... Käyttäjän luonnin jälkeen Windows alkoi vielä määrittämään tiedostoja... Niin kuin alla oleva kuva kehottaa: jätän nyt kaiken asentumisen heille!

![win-asennus6](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/d043f819-0551-4f45-bc78-c6b1116f653e)

Nyt olenkin jo työpöydällä! Windowsin asennus onnistui ongelmitta (ainakin tähän mennessä). Kohti seuraavaa tehtävää!

![win-desktop](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/429c09da-117b-4660-9dfe-f8fe6353a38b)

## b) Asenna Salt Windowsille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut.

Aloitan tämän tehtävän avaamalla virtuaalikonella Microsoft Edgen ja sieltä Googlaamalla 'Salt Windows'. Valitsin Googlen ehdottaman [sivuston](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html). Täällä ollessa valitsin alla olevasta kuvasta näkyvän asennus .exe-tiedoston

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

Lähdin tämän tehtävän kimppuun ajamalla aiemmista kurssitehtävistä tutun 'file.managed' komennon. Eli kirjoitin ``salt call --local state.single file.managed 'C:\users\Tom White\Desktop\saltfiletesti.txt' contents="Tässä on vastaus d)kohtaan, jos tämä näkyy!"``

Komennon polku oli helppo määrittää tabulaattorin avulla. Tabulaattori tarjosi automaattisesti väkäset ('') polulle.

![file-managed](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4822dd0f-5bed-4dbb-92b7-3bf240d084f4)

Kuten kuvasta voidaan huomata, vastaus oli True, eli uusi tiedosto luotiin. Ja lopputulos oli Succeeded: 1 (changed=1). Haluan käydä nyt tarkistamassa PowerShellin kautta, että tekstitiedosto luotiin halutulla tekstillä. Siirryin aluksi käyttäjien hakemistoon komennolla ``cd :\Users\``, jonka jälkeen omalle käyttäjälleni ja siitä työpöydälle, kuten alla olevasta kuvasta voi nähdä. Lopuksi cattasin luodun tiedoston tabulaattorin avulla, komennolla ``cat .\saltfiletesti.txt``.

![testi-cat](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/2736092d-9557-4e26-8222-7cfd5d10efd2)

Tiedosto löytyi oikealla sisällöllä, joten tehtävä onnistui oikein!

## e) Kokeile jotain itsellesi uutta toimintoa Saltista Windowsilla. (Voit käyttää apuna edellisten vuosien kotitehtäväraporttia tai Karvinen 2018: [Control Windows with Salt](https://terokarvinen.com/2018/04/18/control-windows-with-salt/). Huomaa, että noissa muistiinpanoissani voi jo hieman ikä painaa, ja niissä on myös epärelevantteja kokeiluja.)

Löysin Salt Projectin sivuilta [artikkelin](https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.system.html), jossa kerrottiin system-moduulista. Yritän käyttää tätä toimintoa nyt ensimmäistä kertaa Saltissa. Kirjoitan aluksi komennon ``salt-call --local system.get_system_time`` ja se toimii! tämä printtaa paikallisen ajan! Ajan myös seuraavat komennot:

``salt-call --local system.get_system_date``

``salt-call --local system.get_system_computer_name``

``salt-call --local system.get_system_computer_desc``

``salt-call --local system.get_system_hostname``

``salt-call --local system.get_system_info``

Lopputulema näyttää lopulta tältä:

![system-get](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/e621bb32-bc18-48ca-a3a4-54ac779c8a17)

Tehtävä onnistui! Tämä system-get toiminto on mielestäni hyvin kätevä, etenkin jos sinun on hallittava kymmeniä, ellei satoja palvelimia. Taas yhdellä komennolla saa paljon tietoa, tässä tapauksessa paikallisesta koneesta!

## h) Vapaaehtoinen: käytä Saltin user-funktiota Windowsilla.

Tämän tehtävän pitäisi olla helppo. Luon uuden käyttäjän komennolla ``salt-call --local state.single user.present new_user``

![user-add1](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/88513a29-75f4-4f8e-b76d-0b36ef53aa26)

![user-add2](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/54ebfbdd-7504-4aaa-994b-01fe420d672a)

Uusi käyttäjä nimeltä new_user luotiin onnistuneesti. Microsoftila löytyy [artikkeli](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localuser?view=powershell-5.1), Get-LocalUser -komennosta, joka listaa kaikki paikalliset käyttäjät. Ajetaan tämä komento ja varmistetaan, että uusi käyttäjä on todella luotu:

![new-user](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/54f170a8-428e-4573-8406-0d275d4b94fa)

Bravo! Kuten yllä olevasta kuvasta voidaan huomata, uusi käyttäjä nimeltä new_user löytyy nimilistalta. Tehtävä suoritettu onnistuneesti.

## i) Vapaaehtoinen: käytä Saltin cmd.run -funktiota Windowsilla.

Tämänkin tehtävän pitäisi olla helppo. Haluan ajaa cmd.run tilafunktion joka avaa muistion (notepad) tässä tapauksessa paikallisesti. Suoritan komennon ``salt-call --local state.single cmd.run 'start notepad.exe'``.

![cmd-run-notepad](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/8697ea43-bbfd-4217-9d6e-c182c108766c)

Komento toimi! Komento avasi ajamisen jälkeen muistion GUI:ssa! Tämä viikkotehtävä oli kokonaisuudessaan suhteellisen helppo. Tämä "helppous" johtunee minusta siitä, että Salt alkaa tulemaan koko ajan entistä tutummaksi. Näillä eväillä on hyvä lähteä kohti kurssin viimeisiä tehtäviä!

# Lähdeluettelo

- Halonen, Rajala ja Ollikainen, 2023. Installing Windows 10 on a virtual machine. Luettavissa: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md. Luettu: 3.12.2023.
- Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 3.12.2023.
- Karvinen, T. 18.4.2018. Control Windows with Salt. Luettavissa: https://terokarvinen.com/2018/04/18/control-windows-with-salt/. Luettu 4.12.2023.
- Microsoft, s.a.. Download Windows 10 Enterprise. Luettavissa: https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise. Luettu 3.12.2023.
- Microsoft, s.a.. Get-LocalUser. Luettavissa: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localuser?view=powershell-5.1. Luettu: 4.12.2023.
- Salt Project, s.a.. salt.modules.system. Luettavissa: https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.system.html. Luettu 4.12.2023.
- Salt Project, s.a.. Salt install guide Windows: Luettavissa: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html. Luettu 3.12.2023.

