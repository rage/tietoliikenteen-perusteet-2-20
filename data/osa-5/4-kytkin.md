---
path: '/osa-5/4-kytkin'
title: 'Kytkin'
hidden: false
---


<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata kytkimen toiminnan ja miten se eroaa reitittimen ja keskittimen/toistimen toiminnasta
- Osaat kertoa miten kytkin pitää kirjaa aliverkon laitteista ja miten se tietää missä osassa aliverkko laite sijaitsee.

</text-box>


## Keskitin, kytkin ja reititin

Kytkin, keskitin ja reititin on valitettavan helppo sekoittaa toisiinsa, koska niitä kaikkia käytetään yhdistämään tietoliikenneverkon eri osia toisiinsa. Muistathan, että reititin toimii verkkokerroksella. Tästä eri kerroksilla toimimisista on se seuraus, että kun reititin vastaanottaa paketin se purkaa siitä pois linkkikerroksen kehyksen ja tarkastelee esiin kuorittua IP-pakettia. Sen jälkeen se lähettää paketin edelleen haluttuun suuntaan. Tässä vaiheessa IP-paketti kääritään uuteen linkkikerroksen kehykseen, jossa on tämän uuden kehyksen lähettäjän ja vastaanottajan osoitteet.

[Kytkin](https://fi.wikipedia.org/wiki/Kytkin_(tietoliikenne)) toimii linkkikerroksella, joten se käsittelee aina linkkikerroksen kehystä. Se siis toimii kehyksen otsakkeesta saamiensa tietojen mukaan. Näin ollen kytkin osaa tunnistaa laitteiden MAC-osoitteita, mutta ei IP-osoitteita. Koska kytkin sanana suomenkielessä on käytössä sekä valokytkimissä että auton vaihteistossa, niin tarvittaessa voidaan käyttää termiä tietoliikennekytkin tai verkkokytkin erotuksena näistä sanan muista käyttöyhteyksistä. Tietoliikennealan sisällä kuitenkin puhutaan yksinkertaisesti kytkimistä.

[Keskitin](https://fi.wikipedia.org/wiki/Keskitin) on usemmasta toistimesta muodostettu fyysisen kerroksen laite, joka yksinkertaisimmillaan vain toistaa yhdestä portista saamansa tiedon kaikkiin muihin portteihin. Tällaista viestin toistamista tarvitaan, koska pitkillä siirtoyhteyksillä signaalit vaimenevat, eivätkä ne välttämättä olisi enää perillä tunnistettavia. Matkan varrelle sijoitetut toistimet (engl. repeater) generoivat saamansa signaalin uudelleen ja näin vahvistavat sen tasoja ja samalla poistavat häiritsevää taustakohinaa.

Keskitin siis toistaa saamansa signaalit sellaisenaan kaikkiin ulosmeneviin yhteyksiin. Se ei voi muuttaa signaalin nopeutta, joten se soveltuu yhdistämään vain saman teknologian samalla nopeudella toimivia verkon osia toisiinsa. Keskittimet eivät näy linkkikerrokselle, joten ne voivat pidentää viestin kulkuaikaa kyseisellä yhteydellä ja näin lisätä etenemisviivettä kahden ko. yhteyttä käyttävän solmun välillä.


## Kytkin

Kytkin voi suodattaa liikennettä linkkikerroksen MAC-osoitteiden perusteella. Toisin kuin keskitin, niin se voi suodattaa (engl. filter) sen läpi kulkevia viestejä ja lähettää viestin vain siihen porttiin, jossa se vastaanottajan MAC-osoitteen perusteella tietää vastaanottajan olevan. Lisäksi useimmat kytkimet osaavat myös tilapäisesti puskuroida viestejä, jolloin ne voivat liikennöidä eri solmujen kanssa eri nopeuksilla, mutta kuitenkin samaa teknologiaa käyttäen, koska kytkin ei muuta kehystä millään tavalla.

Verkkokerroksen (eli reitittimien ja verkon ulkoreunoilla olevien solmujen) kannalta kytkimen toiminta on ihan yhtä näkymätöntä kuin keskittimien ja toistimien toiminta on näkymätöntä kytkimelle.

Koska kytkin suodattaa kehyksiä ja ohjaa saapuvan kehyksen vain siihen porttiin, jossa kehyksen vastaanottaja on, se pitää kirjaa näistä.  Sillä on oma kytkintaulu (engl. switch table), jonka se voi katsoa, mihin porttiin kehys pitää ohjata. Kytkin käyttää kehyksen ohjaukseen vastaanottajan MAC-osoitetta. Sillä on siis omassa kytkintaulussaan (MAC-osoite, porttinumero) -pareja, joista se voi tarkistaa, mihin suuntaa kyseinen kehys pitää ohjata.

Koska kytkimen pitää välittää kehykset samantien, niin sillä ei ole aikaa kysyä, missä päin verkkoa tietty MAC-osoite sijaitsee. Jos se ei löydä tietoa kytkintaulusta, niin se toimii kuten keskitin ja lähettää kehyksen kaikkiin portteihin, paitsi sinne mistä kehys tuli. Näin se tulvittaa (engl. flooding) kyseisen kehyksen kaikkialle verkossa ja voi olla varma, että vastaanottaja saa sen.

Omaa kytkintauluaan se päivittää saapuvien kehysten tiedoilla. Aina, kun sille saapuu välitettävä kehys, se tarkistaa kytkintaulusta vastaanottajan osoitteen lisäksi myös lähettäjän MAC-osoitteen. Jos lähettäjän MAC-osoitetta ei löydy kytkintaulusta, niin kytkin lisää tauluun lähettäjän MAC-osoitteen ja sen portin numeron, josta kehys saapui. Näin se oppii liikennettä seuraamalla, minkä portissa takanat tietyt MAC-osoitteet ovat. Tästä käytetään joskus termiä 'takaperin oppiminen' (engl. backward learning) korostamaan sitä, että toisin kuin yleensä tässä tarkastellaan lähettäjän tietoja ja päivitetään taulua niiden perusteella. Yleensä tietoliikenteessä ollaan kiinnostuneita vain vastaanottajasta, mutta kytkintaulun päivityksessä kiinnostavaa on nimenomaan lähettäjä.

Tästä vaiheittaisesta kytkitaulun päivityksestä on se hyöty, että kytkimeen voidaan helposti liittää uusia laitteita ja kytkin oppii itsenäisesti, missä portissa laite on. Jotta kytkintaulu pysyisi riittävän pienenä, niin kytkin poistaa kytkitaulusta käyttämättömiä tietoja, eli jos tietystä MAC-osoitteesta ei ole vähään aikaan liikennöity, niin se poistetaan.

Kytkin siis käsittelee kytkintauluaan seuraavasti aina kun sille saapuu kehys
- Onko lähettäjän MAC-osoite taulussa?
    on: päivitä aikaleima
    ei: lisää tauluun (MAC, porttinumero, aikaleima)
- Onko vastaanottajan MAC-osoite taulussa?
    ei:  tulvita (eli lähetä kaikkialle muualle paitsi tuloporttiin)
    on: Jos portti eri kuin tuloportti, niin lähetä ko. porttiin. Jos portti sama kuin tuloportti, niin unohda.

 <img src="../img/5-4-tahti.svg" alt="Kuvassa on tähtitopologiaan perustuva verkko, jonka keskipisteessä on kytkin. Kytkimeen tulee ainakin neljä yhteyttä, joista kaksi tulee suoraan solmulta ja kahdessa kytkin on ensin liitetty keskittimeen, joista toiseen on liitetty 2 ja toiseen 3 solmua. Näin meillä on verkon reunoilla kaikkiaan 7 laitetta ja keskemmällä kaksi keskitintä ja yksi kytkin. A ja B on kiinni keskittimessä H1. H1 ja C on yhdistetty kytkimeen K, H1 porttiin 1 ja C porttiin 2. Toisella puolella kytkintä portissa 3 on H2, jossa on kiinni solmut E,F ja G. Lisäksi kytkimessä on vielä kiinni solmu I portissa 4.">

KUVA: Kuvassa on yksi aliverkko, jossa on 1 kytkin ja 2 keskitintä. Keskittimet välittävät kaiken liikenteen niiden läpi ja kaikkiin niihin liitettyihin laitteisiin. Kytkin osaa eristää liikennettä linkkikerroksella.


Käydään läpi esimerkki tähän kuvaan ja sen solmuihin liittyen. Oletetaan ensin, että kytkimen kytkentätaulu on tyhjä. Solmu A lähettää nyt kehyksen solmulle B. A:n lähettämä kehys päätyy ensin keskittimelle, joka välittää sen kaikkiin suuntiin. Näin kehys päätyy sekä B:lle että kytkimelle. Koska kehys tulee kytkimelle, niin kytkin lisää kytkentätauluunsa, että solmu A on kytkimen portin 1 takana. Näin kytkin osaa jatkossa välittää viestejä A:lle. Koska B:tä ei vielä löydy kytkimen kytkentätaulusta, niin kytkin välittää kyseisen kehyksen kaikkiin muihin suuntiin paitsi kytkimen porttiin 1, josta viesti tuli.  Kun B vastaa A:lle, niin viesti kulkee taas keskittimen H1 kautta ja päätyy sekä A:lle että kytkimelle. Nyt kytkin havaitsee omasta kytkentätaulustaan, että A on samassa portissa, josta viesti tuli, joten kytkin ei välitä viestiä minnekään.

Jos nyt seuraavaksi E lähettää kehyksen B:lle, niin kehys päätyy kytkimelle keskittimen H2 kautta. Kun kytkin saa viestin, se lisää E:n omaan kytkentätauluunsa ja havaitsee taulusta, että viesti B:lle pitää laittaa porttiin 1. Kytkin siis lähettää viestin vain porttiin 1, jolloin kehys päätyy B:lle keskittimen H1 kautta. Koska B löytyi kytkintaulusta, niin kehystä ei tulviteta, joten sitä ei lähetetä muihin portteihin.

<quiz id="ac60ec2c-8945-4296-be47-e3de30133428"> <quiz>

Kehysten tulvituksen liittyy aina riski siitä, että kehys jää ikuisesti kiertämään verkossa. Näin voi tapahtua, jos verkon rakenteessa on silmukka. Tehdään tuohon äskeiseen esimerkkiin silmukka esimerkiksi yhdistämällä keskittimet H1 ja H2 toisiinsa joko suoraan tai uuden kytkimen avulla. Nyt jos kytkin lähettää kehyksen A:lle porttiin 1, niin keskittimet toistavat viestin ja se päätyy takaisin kytkimelle portista 3. Kytkin siis lähettää viestin uudelleen porttiin 1 ja näin meillä on ikuisesti verkossa liikkuva viesti. Tällainen silmukka pudottaa tehokkaasti verkon tehokkuutta, kun suurin osa siirrettävistä kehyksistä on näitä turhia jo moneenkertaan lähetettyjä.

Voimme välttyä silmukoilta muodostamalla tällaiseen verkkoon virittävän puun, jota pitkin kytkimet voivat siirtää kehyksiä. Tämä voidaan tehdä esimerkiksi reitityksen yhteydessä käsitellyllä linkkitila-algoritmilla. Silmukka on ihan yhtä hankala, onpa se reitittimien muodostamassa verkossa tai kytkinten muodostamassa verkossa. Samat periaatteet pätevät niihin molempiin. Verkkoja tutkitaan myös hyvin teoreettisesti, jotta niiden tällaiset yleiset ominaisuudet voidaan havaita ja niiden ratkaisemiseen kehittää yleisiä algoritmeja.

Jos käytössä on puhdas tähtitopologiaan perustuva verkko, niin sellaisessa ei silmukoita voi olla. Usein kuitenkin verkkojen ylläpitäjät lisäävät verkkoihin varareitteja, jotta verkon vikaantuessa vaihtoehtoisten reittien käyttöönotto olisi nopeampaa. Väärin konfiguroituna näistä varareiteistä voi valitettavasti muodostua verkon toimintaa haittaavia silmukoita.

Kytkimien ohessa saatetaan joskus puhua myös [silloista](https://fi.wikipedia.org/wiki/Silta_(tietoliikenne)). Kytkin on internetiin ja erityisesti ethernet-verkkoihin liittyvä termi. Silta on mikä tahansa kahta verkkoa linkkikerroksella yhdistävä laite, mutta nykyisin kun käytetään lähinnä ethernet-verkkoja, niin silta on terminä jäämässä taustalle. Silta-termiä käytetään erityisesti silloin, kun yhdistetään eri teknologioilla toimivia verkkoja toisiinsa nimenomaan linkkikerroksella. Kytkin, joka osaa useita eri ethernet-versioita, on myös silta.

Tyypillisesti termiin silta tai oikeastaan sen muunnokseen siltaus tai siltaava saattaa törmätä ADSL-modeemin tai muun verkkolaitteen yhteydessä. Tällainen laite voi olla joko reitittävässä tai siltaavassa tilassa. Tällöin käytännössä otetaan kantaa millä protokollapinon kerroksella laitteen halutaan toimivan. Reitittävä tila tarkoittaa sitä, että laite toimii verkkokerroksen reitittimenä ja suorittaa verkkokerroksen tehtäviä. Siltaava tila puolestaan tarkoittaa, että laite toimii kuin linkkikerroksen kytkin eli että se ei käsittele IP-pakettia lainkaan vaan lähettää kaikki verkkokerroksen paketit suoraan laitteen läpi kuten kytkin. Toki siltaavassa tilassa se ohjaa linkkikerroksen kehyksiä kytkimen tavoin oikeaan suuntaan, mutta se ei koske lainkaan verkkokerroksen sisältöön.

<quiz id="ae548f5f-8ad3-409b-9d39-e672a90ddcbe"> <quiz>


