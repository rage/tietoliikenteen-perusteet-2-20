---
path: '/osa-4/1-IPv4-IPv6'
title: 'IPv4 ja IPv6 osoitteet'
hidden: false
---


<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata IPv4 ja IPv6 osoitteet.
- Osaat selittää, miksi on siirrytty ja edelleen siirrytään IPv4 osoitteiden käytöstä kohti IPv6 osoitteiden käyttöä.

</text-box>


## Verkkokerroksen tehtävät

Verkkokerros huolehtii viestin välittämistä lähettäjältä vastaanottajalle. Verkkokerroksella toimivien laitteiden täytyy siis tietää jotain verkon rakenteesta ja siitä mistä päin verkkoa vastaanottaja löytyy.

Internetin etappivälitteisyys näkyy erityisesti verkkokerroksella. Jokainen verkkokerroksella toimiva laite voi vastaanottaa sille linkkikerroksella osoitettuja viestejä, joiden lopullinen vastaanottaja se ei ole. Kun laitteelle tulee viesti, jonka vastaanottaja se ei ole, on laitteella velvollisuus lähettää viesti edelleen kohti vastaanottajaa.

Verkkokerroksella laitteet tunnistetaan niiden IP-numeroista, joita usein myös IP-osoitteiksi kutsutaan.

## IP-osoite

