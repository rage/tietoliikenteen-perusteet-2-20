---
path: '/osa-2/2-laki-ja-kuuntelu'
title: 'Verkon kuuntelu ja lainsäädäntö'
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

-Ymmärrät miksi verkon kuuntelu on helppoa ja miksi kuuntelulta suojautuminen tapahtuu lähinnä salauksen avulla.
- Tiedät millaisia lainsäädännöllisiä aspekteja liittyy tietoliikenteen seurantaan verkossa
- Ymmärrät milloin verkon liikennettä voi kuunnella / kaapata ja miten, jotta ei riko lakia
- Osaat alkeellisella tasolla käyttää Wireshark -ohjelmaa valmiin tallennetun liikenteen analysointiin

</text-box>

## Liikenteen analysointi

Tietoliikennettä ja sen protokollien toimintaa voi seurata useilla erilaisissa analysointiohjelmilla. Niiden avulla voi kuunnella tietoliikenneverkossa kulkevia paketteja. Tällainen verkkoliikenteen seuranta ei kaikissa tilanteissa ole laillista, joten sitä ei pidä noin vain kokeilla missä vaan. Rikosoikeudellinen vastuu ja korvausvastuu on aina liikennettä seuraavalla henkilöllä, joten verkon käyttösäännöt pitää tuntea ennen kuin edes lähtee kokeilemaan liikenteen kuuntelua. Kuunnellun liikenteen voi myös tallettaa myöhempää analysointia varten. Tällaisesta pakettien tallettamista kutsutaan englanniksi capture ja suomeksi usein käytetään liikenteen kaappamista tai sieppaamista. Tällaista toimintaa tekevät sekä hyvikset että pahikset. Kumpaan joukkoon liikenteen analysoija milläkin hetkellä kuuluu voi riippua näkökulmasta. Verkon ylläpitäjällä on oikeus seurata verkon liikennettä, jos näin on käyttösäännöissä sovittu. Toisaalta verkon käyttäjä ei varmaankaan toivo oman arkaluontoisen viestittelynsä näkyvän edes ylläpitäjälle.

Viestien salaus on ainoa tällä hetkellä tunnettu keino estää muita seuraamasta omien viestien sisältöjä ja silloinkin liikenteen seuraajalla on käytettävissään erilaisia keinoja liikenteen analysointiin.

