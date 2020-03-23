---
path: '/osa-2/5-verkko-ohjelmointi-rajapinta'
title: 'Verkko-ohjelmointi ja sovelluksen rajapinta'
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Tiedät millaisia rajapintoja verkkosovellukset käyttävät.
- Osaat kertoa mikä on pistoke ja kuvata yleisellä tasolla miten sitä käytetään.

</text-box>

## Verkkosovelluksen rajapinta

Perinteisesti internetin protokollapinon sovelluskerroksella on ollut erilaisia verkkosovelluksia, jotka ovat käyttäneet kuljetuskerroksen tarjoamia palveluja erilaisten kirjastorajapintojen kautta, kuten C:n ja C++:n socket.h tai JAVAn java.net. Kirjasto tarjoaa ohjelmoilla joukon funktioita, metodeja yms, joiten avulla verkkosovellus osien välinen tietoliikenne voidaan toteuttaa. Nämä kirjastot käyttävät sitten edelleen käyttöjärjestelmän palveluja erityisten [pistokkeiden](https://fi.wikipedia.org/wiki/Pistoke_(tietotekniikka)) (engl. socket) kautta.

Pistokkeet ovat siis keino välittää dataa (eli viestejä) sovelluskerroksen ja kuljetuskerroksen välillä. Tällä kurssilla tutustutaan yleisesti kuljetuskerroksen tarjoamien palvelujen käyttöön pistokkeilla. Varsinaista verkko-ohjelmointia emme tällä kurssilla opettele. Helsingin yliopiston tutkinto-opiskelijat voivat osallistua valinnaiselle kurssille [Network Programming](https://courses.helsinki.fi/fi/tkt21026), jossa opiskellaan verkkosovellusten tekoa pistokkeita käyttäen.

Toisaalta nykyään yhä useampi verkkopalvelu toteutetaankin selaimen kautta käytettävänä www-palveluna. Nämä palvelut käyttävätkin sovelluskerroksen omaa HTTP-protokolla sovelluksen eri osien väliseen tiedonsiirtoon.

Ne siis muodostavat ovat kerroksensa HTTP-protokollan päälle ja kasvattavat näin koko järjestelmän abstrakiotasoa, kun kaikki HTTP:n alapulella oleva voidaan jättää huomioimatta. Tällaisen www-palvelun päällä toimivat websovelluksen tietoliikenne tapahtuu HTTP-protokollalla, kuten selaimen ja www-palvelimen välinen liikenne normaalistikin tapahtuu. Selain siis noutaa tällaisen selaimessa suoritettavan 'verkkosivun' www-palvelimelta ja tarvittavat viestien vaihdot selaimessa toimivan asiakasosan ja palvelimella toimivan palvelinsovelluksen välillä tapahtuu HTTP-viesteinä. Jos tämä teema kiinnostaa, niin silloin kannattaa seuraavaksi suunnata kurssille [web-palvelinohjelmointi](https://courses.helsinki.fi/fi/tkt21007) tai [full stack -websovelluskehitys](https://courses.helsinki.fi/fi/tkt21009), jossa opit nimenomaan tekemään sovelluksia web-ympäristössä.

Jotta abstraktiotasoa voidaan nostaa ja käyttää HTTP:tä kuljetuspalveluna, niin täytyy määritellä jonkunlainen rajapinta (tai vähinkäänkin kuvata tapa viestiä HTTP:n kautta). Tämän mukaan sitten sovelluksen eri osat käyttävät samalla tavalla HTTP:tä viestien kuljetukseen. Esimerkiksi [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) kuvaa erilaisia sääntöjä joita verkkosovelluksen toteuksessa voidaan noudattaa.

Huomaa, että valitaanpa verkkosovelluksen kuljetuspalveluksi sovelluskerroksen HTTP tai kuljetuskerroksen TCP tai UDP, verkkosovelluksen osia suoritetaan vain verkon reunoilla olevissa päätelaitteissa, kuten käyttäjän tietokone ja palvelinkeskusten palvelimet. Verkkosovelluksen ohjelmoijan pitääkin kirjoittaa ohjelmakoodi vain näille laitteille. Verkon syövereissä olevissa laitteissa ei ole sovelluskerrosta, joten ne eivät voi verkkosovelluksen osia suorittaa, eikä niiden toiminta muutu sovelluksen vaihtuessa. Koska me tällä kurssilla keskitymme nimenomaan päälaitteiden väliseen viestiin, jää varsinaisten verkkosovellusten oman sisäinen toimina muille kurssille.

<img src="../img/lahettaja-reititin-vastaanottaja.svg" alt="Kuva jossa viesti kulkee protokollapinon läpi reitillä lähettäjä-reititin-reititin-vastaanottaja">

KUVA: Peruskuva protokollapinosta lähettäjän ja vastaanottajan välillä


## Pistokkeet

Verkkosovellusta koneella edustava prosessi käyttää kuljetuskerroksen palveluja pistokkeiden kautta. Näin tapahtuu silloinkin, kun sovellus itse käyttää kuljetuspalveluna HTTP:tä. Tällöin prosessin sisäinen HTTP:n toteutus käyttää pistokkeita sovelluksen puolesta. Pistoketta voi ajatella ovena tai postilaatikkona, jonka kautta sovellusprosessi lähettää ja vastaanottaa viestejä. Sovelluksen ei tarvitse tietää, miten viesti kulkee verkkosovelluksen prosessien välillä ovelta toiselle tai postilaatikosta toiseen. Lähettäjän täytyy vain luottaa, että 'oven' takana oleva kuljetuspalvelu toimittaa viestin perille.

Ohjelmoijan ja sovellusohjelman näkökulmasta pistokkeen käyttö muistuttaa tiedoston käyttöä, sinne kirjoitetaan ja sieltä luetaan. Pistokkeiden kanssa tosin käytetään termejä lähettää (engl. send) ja vastaanottaa (eng. receive).

Pistokkeita voidaan käyttää prosessien välisessä kommunikoinnissa myös samassa tietokoneessa. Pistokkeiden toteutuksessa mikään ei vaadi, että viestit on aina välitettävä koneesta toiseen. Ne voidaan hyvin välittää prosessilta toiselle samassa koneessa. (Itse asiassa kaikissa koneissa on oman sisäinen verkko-osoite 127.0.0.1, johon lähetetyt viestit päätyvät samalla koneelle, mutta kiertävät protokollapinossa verkkokerroksen kautta.)


Tarkastellaan nyt pistoketta käyttöjärjestelmän näkökulmasta eli mitä tapahtuu, kun prosessi haluaa lähettää dataa.
1) Prosessi pyytää kuljetuspalvelua KJ:n palvelupyynnöllä send (kirjastofunktio)
2) Kuljetuskerros hoitaa omat tehtävänsä ja kutsuu verkkokerroksen rutiinia
3) Verkkokerros tekee hommansa ja kutsuu laiteajurin rutiinia
4) Laiteajuri vie datan ja komennot verkkokortin ohjaimen rekistereihin
5) Verkkokortti siirtää bitit linkille (linkkikerros ja fyysinen siirto)

