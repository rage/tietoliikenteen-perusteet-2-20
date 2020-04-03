---
path: '/osa-6/1-kaikki-yhteen'
title: 'Kaikki yhteen'
hidden: true
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata aiempien osien materiaalin yhteistoiminnan.

</text-box>

Tässä osassa käydään läpi yksi kokoava esimerkki, jossa käydään erityisesti läpi, miten kurssin kuluessa käsitellyt menetelmät toimivat yhdessä. Kun käyt tätä läpi, niin kertaa samalla kaikkien protokollien yksityiskohdat.


Käydään esimerkkinä läpi tilanne, jossa käyttäjä liittää oman koneensa aliverkkoon langallisella yhteydellä ja tekee sen jälkeen www-selaimella pyynnön lukea joku tietty sivu.  Tämä aliverkko voi olla kotiverkko tai esimerkiksi jonkun yrityksen hallinnoima oma sisäinen verkko.

Tämä esimerkki tiivistää suurimman osan kurssin sisällöstä yhteen tarinaan. Jotkut protokollat jäävät vielä pois tarinasta ja osasta ei tarvita kaikkia yksityiskohtia, mutta suurin osa kurssisisällöstä tulee kerrattua tällä hyvin yksinkertaisella tarinalla.

<img src="../img/kaikki-yhteen-verkko.svg"> </img>

## Verkkoon liittyminen

Verkkoon liitetty tietokone ei voi liikennöidä verkossa, jos sillä ei ole verkkoon sopivia tunnisteita, joilla muut voivat lähettää sille viestejä.

<quiz id="a69e0cef-84ae-4ad8-8c28-dc408c892e72"> </quiz>

Jotta sovelluskerroksella muodostetty pyyntö voidaan välittää verkkoon, niin sen pitää edetä protokollapinossa kerrokselta toiselle. Jätetään tuo kerrosten tarkastelu hetkiseksi ja mennään eteenpäin. Muista kuitenkin, että kaikki lähetetyt ja vastaanotetut viestit kulkevat aina protokollapinossa kerrokselta toiselle.

Kun tietokone on liittynyt verkkoon, se on valmiina lähettämään ja vastaanottamaan viestejä.

Seuraavaksi käyttäjä käynnistää www-selaimen ja kirjoittaa osoitekenttään sen www-palvelimen nimen, jonka pääsivun käyttäjä haluaa hakea. Oletamme tässä että kyseisen palvelimen nimi on www.verkkotunnus.fi   (Tämä osoite vie itseasiassa Traficomin sivulle, joten tunnus on aidosti olemassa.)

Selain siis lähtee seuraavaksi hakemaan kyseistä www-sivua.

## Kyselyn alussa tietojen selvittäminen

Selaimen pitää muodostaa HTTP-pyyntö, mutta ennen sitä sen täytyy selvittää kyseistä verkkonimeä www.verkkotunnus.fi vastaava IP-osoite.

Niinpä selain tekee pyynnön nimipalvelijalle, joka osoitteen se on jo saanut aiemmin. Sovelluskerrokselta nimipalvelukysely siirtyy kuljetuskerrokselle. Nimipalvelupyyntö käyttää UDP:tä kuljetuskerroksen protokollana, joten erillistä yhteydenmuodostusta ei tehdä.  Kuljetuskerros muodostaa pyynnöstä segementin ja antaa sen edelleen verkkokerrokselle välitettäväksi eteenpäin.
Verkkokerros paketoi segmentin omaan IP-datagrammiinsa ja antaa datagrammin linkkikerrokselle.

Verkkokerroksen datagrammissa on vastaanottajan IP-osoite. Koska vastaanottaja ei ole samassa aliverkossa täytyy verkkokerroksen ohjata viesti reititystaulun mukaisesti oletusyhdyskäytävälle. Tämän IP-osoitteen verkkokerros tietää, mutta linkkikerros tarvitsee tätä IP-osoitetta vastaavan MAC-osoitteen. Tämä protokolla määritellään välillä verkkokerrokselle ja välillä linkkikerrokselle kuuluvaksi.

<quiz id="a50903df-836c-422d-95ae-da29218e3448">  </quiz>

Nyt meillä on tarvittavat tiedot siihen, että linkkikerros voi sijoittaa saamansa IP-datagrammin Ethernet-kehykseen ja lähettää sen paikalliselle yhdyskäytäväreitittimelle.

Muistathan vielä, että kehyksen sisällä olevan datagrammin sisällä olevassa UDP-segmentissä on paikalliselle nimipalvelijalle menossa oleva kysely.

Yhdyskäytäväreititin vastaanottaa verkkokerroksella sille linkkikerroksen kautta ohjatun nimipalvelukysely. Se tarkistaa datagrammista mihin osoitteeseen viesti on menossa ja välittää sen reititystaulunsa tietojen perusteella eteenpäin. Näin viesti liikkuu reitittimeltä toiselle, kunnes se saapuu nimipalvelijan kanssa samassa aliverkossa olevalle reitittimelle. Tämä reititin ohjaa viestin palveiljalle.

Palvelijalla linkkikerros vastaanottaa tälle laitteelle tulossa olevan viestin ja antaa sen verkkokerrokselle. Verkkokerros tarkistaa, että viesti on nyt perillä oikealla laittella, eli että vastaanotetuss datagrammissa on vastaanottajana tämän laitteen IP-osoite. Verkkokerros antaa viestin kuljetuskerroksen UDP-protokollalle, joka segmentin porttinumeron perusteella osaa antaa viestin edel"leen nimipalveluprosessille.

<quiz id="abb2ab02-88ba-465d-9d3b-e2f7d709d64b"> </quiz>

Nimipalveluprosessi selvittää ensin omasta välimuististaan tietääkö se jo valmiiksi vastauksen saamaansa kyselyyn.  Jos pyydettyä tietoa ei ole sen omassa välimuistissa, niin se lähtee kysymään tietoa virallisen nimipalvelijoiden hierarkialta. Ja saatuaan vastauksen lisää sen omaan välimuistiinsa. Oletetaan tässä yksikertaisuuden vuoksi, että tieto löytyy suoraan välimuistista.

## Itse kysely

