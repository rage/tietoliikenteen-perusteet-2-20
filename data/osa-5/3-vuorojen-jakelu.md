---
path: '/osa-5/3-vuorojen-jakelu'
title: 'Lähetysvuorojen jakelu'
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata yleisellä tasolla, miten kanavan jakaminen ja lähetysvuorojen jakelu voidaan tehdä.
- Osaat esitellä erilaisia lähetysvuorojen jakeluun liittyviä periaatteellisia ongelmia sekä joitakin niihin liittyviä ratkaisuja
- Tunnistat mm. lyhenteet TDMA, FDMA, CDMA, CSMA/CD, CSMA/CA, jne.
- Osaat simuloida Ethernetin käyttämää lähetysvuorojen jakelumenetelmää erilaisissa tilanteissa.

</text-box>

## Yhteiskäyttöinen kanava ja lähetysvuorojen jakelu

Kanava, jolla lähettäjän ja vastaanottajan välinen viesti kulkee, voi olla joko oma tai jaettu. Omaa kanavaa kutsutaan kaksipisteyhteydeksi (engl. point-to-point), koska siinä on vain kaksi päätä lähettäjä ja vastaanottaja. Jos kanava on jaettu, niin silloin se on yleislähetysyhteys (engl. broadcast), jossa voi olla useita lähettäjiä ja vastaanottajia. Kaksipisteyhteys on esimerkiksi tähtiverkon yksi haara eli parikaapeli kytkimen ja isäntäkoneen (engl. host) välillä. Myös puhelinverkossa esimerkiksi kahden modeemin välinen yhteys on kaksipisteyhteys. [Point-to-Point Protocol (PPP)](https://fi.wikipedia.org/wiki/PPP_(tiedonsiirtoprotokolla)) on suunniteltu nimeomaan käytettäväksi tällaisilla yhteyksillä. Nykyisin ethernet on paljolti syrjäyttänyt sen, koska modeemien käyttö on vähentynyt ja ethernet-pohjaisten laajakaistayhteyksien käyttö lisääntynyt.

Langaton verkko, väylätopologia ja toisinaan myös tähtitopologia ovat yleislähetysyhteyksiä, koska kaikki voivat lähettää ja kaikki vastaaottajat kuulevat kaiken tai lähes kaiken kanavassa kulkevan liikenteen. Yleislähetysyhteydessä meillä on yksi jaettu kanava, jota kaikki kuuntelevat. Lähetys onnistuu virheettömäasti vain jos yksi lähettäjä lähettää kerrallaan. Jos useampi lähettäjä lähettää samanaikaisesti, niin syntyy yhteentörmäys, jolloin viestien signaalit sekoittuvat ja vastaanottajalle saapuu epämääräistä "bittimössöä". Vastaanottaja ei pysty tästä mössöstä enää erottamaan yhtään yksittäistä virheetöntä viestiä, vaan kaikki törmänneet viestit tuhoutuvat ja ne täytyy lähettää uudelleen. Meidän täytyy siis rakentaa mekanismi, jolla varmistetaan, että vain yksi lähettäjä kerrallaan lähettää.

Kanavan lähetysvuorojen jakelusta käytetään useita erilaisia englanninkielisiä termejä, ainakin channel access method, multiple-access protocol ja media access control (eli MAC). Niminä ne korostavat vuoronjaon eri ominaisuuksia. Suomen kielen termi 'lähetysvuorojen jakelu' on muuten kuvaava, mutta se ei valitettavsti ota kantaa siihen, että usein kyse on nimenomaan lähettäjällä tapahtuvasta päätöksenteosta. Englanninkieliset termit korostavat nimenomaan siitä, että kyse on lähettäjän tai viestin pääsystä (access) kanavalle.

Vuoronjakomenetelmää suunniteltaessa on siis päätettävä, miten solmu päättelee voiko se lähettää ja kuinka solmun on toimittava törmäystilanteessa. Huomaa, että tietoliikenteessä meillä on käytettävissä vain tämä yksi kanava, jolloin kaikki tarvittava neuvottelu lähetysvuoroista käydään tässä yhdessä ja samassa kanavassa, jossa myös dataviestit kulkevat. Neuvotteluissakin lähetetään ja vastaanotetaan viestejä, joiden lähetysvuoroista pitäisi sopia. Tässä siis sekä kontrolli (engl. control) että data kulkevat samassa kanavassa.

Yhteiskäyttöisen kanavan vuoronjakomenetelmä valitaan siten, että saamme mahdollisimman suuren osan kanavasta hyötykäyttöön. Tällöin menetelmän aiheuttama yleisrasite on pieni ja lähetysvuoroista sopiminen tehdään kustannustehokkaasti. Menetelmä on tällöin yleensä myös yksinkertainen ja halpa toteuttaa.

Kun kanavassa on vain yksi lähettäjä kerrallaan, se pystyy hyödyntämään kanavan koko siirtonopeuden eli siirtämään tietoa kanavan 'maksiminopeudella' R bittiä sekunnissa. Pidemmällä aikavälillä tarkasteltuna, kun meillä on M lähettäjää, niin jokainen niistä saa keskimäärin saman osuuden linjan siirtonopeudesta eli R/M bittiä sekunnissa.

Vuoronjakomenetelmän keskeinen ongelma onkin se, että miten voimme varmistaa, että yksi lähettäjä ei voi kaapata kanavaa itselleen, vaan että kaikilla lähettäjillä on tasapuolisesti mahdollisuus saada kanava käyttöön ja lähettää viesti sitä pitkin. Haluamme siis välttää yksittäisen lähettäjän nälkiintymisen (engl. starvation). Tietojenkäsittelytieteessä yleisesti nälkiintyminen tarkoittaa, että kyseinen entiteetti (tässä sanoman lähettäjä) ei saa koskaan tarvitsemaansa resurssia (tässä kanava). Jotta nälkiintymistä ei tapahdu, niin kanavanjako menetelmän täytyy olla tasapuolinen. Toisaalta myöskään lukkiutumista (engl. deadlock), eli että mikään lähettäjä ei voisi enää lähettää, ei saa esiintyä. Menetelmän pitää siis olla kaikilta ominaisuuksiltaan hyvin toimintavarma.


Lähetysvuorojen jakaminen yhdellä kanavalla voidaan tehdä usealla eri tavalla. Kanavanjakoprotokollilla (engl. channel partitioning protocols) voimme jakaa kanavan osiin ja antaa yhden osan yhden lähettäjän käyttöön. Vuorotteluprotokollilla (engl. taking-turns protocols) voimme jakaa käyttövuorot ennalta sovitun periaatteen mukaisesti vuoronperään eri lähettäjille. Kilpailuprotokollilla (engl. random access protocols) lähettäjät joutuvat kilpailemaan lähetysvuorosta. Vuorot jakautuvat niille satunnaisemmin ja lähetysvuoron odottamiseen kuluva aika on vaikeammin ennustettava.

### Kanavanjakoprotokollat

Kanavan jaolla annetaan jokaiselle lähettäjälle oma pala kanavasta. Lähettäjä saa tässä palasessa lähettää omia viestejä ilman, että muiden lähettäjien viestien signaalit häiritsevät lähetystä. Näin kukin solmu saa oman viipaleensa. Voimme jakaa kanavan paloihin ajan tai taajuuden perusteella. Lisäksi on mahdollista käyttää erilaisia bittien koodaustapoja, jolloin samassa kanavassa voidaan samanaikaisesti lähettää useita eri viestejä, jotka vastaanottaja pystyy edelleen erottamaan toisistaan.

Tässä siis oleellista on se, että lähettäjän käyttäessä sille varattua osaa kanavasta, ei yhteentörmäyksiä tapahdu, jolloin kyseinen osa kanavasta on aina lähettäjän käytettävissä. Näin kyseinen osa kanavasta on varattuna tälle lähettäjälle silloinkin, kun se ei sitä tarvitse.

Aikaviipaloidussa kanavassa, ns. [aikajakokanavointi](https://fi.wikipedia.org/wiki/TDMA) (engl. time-division multiple access, TDMA), jaetaan tietty aikajakso vakiokokoisiin aikaviipaleisiin siten, että aikaviipaleen pituus on yhden kehyksen lähetysaika ja jokainen lähettäjä saa (yhden) aikaviipaleen kerran tämän pidemmän aikajakson kuluessa.

<img src="../img/1-4-aikajako-vs-taajuusjako.svg" alt="">

KUVA:  Kanava jaettu aikaviipaleisiin ja niissä lähettäjät. Vieressä on sama kanava, mutta nyt taajuusjaoteltuna

[Taajuusjakokanavoinnissa](https://fi.wikipedia.org/wiki/FDMA) (eng. Frequency division multiple access, FDMA) jaetaan ajan sijasta käytettävää taajuusaluetta. Tämä muistuttaa esimerkiksi radio- tai tv-kanavien jakoperustetta, kun kyseessä on radiosignaalia käyttävä lähetys. Tässä jokaiselle lähettäjälle varataan kiinteä taajuusalue (fixed frequency band), joka on vain osa koko kanavan taajuusalueesta. Koska taajuusalue varataan yhdelle lähettäjälle, niin sitä eivät muut voi käyttää sinä aikana kun varaus on voimassa. Näin, jos lähetettävää ei kyseisellä lähettäjällä ole, niin se osa kanavan kapasiteetistä jää käyttämättä, vaikka muilla olisikin lähetettävää.

Sekä aikajaossa että taajuusjaossa lähettävä solmu (tai asema) saa käyttöönsä vain osan kanavan kapasiteetista. Jos kanavan kapasiteetti R bittiä sekunnissa jaetaan tasan kaikille M:lle lähettäjälle niin jokainen voi lähettää keskimäärin R/M bittiä sekunnissa, jos se käyttää kaikki sille varatut osat kanavasta.

[Koodijakokanavoinnissa](https://fi.wikipedia.org/wiki/CDMA) (engl. code division multiple access, CDMA) voidaan koko kanavan kapasiteetti antaa kaikkien lähettäjien käyttöön samanaikaisesti. Tätä käytetään lähinnä radiotiellä, jossa jokaisella asemalla on yksilöllinen (ja muiden kanssa ortogonaalinen) tapa koodata bitit 1 ja 0. Nyt kaikkien asemien lähettämät signaalit voivat vapaasti sekoittua radioaalloilla. Vastaanottajan pitää vain tietää, millä koodauksella sille tulevat viestit on koodattu ja se voi tämän avulla erotella saapuvasta yhteissignaalista itselleen kuuluvat bitit.


### Vuorotteluprotokollat

Vuorotteluprotokollia (engl. taking-turns protocols) kutsutaan myös vuoronantoprotokolliksi, koska niille on tyypillistä se, että kanavan käyttövuorot jaetaan jollakin etukäteen sovitulla tavalla (kuten vuorokysely tai vuoromerkki), mutta vain niille, joilla on jotain lähetettävää. Näin vältetään kanavan pitäminen turhaan varattuna. Toisaalta, kun lähettäjällä on lähetysvuoro, niin kaikki tietävät sen, eikä viestien yhteentörmäyksiä kanavassa pääse syntymään.

Vuorokyselyssä erityinen isäntäsolmu kyselee vuorotellen (engl. polling) jokaiselta solmulta, onko sillä lähetettävää. Jos solmulla on lähetettävää se voi kyselyn saatuaan lähettää yhden viestin. Isäntä kuuntelee signaalia ja osaa päätellä milloin lähetys loppui. Sitten se kysyy seuraavalta vuorossa olevalta solmulta, onko sillä lähetettävää. Näin lähetysvuoro kiertää solmulta toiselle.

Vuoromerkkiä (engl. token) voidaan käyttää vuoroja jakavan isäntäsolmun sijaan. Se soveltuu erityisesti rengastopologiaan. Kun solmulla on vuoromerkki, se saa lähettää viestin, jonka jälkeen se antaa vuoromerkin renkaassa seuraavana olevalle solmulle. Jos solmulla ei ole lähetettävää, kun se saa vuoromerkin, se antaa merkin välittömästä eteenpäin. Näin kanavan kapasiteetti tulee käyttöön tehokkaammin kuin kanavanjakoon perustuvissa menetelmissä.

Näistä on olemassa erilaisia versioita, joissa voidaan määritellä tarkemmin esimerkiksi, miten kauan vuoromerkkiä saa pitää, kuinka monta kehystä yhdessä vuorossa voi lähettää, jne. Esimerkiksi [Token Ring](https://fi.wikipedia.org/wiki/Token_Ring) on vuorottelua käyttävä verkko.

Näissä vuorottelumenetelmissä on yksi iso ongelma, ns. 'single point of failure'. Menetelmä toimii hyvin vain niin kauan kuin vuoromerkki tai kyselyjä hoitava isäntäkone ei katoa.  Jos tällainen vikaantuminen kuitenkin tapahtuu, niin mikään solmu ei voi lähettää kanavassa ja päädytään lukkiutuneeseen tilaan. Tästä voidaan toipua vain ensin havaitsemalla tilanne ja sitten joko valitsemalla uusi isäntäsolmu tai ottamalla käyttöön uusi vuoromerkki. Tällainen toipuminen on hidasta ja sen aikana viestit eivät kulje. Jätämme tällaisen hajautettujen järjstelmien yhteistoimintaan liittyvän asian tarkemman käsittelyn myöhemmille kursseille.



### Kilpailuprotokollat

Kilpailuprotokollista (engl. random access protocols) käytetään englanniksi myös termiä multiple access protocols. Lähettäjiä on useita ja ne kilpailevat lähetysvuoroista. Lähettäessään niiden pitää noudattaa tiettyjä sääntöjä, mutta etukäteen ei ole tiedossa milloin ja missä järjestyksessä lähettäjät voivat toimia. Periaatteena on siis yksinkertaisesti "kuka ensin ehtii".

Koska mitään keskitettyä jakoa tai vuorottelua ei ole, niin jokaisen solmun pitää itsenäisesti, pelkästään sen hetkisen paikallisen tiedon varassa, tehdä päätös viestin lähettämisestä tai lähetyksen viivästämisestä. Kun solmu haluaa lähettää, niin se ensin kuuntelee, onko joku muu solmu jo lähettämässä. Jos ei ole, niin solmu aloittaa oman lähetyksensä. Kaikki lähetykset tehdään aina kanavan täydellä nopeudella. Jos joku muu jo lähettää, niin solmu viivästää omaa lähetystään ja odottaa kanavan vapautumista.

Koska lähettäjät kilpailevat kanavasta, niin on mahdollista, että kaksi lähettäjää yrittää lähettää samaan aikaan ja tapahtuu yhteentörmäys. Mikäli kahden lähettäjän viestit ovat kanavassa samaan aikaan, niin kumpikin viesti tuhoutuu, koska niiden signaaleja ei voi sekoittumisen jälkeen enää erottaa. Kun tällainen viestien törmäys tapahtuu, niin molemmat lähettäjät joutuvat lähettämään omat viestinsä myöhemmin uudelleen.

Solmun pitää tavalla tai toisella havaita törmäys. Tässä suhteessa eri protokollat toimmivat eri tavalla. Niissä on myös eroja siinä, miten törmäyksestä toivutaan eli miten ja milloin uudelleenlähetys tapahtuu. Tyypillisesti törmäyksen jälkeen solmu odottaa satunnaisen (engl. random) ajan ja yrittää sitten uudelleen.


#### Aloha ja viipaloitu aloha

Aloitetaan historiasta ja ihan ensimmäisestä langattoman verkon lähetysvuorojen jakoprotokollasta, jonka nimi on [ALOHA](https://en.wikipedia.org/wiki/ALOHAnet). Vaikka ALOHA kehitettiin nimenomaan langatonta radioteitse tapahtuvaa viestintää varten 1971 Havaijilla, niin sitä käytettiin myös kaapeliyhteyksillä.

Alkuperäinen ALOHA ei ollut kovin tehokas, koska jokainen solmu sai lähettää viestin heti, kun sillä oli lähetettävää. Se ei siis  etukäteen kuunnellut onko radiotiellä jo viesti kulkemassa. Solmu kuunteli vasta lähetyksen jälkeen onnistuiko lähetys. Jos törmäys tapahtui, niin solmun piti odottaa satunnainen aika ja yrittää sitten uudelleen.

Koska solmut eivät kuunnelleet etukäteen ja koska viestien pituutta ei ollut rajoitettu, niin alkuperäisessä ALOHAssa törmäyksen todennäköisyys oli niin suuri, että vain noin 18% kanavan koko kapasiteetista saatiin käyttöön. Suurin osa kanavan kapasiteetista kului siis törmäysten vuoksi hukkaan. Katso tuolta englanninkielisestä wikipediasta kuvia, joista selviää miksi yhteentörmäykset vievät noin suuren osan kanavan kapasiteetista.

<quiz id="b56f937e-907b-492f-8314-efd73978c244"> </quiz>


ALOHAsta tehtiin paranneltu versio, jossa karsittiin osittain päällekkäiset yhteentörmäykset pois. Viipaloidussa ALOHAssa (engl. slotted ALOHA) kaikki siirrettävät kehykset ovat keskenään samankokoisia. Lisäksi kanava jaettiin aikaviipaleisiin siten, että yhdessä aikaviipaleessa voi lähettää yhden kehyksen. Nyt kaikki solmut aloittavat lähetyksensä aina aikaviipaleen alusta eikä kesken toisen solmun lähetystä. Tämä onnistui, kun solmut ja niiden kellot saatiin synkronoitua. Kaikki (lähettävät) solmut havaitsevat yhteentörmäykset.

Kun siirtokehys on valmis, niin solmu lähettää sen heti seuraavassa aikaviipaleessa. Jos ei tullut yhteenörmäystä, niin solmu voi lähettää seuraavan kehyksen heti seuraavassa aikaviipaleessa. Jos lähetyksessä tuli yhteentörmäys, niin solmu yrittää lähetystä uudelleen seuraavassa aikaviipaleessa jollakin todennäköisyydellä p.

Viipaloidulla ALOHAssa saadaan kanavan kapasiteetista käyttöön noin 37%, mikä on kaksinkertainen määrä alkuperäiseen ALOHAan verrattuna. Englanninkielisessä wikipediassa on hyvin kuvattu matemaattinen päättelyketju tämän tehokkuusluvun laskennassa.

Viipaloitua ALOHAa käytetään edelleen tietyissä erikoistilanteissa, koska se on toteutukseltaan ja toiminnaltaan hyvin yksinkertainen ja koska se sallii yhden lähettäjän lähettää täydellä nopeudella, jos muita lähettäjiä ei samanaikaisesti ole.

Wikipedian englanninkielisellä [ALOHAnet](https://en.wikipedia.org/wiki/ALOHAnet) sivulla on hyvä kuva viipaloidun ALOHAn toiminnasta useamman lähettäjän kanssa. Kuvassa on sekä yhteentörmäyksiä että onnistuneita lähetyksiä.

<quiz id="ac4463e7-892e-470d-bb03-e3b8788683be"> </quiz>




### Lähetyskanavan kuuntelu CSMA

[Lähetyskanavan kuuntelu](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access) (engl. Carrier Sense Multiple Access, CSMA) on ALOHAn jälkeen kehitetty menetelmä, jossa keskeistä on, että lähettäjän täytyy ennen omaan lähetystään varmistaa, että kanava on tyhjä. Ajatus on siis, että solmu kuuntelee kanavaa ennen kuin lähettää ja lähettää vain, jos kanava on tyhjä. Jos kanava on tyhjä, niin voi lähettää samantien. Jos kanava on varattuna, niin solmun pitää odottaa hetki ja sitten voi yrittää uudelleen. Odotuksen kesto riippuu CSMAn toteutusvaihtoehdosta. Aika tavallista on odottaa satunnainen aika ja sitten tarkistaa, onko kanava vapaa. Odottaminen voi myös olla aktiivista, jolloin solmu aktiivisesti kuuntelee kanavan vapautumista ja lähettää heti, kun kanava on vapaa.

Etukäteen tapahtuvalla kanavan kuuntelulla ei kuitenkaan voi havaita kaikkia muiden lähetyksiä ainakaan riittävän nopeasti, joten yhteentörmäykset ovat edelleen mahdollisia. Esimerkiksi kaapelissa etenemisviiveen takia kaksi lähettäjää voivat molemmat havaita kanavan tyhjäksi ja aloittaa lähetyksen samaanaikaan. Langattomassa lähiverkossa lähettäjän ympäristön kuuntelu ei kerro, onko vastaanottaja saamassa sanomia muilta, joita lähettäjä ei voi etäisyyden takia kuulla. Vastaavasti satelliittikanavan kuuntelu ei paljasta, onko jonkin muu maa-asema jo aloittanut lähetyksen. Huomaathan, että sateelliittien kautta tapahtuvassa viestinnässä etenemisviiveet voivat olla hyvinkin suuria.

Mikäli yhteentörmäys tapahtuu, niin pakettien lähetykset epäonnistuvat ja ne joudutaan lähettämään uudelleen. Tällöin paketin lähettämiseen käytetty aika menee hukkaan. Solmujen väliset etäisyydet ja etenemisviiveet vaikuttavat yhteentörmäysten todennäköisyyteen, koska jos solmu ei voi havaita toisen solmun jo aloittamaa lähetystä, niin se voi aloittaa oman lähetyksensä samaan aikaan tuon toisen solmun kanssa.

CSMA:sta on useita erilaisia versioita esimerkiksi CSMA/CA (engl. Collision Avoidance, CA), jossa pyritään ensi sijaisesti välttämään törmäyksiä. Törmäysten välttämistä käytetään mm. WLANien käyttämässä standardissa [IEEE 802.11](https://fi.wikipedia.org/wiki/IEEE_802.11). Langattomassa liikennöinnissä solmun on vaikea samaan aikaan sekä lähettää että kuunnella mahdollista yhteentörmäystä. Sen takia siinä pyritään ensi sijaisesti välttämään törmäyksiä. Tämä voidaan tehdä esim. lähettämällä varaussignaali ennen varsinaista kehystä tai kuten IEEE 802.11:sta odottamalla satunnainen aika vapaata kanavaa seuraten ennen varsinaista lähetystä. Näin, jos kaksi solmua haluaa lähettää samaan aikaan, on todennäköistä, että toinen niistä aloittaa ensin ja toinen havaitsee lähetyksen eikä yritä lähettää päälle, vaan joutuu odottamaan pidempään.

Ethernetissä käytetään [CSMA/CD:tä](https://fi.wikipedia.org/wiki/CSMA/CD) (engl. Collision Detection). Siinä solmu voi aloittaa lähettämisen heti, kun se havaitsee kanavan olevan vapaana, mutta sen täytyy samanaikaisesti kuunnella kanavaa, jotta se voi havaita yhteentörmäyksen. Koska liikennöinti tapahtuu kaapelissa, niin voimme olla varmoja, että solmu voi havaita yhteentörmäyksen. Havaitseminen voidaan tehdä esimerkiksi signaalin voimakkuuden muutoksesta. Langattomassa verkossa törmäystä ei aina voida havaita, koska lähettäjät eivät välttämättä voi kuulla toisiaan esimerkiksi liian suuren etäisyyden aiheuttaman signaalin vaimenemisen vuoksi.

CSMA/CD:ssä solmun pitää keskeyttää kehyksen lähettäminen heti (minimipituuden jälkeen), jos se havaitsee törmäyksen. Tällä nopealla keskeytyksellä pienennetään törmäyksen aiheuttaa hukka-aikaa ja lyhennetään kanavan varauksen kestoa. Näin yhteentörmäyksestä toivutaan hiukan nopeammin, kuin jos solmut lähettäisivät kehyksen kokonaan. Tämä on CSMA/CD:n keskeinen ero perus-CSMA:han, jossa solmu saa lähettää koko kehyksen, vaikka se olisi havainnut törmäyksen.

Jos kehyksen lähetys epäonnistui törmäyksen vuoksi, niin solmu odottaa satunnaisen ajan ja yrittää lähettämistä sen jälkeen uudelleen. Tällöinkin se voi lähettää vasta, kun kanava on vapaa.

Iso kysymys erityisesti CSMA/CD:ssä on, että kuinka paljon pitää vähintään lähettää ja toisaalta, kuinka kauan on kuunneltava oman lähetyksen aloituksen jälkeen. Ajatellaan, että A:n ja B:n etäisyys on kyseisessä kaapelissa mahdollisimman suuri. Tällöin ne ovat kaapelin eri päissä. Vaikka A on jo aloittanut lähettämisen, niin B kuulee vielä hetken aikaa tyhjän kanavan ja voi aloittaa oman lähetyksensä. Tämä aika on etenemisviive A:lta B:lle. Jotta A voi havaita törmäyksen, niin sen pitää kuulla B:n lähetys, johon menee vielä toinen etenemisviive. Näin ollen pisin aika, jonka kuluessa mikä tahansa solmu voi havaita törmäyksen on 2 * etenemisviive solmujen välillä.  Varmistaakseen yhteentörmäyksen havaitsemisen solmu ei saa lopettaa omaa lähetystään ennen kuin törmäysignaali olisi ehtinut tulla. Näin ollen pienin sallittu lähetettävän kehyksen koko on se, jonka lähettäminen kestää vähintään 2 kertaa kaukaisimpien laitteiden välisen maksimietenemisviiveen verran. Näin ethernet-kehyksen minimikoko rajoittaa yhden linkkivälin ethernet-kaapelin pituutta.

Solmun, tai oikeammin sen verkkosovittimen (verkkokortti ja sen laiteohjain), toimenpiteet kehyksen lähetyksessä, kun noudatetaan CSMA/CD:tä:
1) Verkkosovitin saa kehyksen välitettäväksi.
2) Jos verkkosovitin havaitsee, että kanava on tyhjä, niin se aloittaa kehyksen lähetyksen samantien. Jos se havaitsee, että kanava on käytössä, niin se odottaa kunnes kanava on tyhjä ja aloittaa sitten kehyksen lähetyksen.
3) Kun kehyksen lähetys on käynnissä, verkkosovitin kuuntelee kanavaa ja pyrkii havaitsemaan jonkun muun lähettämän signaalin.
4) Jos verkkosovitin havaitsee signaalin, niin se lopettaa kehyksen lähettämisen (vaaditun minimikoon jälkeen).
5) Jos kehyksen lähetys päättyy, eikä muiden signaaleja ole kuultu, niin kehyksen lähetys on onnistunut.
6) Jos kehyksen lähetys päättyi keskeytykseen, niin verkkosovitin odottaa satunnaisen ajan ja jatkaa kohdasta 2.

Tuo verkkosovittimien satunnainen odotusaika kasvaa jokaisen peräkkäisen yhteentörmäyksen yhteydessä. Tästä käytetään englanninkielistä termiä binary exponential backoff. Odotusaika valitaan satunnaisesti kokonaislukujen joukosta {0,1,2,... (2^n)-1}, missä n on peräkkäisten yhteentörmäysten määrä.

Huomaathan, että verkkosovittimien toiminta on täysin riippumatonta aiempien viestien onnistumisesta ja muiden verkkosovittimien toiminnasta. Tällöin on täysin mahdollista, että verkkosovitin, joka juuri sai kehyksen välitettäväksi, saa kehyksensä välitettyä sillä aikaa, kun muut yhteentörmäysten jälkeen odottavat seuraavaa lähetysyritystä.

<quiz id="a349c4e9-8207-4fb2-82cb-d7d9ea386496"> </quiz>