Lähettäminen on selkeää ja yksinkertaista, koska sovellus päättää, koska lähetetään ja kutsuu kirjastorutiinia, joka sitten käyttöjärjestelmän toimintoina huolehtii varsinaisesta lähettämisestä. Viestiä ei välttämättä pystytä heti laittamaan eteenpäin, vaan se laitetaan ensin lähetyspuskuriin (engl. send buffer), josta viesti sitten lähetetään, kun verkossa on tilaa.

Vastaanottaminen on haasteellisempaa, koska sovellus tekee vastaanottopyynnön (receive) silloin, kun se sovelluksen oman toiminnan kannalta on tarpeellista. Huomaa erityisesti, että se ei ole mitenkään sidottu varsinaiseen viestin saapumiseen. Viesti on voinut saapua jo ennenkuin prosessi tekee vastaanottopyynnön tai viesti voi saapua paljon myöhemmin. Siksi meillä on pistoke, jota käyttäen prosessi tai viesti voi odottaa. Viesti siis odottaa, että prosessi haluaa lukea sen. Prosessi voi halutessaan jäädä odottamaan kunnes viesti saapuu. Sovellus voi pistokkeen avaamisen yhteydessä määritellä, haluaako se receive-pyynnön yhteydessä jäädä odottamaan viestiä vai ei (engl. blocking vs. non-blocking).