Tietoliikenteen seuraamiseen on saatavilla valmiita ohjelmia. Wireshark on ilmainen, avoimen lähdekoodin ohjelma, joka on laajasti käytetty. Ohjelman voi ladata [Wiresharkin omilta sivuilta](https://www.wireshark.org/). Sille on myös saatavilla paljon valmiita analysoitavia tiedostoa, jolloin voimme välttää todellisen tietoliikenteen kuuntelun tällä kurssilla kokonaan.

Wireshark on erittäin monipuolinen ja tarjoaa paljon vaihtoehtoja tietoliikenteen analysointiin. Tällä kurssilla käytämme hyvin pientä osaa sen piirteistä ja tarkastelemme vain muutamaa valmista aineistoa.


## Lainsäädännöstä ja etiikasta

Tietoliikenteen ammattilaisten ja käyttäjien on hyvä tietää mitä verkossa voi tehdä ja mitä ei. Suomen laki asettaa suomalaisille toimijoille selkeät rajat. Ajankohtaisen kotimaisen lainsäädännön voit aina tarkistaa [finlex.fi](https://finlex.fi) -palvelusta.

Alalla on myös TIVIAn laatimat ja julkaisemat [etiikan ohjeet](https://tivia.fi/toimiala/etiikan-ohjeet/), joita kaikkien alan ammattilaisten olisi hyvä noudattaa. Nämä ohjeet ovat sen verran lyhyet, että kaikkien hyvä ne lukea ja ymmärtää. Sen sijaan kaikkia tässä osiossa mainittua lakeja ei tarvitse kurssimateriaalina lukea.

Tietoliikenteeseen liittyy useita lakeja, mutta näistä ehkä kaikkien keskeisin on suhteellisen uusi laki [sähköisen viestinnän palveluista]( https://www.finlex.fi/fi/laki/ajantasa/2014/20140917).  Siinä käydään läpi erilaisten sähköisten palvelujen tarjoajien oikeuksia ja velvollisuuksia. Tähän lakiin perustuu esimerkiksi entisen viestintäviraston, nykyisen TrafiComin, oikeus hallita .fi ja .ax osoitteita.

Harjoitellaan hiukan lakien lukemista. Tutustu siis sähköisen viestinnän palvelujen lain lukuun 21 ja lue siitä, mitä laissa sanotaan verkkotunnuksista.

<quiz id="a5c757b9-8403-4b31-b773-db24b9cd7b10"></quiz>


Kaikenlaiset väärinkäytöksiin, rikoksiin ja niiden valmisteluun liittyvät säännöt ja rangaistukset on koottu [rikoslakiin](https://www.finlex.fi/fi/laki/ajantasa/1889/18890039001). Tietotekniikkaan ja tietoliikenteeseen liittyviä rikoksia on kuvattu erityisesti rikoslain luvussa 38, joka käsittelee tieto- ja viestintärikoksia. Toki lain muissakin luvuissa on kuvattu rikoksia, jotka voivat liittyä tietotekniikkaan ja tietoliikenteeseen.

<quiz id="b0b05e15-8cb3-4de1-bd7b-e990d5fe766a"></quiz>



Jos hallinnoisimme verkkoa tai sen palvelua siten, että käsittelisimme henkilötietoja, niin meidän täytyisi noudattaa [tietosuojalakia]( https://www.finlex.fi/fi/laki/ajantasa/2018/20181050). Työnantajina ja työntekijöinä on hyvä tietää mitä laissa [yksityisyyden suojasta työelämässä](https://www.finlex.fi/fi/laki/ajantasa/2004/20040759) määrätään.

Koska seuraavaksi tutustutaan työkaluun, jolla voi kuunnella tietoliikenneverkossa kulkevia viestejä, on tärkeä tietää, milloin tällainen kuunteleminen on sallittua ja milloin ei. Suomen laki asettaa perusrajat verkkokuuntelulle, mutta myös verkon käyttösäännöt vaikuttavat asiaan. Esimerkiksi Helsingin yliopiston käyttösäännöt kieltävät kaiken verkkokuuntelun yliopiston omissa verkoissa. Näin on useimpien palveluntarjoajien käyttösäännöissä.

Lait myös muuttuvat. Keväällä 2019 tuli uusi laki  [tietoliikennetiedustelusta siviilitiedustelussa](https://www.finlex.fi/fi/laki/alkup/2019/20190582), joka tarkentaa poliisilakia tältä osin. Lisäksi on paljon muitakin lakeja, jotka vaikuttavat erilaisten palvelujen tarjoajiin ja muihin ammattilaisiin ja heidän toimintaansa.

## Tietoliikenteen kaappaus

Tietoliikenneverkon kuuntelu on teknisesti hyvin helppoa. Laitteen verkkoyhteys täytyy vain asettaa tilaan, jossa linkkikerros ei valikoi viestejä laitteen MAC-osoitteen perusteella, vaan ottaa vastaan kaikki saapuvat viestit. Näin ne voidaan ottaa talteen, vaikka niitä ei olisi osoitettukaan tälle laitteelle. Tästä toiminnallisuudesta käytetään englanninkielistä termiä 'promiscuous mode'. Tilan voisi kääntää valikoimaton tai sekalainen, jolla tarkoitetaan sitä, että verkkokortti ei valikoi (tai poimi) kuulemistaan viesteistä vain tälle laitteelle osoitettuja, vaan ottaa tyynesti vastaan kaiken kuulemansa verkkoliikenteen.

Verkkokortin tai verkkoyhteyden toiminnallisuuden muuttaminen edellyttää yleensä laitteen pääkäyttäjän oikeuksia, joten tavallinen käyttäjä ei pääse kuuntelemaan liikennettä.

Perinteinen UNIX / Linux ohjelma, jolla tietoliikennettä on seurattu jo vuosikymmeniä, on tcpdump. Ohjelmalla on vain tekstipohjainen käyttöliittymä, joten sen käyttäminen edellyttää erilaisten komentojen opettelua. Yleisesti käytetty valikkoja ja graafista käyttöliittymää hyödyntävä ohjelma on Wireshark. Siitä lisää alempana.

Nämä tietoliikenteen kaappaamiseen tarkoitetut ohjelmat käyttävät yleensä käyttöjärjestelmän tarjoamaa pcap -rajapintaa, joka on tarkoitettu juuri verkkoliikenteen seurantaan. Englanninkielisellä wikipedian sivulla [Pcap](https://en.wikipedia.org/wiki/Pcap) on lueteltu paljon muitakin ohjelmia, joilla verkkoliikennettä voi kaapata.

Tällä kurssilla emme kaappaa liikennettä, koska kaikilla osallistujilla ei välttämättä ole käytettävissä sellaista tietoliikenneverkkoa, jossa se olisi sallittua.

Wiresharkin sivulta https://wiki.wireshark.org/SampleCaptures löytyy kymmeniä ellei satoja valmiita kaappaustiedostoja, joita voimme käyttää kurssilla.

Internetissä on myös joitakin ilmaisia verkossa toimivia kaappaustiedostojen katseluohjelmia  (kuten packettotal.com), joten Wiresharkin käyttäminen ei ole millään tavalla pakollista kurssin kuluessa.  Näiden ohjelmien kanssa pitää olla varovainen, koska ne tallettavan kaikki saamansa kaappaukset, joten jos kyseessä on mahdollista yksityistä tietoa sisältävä kaappaus, niin näitä ei pidä käyttää. Wiresharkin valmiiden kaappausten kanssa näiden käyttö on mahdollista.

Osa näistä ohjelmista näyttää vain pakettien otsaketietoja ja niitäkin rajoitetusti, joten kovin kattavaa mahdollisuutta viestien sisältöjen seurantaan ne eivät välttämättä tarjoa. Toisaalta kuten tuosta sähköisiä viestintäpalveluja käsittelevästä laista pystyimme lukemaan, eivät edes verkon ylläpitäjät saa manuaalisesti tutkia viestien sisältöjä.

Tyypillisesti liikenteen kaappaaminen ja seuranta on vain ensimmäinen askel verkon laittoman käytön selvittämisessä / estämisessä. Varsinaiset laajemmat [tunkeilijan havaitsemisjärjestelmät](https://fi.wikipedia.org/wiki/Tunkeilijan_havaitsemisj%C3%A4rjestelm%C3%A4) tekevät laajempia analyysejä liikennevirrasta, jotta väärinkäytökset voitaisiin havaita mahdollisimman hyvin. Valitettavasti meillä ei ole tällä kurssilla mahdollisuutta tarkastella näitä järjestelmiä tarkemmin.


## Wireshark

Jos haluat asentaa ohjelman omalle koneellesi, niin ladattava tiedosto löytyy Wiresharkin omalta sivulta https://www.wireshark.org/

Älä kokeile liikenteen kaappaamista, ellet ole varma, että se on kyseisessä verkossa sinulle sallittua. Kotiverkossa, jonka ylläpitäjä olet, voit yleensä kokeilla liikenteen kaappaamista, mutta silloinkin on tarpeellista kertoa asiasta kaikille muille verkon käyttäjille.
Näin vältät mahdolliset yksityisyyden suojan loukkaukset.