IP-protokollasta on tällä hetkellä käytössä kaksi versiota perinteinen [IPv4](https://fi.wikipedia.org/wiki/IP) ja uudempi [IPv6](https://fi.wikipedia.org/wiki/IPv6). IPv4 on ollut Internetin verkkokerroksen välitysprotokolla koko Internetin olemassaolon ajan. Vieläkin kun puhutaan pelkästään IP:stä yleensä aina tarkoitetaan IP versiota 4.

Nämä versionumerot 4 ja 6 vaikuttavat oudoilta, mutta internetin dokumentaatiosta löytyy määrittelyt myös protokollan versioille 1,2,3 ja 5. Ne eivät ole käytössä.

## IPv4

IPv4:ssä [IP-osoitteen](https://fi.wikipedia.org/wiki/IP-osoite) pituus on 32-bittiä eli 4 tavua. Osoite ilmoitetaankin yleensä näiden tavujen numeroarvoina eli muodossa 10.12.34.216. Jokaisella internetiin suoraan liitetyllä laitteella täytyy olla uniikki osoite. Kuten jo tiedämme näitä osoitteita maailmanlaajuisesti hallinnoi IANA.

IP-osoite jaetaan aina kahteen osaan. Osoitteen alkuosa kertoo kyseistä osoiteavaruutta hallinnoivan operaattorin. Operaattori voi sitten jakaa hallitsemansa osoiteavaruuden osoitteet verkkolaitteille haluamallaan tavalla. Tämä rajapinta tunnistetaan verkon peitteen avulla. Verkonpeite on 32-bittinen sana, jonka alkupään bitit ovat ykkösiä ja loppupään bitit nollia. Ykkösbittien osa kertoo operaattorin tunnisteen ja nollabittien osa on sitten se alue osoitteista, joilla operaattorin hallinnoiman verkon laitteet tunnistetaan. Saman verkon laitteilla on aina sama alkuosa, jonka pituuden näkee suoraan tästä verkon peitteestä. Saman verkon laitteilla on kuitenkin oltava eri IP-osoitteet, jotta ne voidaan erottaa toisistaan. Verkon peite merkitään usein kauttaviivalla IP-osoitteen perään. Peite voi olla samassa muodossa kuin IP-osoite tai sitten se voidaan ilmaista kokonaisluvulla 0-32, jolloin kokonaisluku kertoo peitteen alkuosan ykkösbittien lukumäärän.

Alunperin näitä jakokohtia oli vain tavujen välissä ja silloin puhuttiin luokista A (ensimmäisen tavun jälkeen), B (toisen tavun jälkeen) ja C (kolmannen tavun jälkeen). Näiden perusluokkien lisäksi koko osoiteavaruuden lopusta on varattu osoitteita monilähetykseen (luokka D) ja tulevaan käyttöön (luokka E). Näihin käyttöihin varattuja osoitteita ovat IP-osoite 224.0.0.0 ja sitä suuremmat arvot. Käy katsomassa luokkakohtaiset IP-osoitteet wikipedian sivulta [IP-osoite](https://fi.wikipedia.org/wiki/IP-osoite).

Luokkajaon lisäksi sovittiin, että kaikki 127-alkuiset osoitteet ovat käytettävissä paikallisena osoitteena yhden verkkolaitteen sisällä. Tämä voidaan merkitä joko 127.0.0.0/255.0.0.0  tai 127.0.0.0./8.  Näistä erityisesti paikallisen koneen oma sisäinen osoite (engl. localhost) 127.0.0.1 tulee vastaan laitteen konfigurointitiedoissa. Huomaa, että tuo kauttaviivan jälkeinen merkintä /255.0.0.0 tai /8 kertoo samalla [aliverkon peitteen](https://fi.wikipedia.org/wiki/Aliverkko). Aliverkon peite kertoo kuinka monta monta bittiä osoitteen alusta on verkon tunnusta ja loput bitit ovat siis laitteiden erotteluun aliverkon sisällä. Verkon tunnistetta kuvastavat bitit ovat peitteessä ykkösiä ja verkon sisäistä vaihtuvaa osaa kuvaavat bitit nollia. Luvussa 255 on 8 bittiä, joten aliverkon peitettä 255.0.0.0. vastaava binäärinen esitys on 11111111 00000000 00000000 00000000. Tämän voi kirjoittaa lyhyemmin /8, jossa siis kerrotaan kuinka monta ykkösbittiä peitteessä on.

IPv4:n osoitteista on myös varattu tietyt alueet yksityisissä verkoissa käytettäviksi. Näillä yksityisverkojen osoitteilla ei saa liikennöidä julkisessa internetissä. Toisaalta ne voi ottaa käyttöön omassa suljetussa verkossa sen tarkemmin kysymättä keneltäkään.
Näistä ehkä eniten käytettyjä ovat  10.0.0.0/8 ja 192.168.0.0/16, koska ne noudattavat näitä vanhoja luokkarajoja. Kolmas yksityisverkon osoitealue 172.16.0.0/12 (tai 172.16.0.0/255.240.0.0)  eli osoitteet 172.16.0.1 – 172.31.255.255 on luokaton, koska siinä verkon peite katkeaa tavun sisällä.

Luokallisesta osoitteiden hallinnoinnista siirryttiin luokattomiin osoiterajoihin, koska näin saatiin joustavammin jaettua verkon osoiteavaruutta useammalle operaattoreille. Tätä kutsutaan termillä [luokaton reititys](https://fi.wikipedia.org/wiki/Luokaton_reititys) (engl. Classless InterDomain Routing, CIDR). CIDR:n mukaisesti operaattorin tunnisteen ja verkkolaitteen tunnisteen raja voidaan sijoittaa mihin kohtaan tahansa 32-bittisessä osoitteessa eikä vain tavurajoille.

Organisaatio saa aina käyttöönsä yhtenäisen osoitealueen. Alueen koon kertoo nimenomaan tuo aliverkon peite. Organisaatio saa edelleen jakaa tämän osoitealueen haluamallaan tavalla pienemmiksi aliverkoiksi. Otetaan esimerkiksi tuo yksityisverkon osoitealue 172.16.0.0/12. Voimme jakaa sen kahteen yhtäsuureen alueeseen 172.16.0.0/13 ja 172.24.0.0./13.  Miten nämä alueet on sitten laskettu? Otetaan tarkasteluun vain tuo toinen tavu, jossa muutoksia tapahtuu. 16 on binäärilukuna 10000, mutta koska meillä on kokonainen tavu, niin tavu on silloin 00010000. Kun teemme kaksi yhtä suurta aluetta, niin toinen alue on binäärilukuna 00010000 ja toinen 00011000 eli niiden ero on ykkösbitiä seuraavassa bitissä. Kun nämä muunnetaan takaisin kymmenjärjestelmän luvuiksi, niin saamme luvut 16 ja 24. Samalla myös aliverkon peitteen koko kasvoi yhdellä, koska siirsimme yhden bitin lisää verkkoja erottavaan osioon.  Jos haluamme jakaa sen kahdeksaan yhtä suureen alueeseen, niin silloin niiden osoitelueet ovat 172.16.0.0/15, 172.18.0.0./15, 172.20.0.0/15, ... 172.30.0.0/15. Tee tuo äskeisen kaltainen bittimuunnos itsenäisesti ja varmistu näin, että ymmärrät miten nuo verkkonumerot ja peitteet toimivat.

<quiz id="b51a8df2-9037-4de6-a12c-ef66d593d50c"></quiz>


## IPv6

[IPv6](https://fi.wikipedia.org/wiki/IPv6) osoitteet ovat tällä hetkellä käytössä Internetissä rinnan IPv4 osoitteiden kanssa. Valitettavasti nämä kaksi versiota eivät ole yhteensopivia, kuten seuraavassa aliluvussa näemme.

IPv6 osoitteet ovat 128-bittisiä eli huomattavan pitkiä verrattuna IPv4:n 32-bittisiin osoitteisiin. Tämän seurauksena osoitteita on käytettävissä yli 340 sekstiljoonaa.

IPv6:n osoitteet esitetään kahdeksana hexadesimaaliryhmänä, joissa kukin 4-merkkinen hexadesimaaliluku kuvaa 16 bitin mittaista osiota osoitteesta. Muistathan, että hexadesimaalilukujen kantaluku on 16, joten yhdellä hexadesimaaliluvulla voi kuvata 4-bittisen luvun. Nämä 4-merkkiset hexadesinaaliluvut erotetaan toisistaan kaksoispisteellä. Esimerkki osoitteesta 2001:0DB8:AC10:FE01:0000:0000:0000:0000.  Osoitetta kirjoitettaesssa alkunollat voi jättää pois ja nolla-alueen voi jättää tyhjäksi. Kaikki 8 osiota on kuitenkin tunnistettava, joten kaksoispisteitä ei saa jättää pois. Äskeinen osoite mahdollisimman tiiviisti kirjoitettuna on 2001:DB8:AC10:Fe01::::.

<a title="Ipv6_address.svg: Indeterminate
derivative work: BobbyPeru / Public domain" href="https://commons.wikimedia.org/wiki/File:Ipv6_address_leading_zeros.svg"><img width="512" alt="Ipv6 address leading zeros" src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/Ipv6_address_leading_zeros.svg/512px-Ipv6_address_leading_zeros.svg.png"></a>

KUVA: Kuva osoitteen purkamisesta (Lähde: wikimedia)

Koska IPv6:n osoiteavaruus valtava, on erikseen sovittu, että yksittäistä osoitetta operaattorilta pyytävä reititin saa tyypillisesti oman aliverkon, jonka peite on 64 bittiä. Esimerkiksi juuri edellä kuvatun osoitteen. Tähän aliverkkoon liittyvä laite puolestaan saa reitittimeltä aliverkon tunnisteen ja aliverkon peitteen. Osoitteen loput bitit (tyyppillisesti siis jälkimmäiset 64 bittiä) se voi määritellä itse. Automaattisessa konfiguroinnissa voidaan käyttää vaikkapa laitteen MAC-osoitetta apuna. MAC-osoitteen tavoitehan on yksilöidä laite linkkikerroksen tasolla. MAC-osoitteen pituus on 48 bittiä, joten se mahtuu vallan hyvin laitteen aliverkossa yksilöivään 64:ään bittiin. Itseasiassa osoitteen automaattiseen konfigurointiin on määritelty menetelmä, jolla MAC-osoitteen voi muuntaa IPv6 osoitteen osaksi laitteen (tai oikeammin sen verkkoyhteyden) tunnisteeksi.

Näiden isojen aliverkkojen tavoitteena on helpottaa iPv6-osoitteiden reititystä ja pitää reititystaulut kooltaan siedettävinä.

Siirtymä IPv6:n käyttöön on vahvasti käynnissä. Esimerkiksi googlen tilastojen mukaan maailmanlaajuisesti jo noin neljännes sen sivuille tulevista yhteyksistä käyttää IPv6:ta. IPv4-osoitteet on kaikki jaettu ja jos operaattorilta loppuvat osoitteet, se joutuu ostamaan niitä lisää joltain muulta. Tämä aiheuttaa näille operaattoreille paineita siirtyä käyttämään IPv6:tta, koska siellä on runsaasti osoitteita. Jos tilastotieto IPv6:den yleisyydestä kiinnostaa, niin verkkosivulta https://www.internetsociety.org/deploy360/ipv6/statistics/ löytyy linkkejä erilaisiin tilastoihin.


## Vastaanottajien määrä? - Erilaiset kommunikointitavat

Tällä kurssilla olemme keskittyneet etupäässä viestien siirtoon yhden lähettäjän ja yhden vastaanottajan välillä. Tämän [täsmälähetyksen](https://fi.wikipedia.org/wiki/T%C3%A4sm%C3%A4l%C3%A4hetys) (engl. unicast) lisäksi käytettävissä on myös [yleislähetys](https://fi.wikipedia.org/wiki/Yleisl%C3%A4hetys) (engl. broadcast) ja [ryhmälähetys](https://fi.wikipedia.org/wiki/Ryhm%C3%A4l%C3%A4hetys) (engl. multicast). Nämä kolme viestien lähetystapaa ovat käytössä IPv4:ssä. IPv6:ssa tarjolle tulee monilähetyksen erikoistapaus [jokulähetys](https://fi.wikipedia.org/wiki/Jokul%C3%A4hetys) (engl. anycast) ja poistuu mahdollisuus yleislähetykseen kaikille saman verkon laitteille.

Täsmälähetyksessä lähettäjä lähettää viestin täsmälleen yhdelle vastaanottajalle, joka tunnistetaan vastaanottajan IP-osoitteella. Tämä on yleisin tapa viestiä internetissä laitteiden välillä.

Yleislähetyksessä lähettäjä lähettää viestin kaikille laitteille, jotka voivat teknisesti kuulla sen lähettämät viestit. Tämä kuormittaa verkon toimintaa merkittävästi ja siksi on määritelty, että yleislähetykset ovat sallittuja vain saman aliverkon sisällä. IPv4:ssä on yksi täysin geneerinen yleislähetysosoite 255.255.255.255, jota mikään reititin ei välitä eteenpäin.  Jos samaan aliverkkoon kuuluvia osia on reitittimen eri puolilla ja yleislähetyksen on tarkoitus tavoittaa kaikki kyseisen aliverkon laitteet, niin pitää käyttää kyseisen aliverkon omaa yleislähetysosoitetta, jossa alkuosa on aliverkon tunniste ja loppuosa on pelkkiä ykkösbittejä. Wikipedian sivulla [yleislähetys](https://fi.wikipedia.org/wiki/Yleisl%C3%A4hetys) on yksi esimerkki tästä, käy lukemassa se.

Ryhmälähetystä käytetään silloin, kun lähettäjä haluaa lähettää vain yhden viestin, mutta toivoo sen menevän perille useammalle vastaanottajalle. Tyypillinen ryhmälähetystä käyttävä internetpalvelu on IPTV. Ei ole järkevää, että lähettäjä lähettäisi saman viestin erikseen jokaiselle vastaanottajalle. Se kuormittaisi sekä lähettäjää että verkko tarpeettomasti, kun kuitenkin viestin sisältö on täysin identtinen kaikille vastaanottajille. IPv4:ssä osoitteet 224.0.0.0 – 239.255.255.255 (eli luokka D) on varattu näille ryhmälähetyksille. IPv6:ssa puolestaan ryhmälähetys on otettu huomioon jo protokollan suunnitteluvaiheessa siten, että siinä on tuki useammille erilaisille ryhmille.  IPv4:ssä D-luokan osoitteita voi käyttää ryhmälähetyksissä vain saman aliverkon sisällä. Tällöin käytetään yleensä osoitetta 224.0.0.1. IPv6:ssa on varattu osoite ff02::1::::: tätä vastaavaan käyttöön.

Jokulähetys on tavallaan ryhmälähetyksen tai yleislähetyksen erikoistapaus. Siinä mahdollisia viestin vastaanottajia on useita, mutta yksi viesti päätyy kuitenkin vain yhdelle vastaanottajalle. Lähettäjä ei lähettäessään välitä mille ryhmän jäsenelle viesti päätyy, jokainen niistä osaa asian käsitellä. Tämän tyyppinen viestinlähetys sopii sellaiseen tilanteeseen, jossa palvelin on kuormituksen vuoksi monistettu ja kaikki kopiot ovat toiminnassa. Internetin nimipalvelu on hyvä esimerkki tämän kaltaisesta tilanteesta. Lähettäjä haluaa vastauksen nimipalvelukysymykseensä, mutta sitä ei kiinnosta, mikä auktorisoiduista nimipalvelijoista voisi siihen vastata. WWW-palvelu on toinen hyvä esimerkki. Isoilla toimijoilla on palvelimia eri puolilla internetiä ja käyttäjälle riittää, että joku niistä vastaa kyselyyn, mutta käyttäjälle ei ole väliä mistä vastaus tarkemmin ottaen tulee.

<quiz id="a53ea914-8396-4db0-87eb-da700b8d26de"> </quiz>