Vastaanottava laite (tai sen prosessi) ei voi vaikuttaa viestin saapumisen ajankohtaan tai saapuvan viestin kokoon. Sen pitää ottaa viesti vastaan silloin, kun se on verkkoyhteydeltä tulossa.  Viesti siis vastaanotetaan ensin vastaanottopuskuriin (engl. receive buffer), josta se voidaan antaa sovellukselle silloin, kun sovellus sitä pyytää.

Viestin vastaanotto vaiheittain:
1) Verkkokortti ottaa vastaan linkiltä tulevat bitit (fyysinen siirto ja linkkikerros)
2) ...ja aiheuttaa keskeytyksen.
3) KJ:n laiteajuri siirtää bitit verkkokortilta keskusmuistiin
4) Ajuri kutsuu verkkokerroksen rutiinia, joka suorittaa omat toimintonsa
5) Verkkokerros kutsuu kuljetuskerroksen rutiinia, joka tekee omat toimensa
6) Sanoma prosessille vasta, kun se sitä pyytää palvelupyynnöllä receive

Pistoke on aina kaksisuuntainen eli samalla pistokkeella sekä lähetetään että vastaanotetaan. Pistoke on siis prosessin yhteyspiste toisen prosessin kanssa. Molemmilla kommunikointiin osallistuvilla prosesseilla on omat pistokkeensa, jotka ovat yhteydessä toisiinsa. Yhdellä prosessilla voi olla samanaikaisesti käytössä useita pistokkeita eri yhteyksiä varten.

Kuljetuskerros tarjoaa yhteydellisen kuljetuspalvelun TCP-protokollalla ja yhteydettömän UDP-protokollalla. Palvelun mukaan pistokkeet voivat vastaavasti olla joko tavuvirtapistokkeita (engl. stream socket) tai tietosähkepistokkeita (engl. datagram socket). TCP liittyy tavuvirtapistokkeisiin ja UDP tietosähkepistokkeisiin.

## Päästä-päähän yhteys

Prosessin verkkoyhteys on sidottu tiettyyn pistokkeeseen, joka on sidottu kuljetuskerroksen käyttämään porttinumeroon. Kuljetuskerros tunnistaa yhteyden saapuvat viestit, sen otsakkeessa olevalla porttinumeroilla.  Lähtevässä viestissä pitää siksi aina olla verkkokerroksen käyttämän vastaanottajan IP-numeron lisäksi kuljetuskerroksenn tarvitsema porttinumero. Saapuvan viestin porttinumerolla kuljetuskerros tunnistaa oikean pistokkeen ja osaa välittää viestin oikealle prosessille. Prosessilla voi olla samaan aikaan käytössä useita pistokkeita, jolloin sen käytössä on myös useita porttinumeroita, koska jokaisella pistokkeella on oltava yksilöivä tunniste eli porttinumero.


<img src="../img/2-5-pistokekuva.png"  alt="Kuvassa on kaksi laitetta, joista ensimmäisellä on useita prosesseja, joihin on liitetty pistoke per prosessi. Ne kaikki kommunikoivat toisella laitteella olevan yhden prosessin kanssa, johon on myös liitetty vain yksi pistoke.">
KUVA: Kuvassa on prosesseja ja niihin liitettyjä pistokkeita. Prosessilla voi olla useita pistokkeita, vaikka tässä kuvassa niitä onkin vain yksi. Kukin pistoke on kuvattuna sitä vastaavalla porttinumerolla.  Kaikki asiakas koneen prosesit kommunikoivat saman palvelinlaitteella toimivan palvelinprosessin kanssa.  Kuljetuskerros tunnistaa jokaisen prosessin pistokkeen niihin liitetystä porttinumerosta. Muistathan, että porttinumero yksilö prosessin vain yhden laitteen sisällä eli eri laitteilla olevat prosessit voivat käyttää samoja porttinumeroita. TODO svg

