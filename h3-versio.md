# H3 - Versio
Tältä sivulta löytyy vastaukset Palvelinten hallinta -kurssin kotitehtävään 'h3 Versio'. Tehtävät löytyvät [täältä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) (Karvinen 2023). Työkoneena toimii HP:n Elitebook 830 G5 (speksit: Windows 11 Pro versio 10.0.22621, Intel Core i5-8350U, 16GB RAM, 256GB SSD). Kaikki kuvankaappaukset ovat minun ottamia, ellen toisin mainitse.

Tehtävän lähtökohdat: En ole käyttänyt GitHubia ennen tätä kurssia. Gittiin en ole koskenut ikinä. Eli oppimista on tiedossa!

Tein tehtäviä yhdessä [Essi Krakaun](https://github.com/esskra) sekä [Joonas Hautaviidan](https://github.com/hautadata) kanssa.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## a) Online. Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "winter". Aiemmin tehty varasto ei kelpaa.

Aloitin uuden varaston luomisen kirjautumalla sisään [GitHubiin](https://github.com/). Uuden varaston (repository) luominen alkaa klikkaamalla +-painiketta GitHubin oikeasta yläkulmasta ja valitsemalla alavalikosta New Repository.

![new-repo](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7e5bc50e-0e89-411f-8b7e-87af0371a66e)

Nimesin uuden varaston "winter-management" ja lisäsin sopivan kuvauksen varastolle. Varaston luomisen yhteydessä lisäsin myös README-tiedoston. Valitsin GNU General Public License v3.0 lisenssiksi.

![create-repo](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/38decd31-99ab-481f-80c4-c55d932c3a21)

Varasto luodaan klikkaamalla Create Repository -painiketta.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.

Aloitetaan tämä osa tehtävää perkaamalla mikä on edes Git? "Git on tähän mennessä ylivoimaisesti eniten käytetty moderni versionhallintajärjestelmä maailmassa." ([Atlassian s.a.](https://www.atlassian.com/git/tutorials/what-is-git))

Teen tätä tehtävää Windows-koneella, joten aluksi täytyy asentaa Git ja Git Bash. Itselle helpoin tapa oli mennä git-scm:n [verkkosivuille](https://git-scm.com/download/win) ja ladata sieltä uusin versio (2.42.0)

![git-lataus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/429b5a3d-9466-43d7-ab81-8a858989bc2c)

Valitsin 64-bittisen version Windowsille.

Asensin Gitin oletusvalinnoilla. Ainoa lisä jonka, valitsin oli asentaa Git Bash samalla. Mikä on Git Bash? "Git tarjoaa Windowsille BASH emulaattorin, jolla voi ajaa Gittia komentoriviltä." ([Git for Windows](https://gitforwindows.org/))

![git-asennus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ae1ba7c9-d0fd-43c3-b4c6-de022a3412de).

Asennuksen onnistuessa varmistin, että Git on todella asentunut. Käytin komentoa `` git version``.

![git-versio-powershell](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7c715ca7-5cf9-4ef7-aec7-1a928df770e3)

Aloin ottamaan selvää miten Githubin varasto kloonataan Gitissa. Luin GitHubin [artikkelin](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) aiheesta, jota käytin apuna tässä tehtävässä. Halusin käyttää SSH-osoitetta kloonaukseen, kuten Karvisen [Tehtävän h3 vinkeissä](https://terokarvinen.com/2023/configuration-management-2023-autumn/) suositeltiin.

Luin TheServerSiden [blogiartikkelin](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/GitHub-SSH-Windows-Example)  "How to SSH into GitHub on Windows example", jossa näytetään kohta kohdalta kuinka luoda SSH-avainpari ja kuinka lisätä julkinen avain omaan Githubiin SSH yhteyden mahdollistamiseksi. Käytän tämän artikkelin ohjeita SSH avainten luomiseen.

Avasin Windowsin PowerShellin ja ajoin komennon ``ssh-keygen -o -t rsa -C "key to winter-management repo"``. Komennossa -o tarkoittaa uusimman OpenSSH formaatin käyttöä, -t tarkoittaa avaimen tyyppiä ja -C tarkoittaa kommenttia tai metadataa joka lisätään julkiseen avaimeen.

![ssh-avain-luonti](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3b18937e-c28b-42c9-b216-092e7e195f57)

Komennon suorittamisen jälkeen voitiin käydä tarkistamassa, että avaimet luotiin onnistuneesti (julkinen avain id_rsa.pub, yksityinen avain id_rsa).

![avaimet](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3195c688-f803-4089-8184-137621481bcd)

Tämän jälkeen luin GitHubin [artikkelin](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) "Adding your SSH key to the ssh-agent" kuinka lisätä uusi SSH avain ssh-agenttiin.

Käytin artikkelin ohjeita, ja käynnistin ssh-agentin PowerShellissä komennoilla ``Get-Service -Name ssh-agent | Set-Service -StartupType Manual`` ja ``Start-Service ssh-agent``. Tämän jälkeen ajoin komennon ``ssh-add yksityisen avaimen polku``, joka lisäsi yksityisen avaimen ssh-agentille. Tällä tavoin voin ottaa yhteyden GitHubiin SSH:lla, kunhan käyn lisäämässä julkisen avaimen omaan GitHubiini. Tehdään tämä seuraavana.

Navigoin itseni oman GitHub-käyttäjäni asetuksissa kohtaan "SSH and GPG keys". Avasin julkisen avaimen sisällön muistiolla, ja kopioin sen sisällön avain-kohtaan. Lisätty avain näkyy seuraavasti

![ssh-add-github](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a590a98f-6f2d-43ed-95dc-aa04d33ae4a5)

Nyt lähdetään testaaamaan saanko otettua SSH yhteyden GitHubiin Git Bashin kautta. Otin ohjeita GitHubin [artikkelista](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection) "Testing your SSH connection". Aloitin yhteyden testauksen ajamalla komennon ``$ ssh -T git@github.com``.

![ssh-yhdistäminen](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/c75f9e9b-dfea-4e58-93a9-8a46c346bd9f)

Komennon ajaminen onnistui. Sain varoituksen, että isännän autenttisuutta ei voida luoda (establish) ja haluanko käyttää avainta jatkaakseen yhdistämistä. Kuten yllä olevasta kuvasta näkyy, vastasin tähän ``yes`` ja autentikointi suoritettiin onnistuneesti.

Tehtävä on ollu tähän asti jokseenkin raskas, sillä kaikki tämä on ollut uutta minulle. Nyt vasta päästään kunnolla itse tehtävän kimppuun. Eli kloonataan aiemmin tehty winter-management -varasto ja tehdään siihen muutoksia ja pusketaan ne onnistuneesti takaisin GitHubiin.

Suoritin tässä vaiheessa paria komentoa, jotka löytyvät Karvisen h3-tehtävän vinkeistä. Eli komento ``$ git config --global user.email "thomas.helminen@hotmail.com"`` ja ``git config --global user.name "Thomas Helminen"``.

![git-identity-komennot](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/cfcf282f-f18e-46b3-a96d-06f56db0393b)


Käsittääkseni nämä tiedot tarvitaan, jotta tiedostoja voi puskea takaisin GitHubiin. Tämän olisi voinut tehdä varmasti myöhemmin, mutta lisäsin nämä "metatiedot" jo nyt. 


Käytin kloonaamisessa apuna [GitHubin](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) artikkelia "Cloning a repository". Aloitin tämän kopioimalla varaston SSH-osoitteen.

![repo-ssh-osoite](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7e8cf443-6abf-4ba7-907f-184940ed58d2)

Tämän jälkeen ajan komennon ``$ git clone git@github.com:ThomasHelminen/winter-management.git``

![git-kloonaus](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ed410fdb-e1b6-47aa-8321-a2550d5a7ab8)

Kloonaus onnistui, ja tämän pystyi tarkistamaan listaamalla oman kotihakemiston tiedostot. Tiedostoissa pitäisi näkyä nyt tämä winter-management varasto. Ajan komennon ``$ ls | grep winter``.

![winter-ls-grep](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/6f8052c8-ecb8-4e4b-8783-781a3c5a88ac)

Varasto löytyi! Haluan lisätä tähän varastoon uuden tiedoston nimeltä push-testi.md. Eli siirryn tähän varastoon Git Bashissa komennolla ``$ cd winter-management/``.

![cd-winter-management](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/4a8a72cb-9ad7-463c-8d8c-959d2e4eadbd)

Nyt on olemme oikeassa varastossa, niin luodaan tänne tiedosto push-testi.md komennolla ``$ nano push-testi.md``.

![nano push-testi-md](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1824e38c-b0cd-4f6b-9202-52e79087b2ce).

Lisäsin tiedostoon kuvassa näkyvän kommentin ja tallensin sen. 

![push-testi-md-sisältö](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/edc50976-7604-4ec6-b858-3688e187864b)

Voimme tarkastaa, että tiedosto on tallentunut listamaalla varaston sisältö Git Bashissa komennolla ``$ ls``

![ls-push-testi](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/039c124b-540e-4ae4-aaa9-f8cc88973159)

Nyt kun tiedosto todella löytyy varastosta, niin lähdetään puskemaan tiedosto takaisin GitHubiin. Käytin puskemiseen Atlassianin Bitbucket supportin [artikkelia](https://support.atlassian.com/bitbucket-cloud/docs/add-edit-and-commit-to-source-files/) "Add, edit, and commit to source files" sekä TheServerSiden [artikkelia](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-push-an-existing-project-to-GitHub) "How to git push an existing project to GitHub".

Aloitan puskemisen ajamalla komennon ``$ git add -all``, jonka pitäisi lisätä kaikki uudet tiedostot tulevaan puskemiseen.

![git-add-all](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/da9e4fc3-4404-4baf-9e4a-0e9062e8a6a6)

Tämän jälkeen aloitan kommitoida puskemista. Ajan komennon ``$ git commit -m "Pusketaan uusi tiedosto GitHubiin"``

![Näyttökuva 2023-11-10 112551](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f2420c9c-69d1-4c00-9e08-3f0a57b867b8)

Kuvasta pystyy näkemään, että muutoksia on tehty ja yksi tiedosto on lisätty (insertion(+)). 

Tämän jälkeen vuorossa on uuden tiedoston puskeminen GitHubiin. Ajan komennon ``$ git push git@github.com:ThomasHelminen/winter-management.git``.

![git-push-komento](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1a7175f8-b86d-49cc-86fe-166517ea09e5)

Yllä olevasta kuvasta voidaan huomata, että kolme tiedostoa kirjoitettiin komennossa, eli uusi tiedosto push-testi.md sekä README.md ja LICENSE). Käydään tarkistamassa GitHubissa, että löytyykö uusi tiedosto sieltä. 

![git-push-ok](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/d6a4ac94-2e7c-43c9-987f-e5fc9869eb29)

Ja löytyyhän se! Tehtävä onnistui oikein.

## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

Tehdään tyhmä muutos. Muokataan push-testi.md -tiedoston sisältöä niin, että siellä näkyy yrityksen luottokortin tiedot ja tallentetaan tiedosto.

Muokkasin alkuperäistä push-testi.md -tiedostoa seuraavan näköiseksi komennolla ``$ nano push-testi.md`` ja lisäsin seuraavan kommentin.

![md-push-väärä-sisältö](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/1271ca15-5c4f-470b-a83b-699acd547328)

Varmistetaan, että sisältö on muuttunut komennolla ``$ cat push-testi.md``.

![väärä-sisältö-komento](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/f37a2097-3be0-4de2-9b5a-0ec1b6120eb3)

Voimme huomata yllä olevasta kuvasta, että tiedoston sisältö on muuttunut. 

Seuraavaksi haluan tuhota äskeiset huonot muutokset komennolla ``$ git reset --hard``.

![git-reset-hard](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/771bde68-111b-4068-8bab-85934ee1113e)

Yllä olevasta kuvasta voidaan huomata, että nollaus on tapahtunut. Nollauksen jälkeen cattasin aiemman tiedoston johon olin tehnyt muutokset, ja voimme huomata, että nollaus onnistui!

## d) Tukki. Tarkastele ja selitä varastosi lokia. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

Tarkastellaan varaston lokia. Käytin tämän tehtävän tekoon git-scm:n git-log [artikkelia](https://git-scm.com/docs/git-log).

Ajan komennon ``$ git log``

![git-log](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/bf26300f-d5be-4a2d-a834-597e67b79bbf)

Komento näytti kaksi lokitapahtumaa. Lokitapahtumissa näkyi mm. Author, eli tekijän nimi sekä sähköpostiosoite (jotka lisättiin aiemmin). Nyt ymmärrän, että nimi- ja sähköpostitiedot ovat pakollisia varmasti juuri sen takia, että lokimuutokset näkyvät oikein. Yllä olevan kuvan lokitapahtumissa voidaan huomata alkuperäinen commit sekä viesti (jonka laitoin ennen kuin puskin tiedostot takaisin GitHubiin). Lokitiedoissa näkyy myös päivämäärä. Sitä en tiedä, miksi toisessa lokitapahtumassa nimi on kirjoitettu yhteen...?

Nämä tehtävät olivat äärimmäisen opettavaisia, sillä oma alkutilanteeni Gitista oli varsin tietämätön ennen näiden tehtävien suorittamista.

## g) Vapaaehtoinen: Se toinen järjestelmä: kokeile Gittiä eri käyttöjärjestelmällä kuin sillä, millä teit muut harjoitukset. Selitä niin, että kyseistä järjestelmää osaamatonkin onnistuu. Mahdollisuuksia on runsaasti: Debian, Fedora, Windows, OSX...

Aloitin suorittamaan tätä tehtävää käynnistämällä isäntäkoneeltani VirtualBoxin (versio. 7.0.12). Starttasin aiemmin tälle kurssille tehdyn virtuaalikoneeni nimeltä "DebianTeroKarvinenCom".

![debian-vb](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/a15c1960-d548-4962-97e6-49ddefae1385)

Koneen käynnistyttyä etsin sovellusvalikosta "Terminal" sovelluksen. 

![debian-terminal](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/611e25be-5004-4a44-a8f1-1a9dd1191a9e)

Terminalin avauduttua otin esille selaimeen GitHub Docs [artikkelin](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux), jossa näkyy ohjeet SSH avaimen tekemiseen sekä yksityisen avaimen lisääminen ssh-agentille. Seurasin ohjeita kohta kohdalta. Aloitin terminaalin käytön komennolla ``$ ssh-keygen -t ed25519 -C "thomas.helminen@hotmail.com"``, joka luo uuden SSH-avainparin.

![Screenshot from 2023-11-11 00-08-42](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/ab0c4df4-eb30-4e4f-a379-32b813c520a2)

Kuvasta näkyy mihin sijaintiin avainpari on luotu. Seuraavana vaiheessa oli käynnistää ssh-agentti, johon lisätään oma yksityinen avain. Ajoin ensiksi komennon ``$ exec ssh-agent bash``, jonka jälkeen komennon ``$ eval "$(ssh-agent -s)"``, joka käynnisti kyseisen agentin. Tämän jälkeen lisäsin yksityisen avaimen agenttiin komennolla ``$ ssh-add ~/.ssh/id_ed25519``. Alla olevasta kuvasta voi huomata, että identiteetti lisättiin.

![Screenshot from 2023-11-11 00-09-47](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/3ca33e5f-56e5-4486-add9-4035687def5e)
 
Yksityisen avaimen lisäyksen jälkeen avasin julkisen avaimen sisällön komennolla ``$ cat id_ed25519.pub``. Kopioin tämän avaimen sisällön ja kirjauduin virtuaalikoneella GitHubiin. Kävin lisäämässä GItHubin asetuksissa kyseisen julkisen avaimen SSH-avaimeksi.

![Screenshot from 2023-11-11 00-10-34](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/eb9be956-4ee8-4d24-b6a6-7b8ef0045d63)

Kun sain lisättyä julkisen avaimen GitHubiini, niin vuorossa oli asentaa Git Debianille. Varmistin Gitin asennuskomennon [täältä](https://git-scm.com/download/linux). Ajoin komennon ``$ sudo apt install git`` joka asensi Gitin virtuaalikoneelle.

![Screenshot from 2023-11-11 00-13-26](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/524351ba-a34a-4ce0-b305-82196650b124)

Gitin asennuksen jälkeen kloonasin winter-management varaston virtuaalikoneelle komennolla ``$ git clone git@github.com:ThomasHelminen/winter-management.git``. Kloonauksen jälkeen siirryin varastoon komennolla ``$ cd winter-management/`` ja siellä loin uuden nanotiedoston nimeltä debian-testi.md komennolla ``$ nano debian-testi.md``. Tiedoston luonnin jälkeen listasin vielä varaston sisällön komennolla ``$ ls``, jotta pystyin varmistumaan, että tiedosto luotiin.

![Screenshot from 2023-11-11 00-14-38](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/602b3221-4ec3-4c64-b80b-cba36d4c1757)

Sitten oli aika aloittaa työntämään uutta tiedostoa GitHubiini. Eli ajoin alla olevassa kuvassa seuraavia komentoja:
  1. ``$ git add .``
  2. ``$ git commit -m "Työntötesti Debianilla"``, jonka jälkeen nimi- ja sähköpostitiedot konfiguroitiin automaattisesti.

![Screenshot from 2023-11-11 00-15-10](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/7330452e-9599-4be1-8a18-1fb39799ed9f)

Nyt kun työntäminen oltiin valmisteltu, suoritin viimeisen työntökomennon ``$ git push git@github.com:ThomasHelminen/winter-management.git`` joka työnsi uuden tiedoston GitHubiini.

![Screenshot from 2023-11-11 00-15-50](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/59324342-8b73-4718-906f-e4818881da1c)

Lopuksi päivitin kyseisen GitHub varaston verkkosivun virtuaalikoneen selaimessa, ja päivittämisen jälkeen iloikseni huomasin, että debian-testi.md -tiedosto löytyi sieltä!

![Screenshot from 2023-11-11 00-16-56](https://github.com/ThomasHelminen/Palvelinten-hallinta--kurssi/assets/148875548/00286d2e-b77d-4dbf-b33b-4be9239bf74c)



------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Lähdeluettelo
-  Atlassian. s.a. What is Git? Luettavissa: https://www.atlassian.com/git/tutorials/what-is-git. Luettu 10.11.2023.
-  Atlassian, Bitbucket Support. s.a. Add, edit, and commit to source files. Luettavissa: https://support.atlassian.com/bitbucket-cloud/docs/add-edit-and-commit-to-source-files/. Luettu: 10.11.2023.
-  GitHub Inc. s.a. Cloning a repository. Luettavissa: https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository. Luettu 10.11.2023.
-  GitHub Inc. s.a. Generating a new SSH key and adding it to the ssh-agent. Luettavissa: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent. Luettu 10.11.2023.
-  GitHub Inc. s.a. Generating a new SSH key and adding it to the ssh-agent (Linux). Luettavissa: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux. Luettu 11.11.2023.
-  GitHub Inc. s.a. Testing your SSH connection. Luettavissa: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection. Luettu 10.11.2023.
-  Git SCM. s.a. Download for Linux and Unix. Luettavissa: https://git-scm.com/download/linux. Luettu 11.11.2023.
-  Git SCM. s.a. Download for Windows. Luettavissa: https://git-scm.com/download/win. Luettu 10.11.2023.
-  Git SCM. 2.11.2023. git-log Documentation. Luettavissa: https://git-scm.com/docs/git-log. Luettu 10.11.2023.
-  Karvinen, T. 13.10.2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu 11.11.2023.
-  TheServerSide. McKenzie, C. 11.1.2022. How to SSH into GitHub on Windows example. Luettavissa: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/GitHub-SSH-Windows-Example. Luettu 10.11.2023.
-  TheServerSide. McKenzie, C. 29.8.2023. Luettavissa: https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-push-an-existing-project-to-GitHub. Luettu 10.11.2023.
