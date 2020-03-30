---
path: '/osa-4/2-IP-protokolla'
title: 'IP-protokolla'
hidden: false
---


<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata IP-protokollan toimintaperiaatteet ja sanomarakenteen.
- Osaa kertoa miten reititystaulua käytetään viestien edelleenlähetyksessä.

</text-box>

## IP

[IP-protokollan]( https://fi.wikipedia.org/wiki/IP) keskeinen tehtävä on huolehtia viesti lähettäjältä vastaanottajalle siten, että viesti liikuu verkossa aina yhden linkkivälin kerrallaan ja verkkokerroksen tasolla päätetään mihin linkkiin viesti seuraavaksi lähetetään. Oikeastaan IP ei ole vain yksi protokolla vaan oikeammin protokollaperhe, jonka eri protokollilla on omat tehtävänsä.
IP:n toimintaan liittyy keskeisenä elementtinä osoitteet, viestin rakenne ja viestin kuljettaminen eli reititys.  Osoitteet olemme jo käsitelleet aiemmin, joten käydään seuraavaksi läpi viestin rakenne


IP-prokollan mukaista viestiä kutsutaan IP-paketiksi (engl. packet). IP-paketin kuvan voit katsoa wikipediasta IP sivulta kohdasta [IPv4-paketti](https://fi.wikipedia.org/wiki/IP#IPv4-paketti)

IPv4:n mukaisen paketin kentät ovat seuraavat:
* **Ensimmäinen sana**  eli ensimmäiset 32 bittiä
* bitit 0-3: versionumero, joka IPv4:n tapauksessa on 4 ja bitteinä siis 0100
* bitit 4-7: otsakkeet pituus. Minimikokoinen otsake on kooltaan 5 32-bittistä sanaa, joten yleensä arvon on 5. Arvo voi olla enemmän, jos otsakkeen optiokentät ovat käytössä
* bitit 8-15: palveluluokka. Tämän tarkempi käsittely jää myöhemmille kursseille.
* bitit 16-31: paketin pituus tavuina. Tämä on siis paketin kokonaispituus sisältäen sekä otsakkeen että datan. Huomaa, että otsakkeen koko ilmoitettiin edellä sanoina, mutta tässä paketin koko on tavuina.
* **Toinen sana**  eli seuraavat 32 bittiä  eli bitit 32-63
* Tässä on paketin fragmentointiin eli pilkkomiseen ja kokoamiseen liittyvät kentät, fragmentin tunniste, lipukkeita ja fragmentin sijainti alkuperäisessä viestissä
* **Kolmas sana** eli bitit 64-95
* sanan ensimmäinen tavu: paketin elinaika, hyppylaskuri, eli kuinka monta edelleenlähetystä paketille saa korkeintaan tehdä. Reititin vähentää tätä aina yhdellä.
* sanan toinen tavu: Protokollan numero. Mille protokollalle data välitetään. Tyypillisiä arvoja ovat 6-TCP ja 17-UDP.
* sanan kolmas ja neljäs tavu: Tarkistussumma. Otsakkeen tarkistussumma, jotta sen oikeellisuus voidaan tarkistaa.
* **Neljäs sana** eli bitit 96-127
* Lähettäjän IPv4 osoite
* **Viides sana** eli bitit 128-159
* Vastaanottajan IPv4 osoite
* **Mahdolliset optiot** Näiden pitää olla myös kokonaisia 32-bittisiä sanoja. Yleensä näitä ei käytetä, joten data alkaa heti 5. sanan jälkeen

Otsakkeen lisäksi paketissa on sitten varsinainen data. Vastaanottajalla verkkokerros antaa tämän datan otsakkeen mukaiselle protokollalle, joka yleensä on kuljetuskerroksen protokolla. Joissakin tilanteissa kyseessä voi myös olla jokin verkkokerroksen omista protokollista.

IPv4:ssä on määritelty, että yhden lähetetyn paketin voi matkalla pilkkoa useammaksi eri paketiksi, eli alkuperäisen paketin paloiksi, joita kutsutaan fragmenteiksi. Kun paketti on pilkottu fragmenteiksi, nämä fragmentit kulkevat verkossa kuten mikä tahansa IP-paketti aina vastaanottajalle asti. Vasta vastaanottaja yhdistää fragmentit alkuperäiseksi paketiksi. Kun se on saanut alkuperäisen paketin kokonaisuudessaan, voi se antaa data-osion eteenpäin.

Fragmentointi edellyttää reitittimiltä lisätoiminnallisuutta ja hidastaa pakettien uudelleenlähetystä, kun niitä pitää muokata. Toisaalta fragmentointi on välttämätöntä, jos IP-paketti on suurempi kuin mitä linkkikerros pystyy kuljettamaan yhdessä kehyksessä. Mikäli pakettia ei voisi pilkkoa, sitä ei voisi lähettää eteenpäin, joten viestijöiden näkökulmasta se vain katoaisi matkalla.

IPv6:ssa suunnittelun lähtökohtana on ollut pakettien uudelleenlähetyksen mahdollisimman sujuva toiminta ja siksi siitä on jätetty kokonaan pois mahdollisuus pakettien fragmentointiin. Siinä on lähettäjälle tarjolla protokolla, jolla lähettäjä voi tarkistaa mikä on maksimipaketin koko reitillä lähettäjältä vastaanottajalle. IPv6:ssa reititin vain pudottaa liian suuren paketin, jota se ei voi lähettää eteenpäin. Se lähettää tällaisesta tilanteesta kontrolliprotokollan (Internet Control Message Protocol v6, ICMPv6) mukaisen viestin paketin alkuperäiselle lähettäjälle. Näin lähettäjä saa tiedon paketin maksimikoosta ja voi jatkossa huolehtia siitä, että lähetettävät paketit ovat tätä pienempiä.

[IPv6](https://fi.wikipedia.org/wiki/IPv6) pakettien otsakkeita on myös virtaviivaistettu. Otsake on kiinteänkokoinen (320 bittiä) eikä sisällä tarkistussummaa. Otsakkeen neljä ensimmäistä bittiä ovat versionumero kuten IPv4:ssä. Tämä on välttämätöntä, jotta pakettia käsittelevä solmu voi tunnistaa kumman protokollan mukainen viesti on kyseessä. IPv6:ssa näiden bittien arvo on siis 6 eli binäärilukuna 0110. Koska tämä kiinteä otsake ei aina riitä, IPv6 sallii, että datan alussa voi olla lisäotsakkeita, joiden tunnistamiseen on kiinteä kenttä. Tämä 'seuraava otsake' kenttä kertoo siis minkä protokollan mukainen otsake on datakentän alussa. Tässä käytetään samoja protokollanumeroita kuin IPv4:n kentässä 'protokolla numero'. Näin IPv6:n otsake saadaan pidettyä selkeänä ja yksinkertaisena reitittimien kannalta. Reitittimien ei tarvitse välittää näistä data-alueen tiedoista.


## Paketin kuljetus verkossa

Internetin toiminnalle keskeistä on, että verkkokerros osaa kuljettaa IP-paketin lähettäjältä vastaanottajalle. Verkkokerroksella pitää siis olla riittävästi toiminnallisuutta, jotta paketti voi edetä etappikerrallaan kohti vastaanottajaa ja lopulta päästä perille. Tätä verkon toiminnallisuutta kutsutaan reititykseksi. Paketti siis reititetään lähettäjältä vastaanottajalle. Tarkastellaan reitittimien yksityiskohtaisempaa toimintaa seuraavassa aliluvussa.  Koska internetissä ei normaalisti varata reittejä etukäteen, on reitittimillä oltava riittävä tieto verkon rakenteesta, jotta ne osaavat lähettää paketin eteenpäin oikeaan suuntaan. Tätä tietoa säilytetään reititystauluissa. Käymme tällä kurssilla läpi vain IPv4:n mukaiset reititystaulut.

Toisaalta verkossa liikkuu sekä IPv4:n että IPv6:n mukaisia paketteja. Jos reititin osaa käsitellä niitä molempia, niin silloin sille voi lähettää paketteja välitettäväksi kummalla tahansa versiolla. Jos reititin ei osaa käsitellä kuin IPv4:n mukaisia paketteja, niin sille ei kannata lähettää IPv6:n mukaisia paketteja, koska se vain pudottaa ne pois verkosta. Mikäli paketti pitää välittää verkossa tällaisten reitittimien kautta joudutaan tunneloimaan (engl. tunneling) eli rakentamaan tunneli, jossa toisen protokollan viesti kulkee toisen protokollan sisällä. Tällainen tunneli muodostetaan siis kahden sellaisen reitittimen välille, jotka hallitsevat kummankin protokollaversion. Tämä tehdään "piilottamalla" tuntematon protokollaversio tunnetun protokollan sisään.


### IPv4 Reititystaulu

Kun reititin saa linkistä paketin, niin sen täytyy päättää mihin linkkiin paketti lähetetään seuraavaksi. Tavoitteena reitittimellä on siis lähettää paketti edelleen sellaiseen linkkiin, jonka kautta se voi päästä vastaanottajalle. Päätös linkin valinnasta perustuu reititystaulun sen hetkiseen sisältöön.  Reitittimien lisäksi myös solmuilla (eli käyttäjien tietokoneilla, palvelimilla, yms.) on reititystaulu, jonka perusteella se tekee päätöksiä viestien lähettämisestä. Englanninkielisen wikipedian sivulla [Routing table](https://en.wikipedia.org/wiki/Routing_table) on esimerkki kotiverkkoon liitetyn tietokoneen reititystaulusta:

| Network Destination |	Netmask |	Gateway |	Interface |	Metric |
| ------------------- | --------------- | ------------- | -------------  | ------ |
| 0.0.0.0	| 0.0.0.0	    | 192.168.0.1	| 192.168.0.100 |	10 |
| 127.0.0.0 |	255.0.0.0	| 127.0.0.1	| 127.0.0.1	| 1 |
| 192.168.0.0 |	255.255.255.0 |	192.168.0.100 |	192.168.0.100 |	10 |
| 192.168.0.100	| 255.255.255.255 |	127.0.0.1	| 127.0.0.1	| 10 |
| 192.168.0.1 |	255.255.255.255 |	192.168.0.100	| 192.168.0.100 |	10 |

Tässä reititystaulussa on useita kenttiä. Ensimmäisenä on kohdeverkko (network destination) ja siihen liittyvä verkonpeite (netmask). Näillä yhdessä voidaan tunnistaa IP-paketin vastaanottajan osoitteen perusteella verkko, johon paketti pitää tältä laitteelta toimittaa. Huomaa, että kyseinen verkko ei välttämättä ole vastaanottajan oma verkko, vaan lähinnä suunta, johon tältä laitteelta tuohon osoitteeseen menevät viestit pitää lähettää. Yhdyskäytävä (gateway) kertoo siis "laitteen", jonka kautta kyseisen verkon voi saavuttaa. Rajapinta (interface) auttaa tunnistamaan paikallisen linkin, verkkokortin tai muun paikallinen rajapinnnan, jonka kautta liikenne yhdyskäytävälle kulkee. Viimeinen kenttä kustannus (metric) auttaa valitsemaan useammasta mahdollisesti vaihtoehdosta kustannusten kannalta parhaimman.

Koska tämä reititystaulu on kotiverkon laitteesta, niin siinä näkyy laitteen oma paikallinen, laitteen sisäinen osoite, 127.0.0.1 sekä laitteen oma IP-numero 192.168.0.100, jolla tunnistetaan laitteen oma verkkokortti. Lisäksi tästä reititystaulusta löytyy oletusyhdyskäytävän osoite 192.169.0.1, joka on kotiverkon NAT-reitittimen osoite.

Kuten huomaat, niin tässä reititystaulussa on varsin paljon rivejä. Jos voisimme katsoa internetin syövereissä olevan reitittimen reititystaulua, niin siinä näitä rivejä olisi paljon enemmän.

Käytämme seuraavissa tehtävissä yksinkertaisempaa reititystaulua, jossa kohdeverkko ja verkonpeite on yhdistetty, rajapinta on jätetty kokonaan pois ja yhdyskäytävä on saatettu korvata vain linkin numerolla. Joskus kustannuskin voi puuttua. Yllä oleva reititystaulu tällä tavalla yksinkertaistettuna voisi olla

| Osoite        |	linkki / (yhdyskäytävä/rajapinta) |
| ---------------- | --------------------- |
| x.x.x.x	(default)|  1 (192.168.0.1)	|
| 127.0.0.0/8 | 0 (127.0.0.1)	|
| 192.168.0.0/24 | 1  (samassa verkossa) |

Nyt 'x':llä merkityt osat osoitteesta ovat aliverkon sisäisiä laitenumeroita ja vain numerot kertovat reitityksen kannalta merkityksellistä tietoa osoitteesta. Koska käytämme luokatonta reititystä, niin reititin etsii reititystaulusta sen rivin, jonka osoitekentässä on pisin yhteinen alkuosa etsittävän osoitteen kanssa. Näin kaikki osoitteet, jotka eivät ala 127 tai 191.168.0 ohjataan reitittimelle ja viestit, jotka ovat omaan aliverkkoon 192.168.0.x kuuluvia, lähetetään vain aliverkkoon. Koneen sisäisiä viestejä 127 ei edes lähetetä verkkoon, vaan ne käsitellään koneen sisällä.

Joskus reititystaulusta (engl. router table) saatetaan käyttää englanniksi nimitystä forwarding table, ja joskus tuo uudelleenlähetystaulu on isommasta kaikki mahdolliset reitit kattavasta reititystaulusta poimittu osa, joka kattaa uudelleenlähetyksessä tarvittavien aktiivisten reittien tiedot. Suomeksi näistä molemmista käytetään yleensä  nimeä reititystaulu.

Seuraavassa tehtävässä tarvitset tätä reititystaulua:

| Osoite/peite  |	linkki |
| ---------------- | -------- |
| 140.24.7.0/26 |  m0 |
| 140.24.7.64/26 | m1 |
| 140.24.7.128/26 | m2 |
| 140.24.7.192/26 | m3 |
| 0.0.0.0./0 | m4 |

<quiz id="b2325dad-8de7-43fd-aed5-eb8f16a85487"></quiz>

Koska osoitteiden tulkinnan periaatteena on tuo pisin yhteinen alkuosa, on mahdollista yhdistellä reitittimen rivejä, jos ne kattavat yhdessä koko osoiteavaruuden. Esimerkiksi äskeisessä tehtävässä olleen reititystaulun osoitteet kaikki yhdessä voidaan kattaa osoitteella 140.24.7.0/24, joka voisi siis olla tälle reitittimelle liikennettä ohjaavan reitittimen reititystaulussa.

Luokaton reititys ja pisimmän yhteisen alkuosan käyttäminen antaa myös mahdollisuuden järjestellä verkon arkkitehtuuria uudelleen ilman, että laitteiden julkisia IP-osoitteita tarvitsee vaihtaa.  Edellä joku noista aliverkoista (vaikkapa 140.24.7.128/26) voitaisiin siirtää tästä reitittimestä jollekin muulle reitittimelle, vaikkapa tätä reititintä edeltävälle reitittimelle, jolloin sen reititystaulussa olisikin molemmat osoitealueet 140.24.7.0./24 ja 140.24.7.128/26, vaikka ne ovat päällekkäisiä. Se osaisi kuitenkin päätellä, että laitteelle 140.24.7.165 lähetetty viesti kuuluu lähettää tuon pienemmän osoitealueen 140.24.7.128/26 suuntaan eikä tuohon 140.24.7.0/24 suuntaan, koska sillä on  pidempi yhteinen alkuosa pienemmän verkon osoitteiden kanssa.  Suosittelen, että muunnat kaikki kolme osoitetta biteiksi ja lasket alkuosan yhteensopivien bittien lukumäärät ja näin varmistut siitä, että reititin oikeasti osaa tuon päätöksen tehdä näiden periaatteiden mukaan.

<quiz id="a1421fd0-806a-42c2-bc75-d52afe982742"></quiz>

Edellä jo tarkasteltiinkin osoiteavaruuden 140.24.7.0/24 jakamista neljään yhtä suureen osaan, joilla kullakin on 26 bittinen aliverkon peite, eli aliverkot eroavat toisistaan kahden bitin verran (bitit 24 ja 25, kun bitit numeroidaan alkaen nollasta). Olisimme ihan yhtä hyvin voineen jakaa osoiteavaruuden eri suuruisiin osiin esimerkiksi 140.24.7.0/25, 140.24.7.128/26 ja 140.24.7.192/26  tai 140.24.7.0/26, 140.24.7.62/26 ja 140.24.7.128/25. Nyt yksi alue kattaa puolet osoiteavaruudesta. Tarkasta binäärilukujen avulla, miksi emme voi yhdistää kahta keskimmäistä aluetta yhdeksi, vaan isomman yhtenäisen alueen pitää olla joko numeroavaruuden alussa tai lopussa.

<quiz id="a999dbaf-870e-4eb9-a8eb-e0323aa72347"></quiz>



### Tunnelointi

Voi halutessasi lukea tunneloinnista lyhyen kuvauksen englanninkielisen wikipedian sivulta [IP Tunnel](https://en.wikipedia.org/wiki/IP_tunnel)

Piirrä aluksi itsellesi kuva, jossa verkon reunoilla olevilla solmuilla A ja Z on IPv6:n mukaiset IP-osoitteet ja ne haluavat kommunikoida keskenään. Ne ovat siis eri aliverkoissa ja eri reunoissa piirrosta. Niiden aliverkot on yhdistetty internetiin reitittimillä (B ja Y), jotka osaavat aliverkon edellyttämää IPv6:sta ja internetin suuntaan ne osaavat myös IPv4:sta, koska reitittimet (C ja X), joiden kautta niiden viestit siirtyvät internetiin on vielä sen verran vanha, että se osaa vain IPv4:ää. Reitti C:n ja X:n välillä on olemassa, mutta sen yksityiskohdat eivät ole tiedossa.

Kuvassa on siis kuusi solmua A, B, C, X, Y ja Z, jotka on yhdistetty toisiinsa siten, että viestit A:lta voivat päätyä Z:lle ja päinvastoin.

Nyt A lähettää IPv6-paketin Z:lle. Tässä paketissa viestin lähettäjä on A ja vastaanottaja on Z. Koska reitti kulkee B:n kautta, niin tämä paketti ohjataan ensin B:lle. Reititin B ei voi suoraan lähettää viestiä eteenpäin, koska A:n lähettämä viesti on IPv6:n mukainen ja C osaa käsitellä vain IPv4:n mukaisia viestejä. Siksi "muodostetaan tunneli" B:n ja Y:n välille siten, että B laittaa IPv6:n  mukaisen paketin IPv4-paketin sisään dataksi. Tällä IPv4-paketilla lähettäjänä on B ja vastaanottajana Y. Nyt B:n ja Y:n välillä kulkee IPv4:n mukainen paketti, jonka C, X ja muut niiden välillä olevat reitittimet osaavat käsitellä ja ohjata oikeaan suuntaan.

Kun IPv4-paketti saapuu Y:lle, joka siis on kyseisen paketin vastaanottaja, niin Y havaitsee, että paketissa on sisällä IPv6-paketti. Y purkaa tuon IPv4 kuoren pois ja lähettää paljastuvan IPv6-paketin sen vastaanottajalle eli Z:lle.

Yllä on kuvattu niin sanottu [IP-in-IP](https://en.wikipedia.org/wiki/IP_in_IP) tunnelointi. Jo edeltävällä kurssilla käsittelimme virtuaalista yksityisverkkoa VPN:ää, jonka toiminta perustuu tunnelointiin. VPN:ssä tunnelissa voidaan kuljettaa useita eri protokollia, kun yllä kuvattu esimerkki IPv6:n lähettämisestä IPv4-paketissa koski vain yhtä protokollaa.

Yleensä tunneli toteutetaan käyttäen salaavaa [IPSec](https://fi.wikipedia.org/wiki/IPsec)-protokollaa, jolloin tunnelissa kulkeva tieto on salattua ja vain tunnelin muodostava kuori on salaamatonta. Se on tyypillisin protokolla myös VPN:n toteutuksessa. Joissakin suosituksissa korostetaan, että tunnelit pitäisi aina tehdä käyttäen salaavaa protokollaa, jolloin viestinnän turvallisuutta saadaan parannettua.

<quiz id="b425e36b-8f75-408b-94e9-ee2368bbc257"></quiz>