Osa porttinumeroista on varattu tietyille tunnetuille palveluille. [Internet Assigned Numbers Authority (IANA)](https://www.iana.org), hallinnoi internetin IP-numeroita ja myös erilaisten palvelujen porttinumeroita. IANA ylläpitää myös muuta internetin ja sen protokollien käyttöön liittyvää tietoa.



<quiz id="a9f3871a-8756-453a-84d2-e0a8c37ea170"></quiz>



## Pistokerajapinnan socket.h keskeiset funktiot

Sovellusohjelmassa pisto pitää ensin luoda (socket). TCP:n kanssa käytettävät tavuvirtapistokkeet pitää lisäksi yhdistää kommunikoinnin vastapuoleen. Tämä on erilainen asiakkaalla (connect) ja palvelimella (bind, listen ja accept). Sen jälkeen pistokkeen avulla voi lähettää (send) ja vastaanottaa (receive) viestejä. Lopuksi pistoke pitää vielä sulkea (close). UDP:n kanssa käytettävät tietosähkepistokkeet eivät edellytä pistokkeiden yhdistämistä, mutta lähetys- ja vastaanotto-operaatioissa pitää kuljettaa mukana tietoa kommunikoinnin vastapuolesta, siksi niillä on omat lähetys (sendto) ja vastaanotto (receivefrom) funktiot. Huomaa, että jo pistokkeen avauksessa pitää päättää kummanlaisen pistokkeen avaa ja sitten sitä pitää käyttää kyseisellä tavalla. Käyttötapoja ei siis pidä sekoittaa.

<img src="../img/2-5-pistoke-asiakas-palvelin.svg">

KUVA: Kuvassa on TCP:n kanssa käytettävien tavuvirtapistokkeiden funktiot. Lisäksi kuvaan on piirretty TCP:n yhdeyden muodostukseen liittyvät viestit (conn.req ja conn.ack), esimerkin omaisesti kaksi data viestiä ja yhteyden purkamiseen liittyvä viestit (disconn.req ja disconn.ack). TCP-yhteys tulee seuraavassa luvussa tarkemmin.

Tässä vielä esimerkiksi toimiva python koodi, jolla voit halutessasi kokeilla verkko-ohjelman toimintaa.

Asiakkaan koodi:
```java
from socket import *
serverName = ’servername’
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName,serverPort))
viesti = raw_input(‘Kirjoita lähetettävä viesti: ’)
clientSocket.send(viesti)
muokattuviesti = clientSocket.recv(1600)
print ‘Palvelimen vastaus: ’, muokattuviesti
clientSocket.close()
```
Palvelijan koodi:
```java
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET,SOCK_STREAM)
serverSocket.bind((‘’,serverPort))
serverSocket.listen(1)
print ‘Palvelija on valmiina’
while 1:
     connectionSocket, addr = serverSocket.accept()

     viesti = connectionSocket.recv(1600)
     muunnettuviesti = sentence.upper()
     connectionSocket.send(muunnettuviesti)
     connectionSocket.close()
```

Jos verkko-ohjelmointi kiinnostaa enemmänkin, niin voit täysin ylimääräisenä materiaalina tutustua erilaisiin verkosta löytyviin esimerkkiohjelmiin. Oheiset linkit vievät englanninkielisille sivuille:
1) Pistokkeet pythonissa: https://realpython.com/python-sockets/
2) Pistokkeet Javassa: https://docs.oracle.com/javase/tutorial/networking/sockets/index.html
3) Ray Toalin kurssimateriaalissa olevia esimerkkejä javalle: https://cs.lmu.edu/~ray/notes/javanetexamples/ liittyvät kurssiin https://cs.lmu.edu/~ray/classes/it/


