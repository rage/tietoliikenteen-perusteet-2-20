---
path: '/osa-2/4-sahkoposti'
title: 'Sähköposti'
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kuvata sähköpostipalvelun.
- Osaat kuvata SMTP protokollan toimintaa ja sen käyttöä sähköpostipalvelussa.
- Osaat perustella miksi lähettäjällä ja vastaanottajalla on käytössään eri protokollat.

</text-box>



## Sähköpostipalvelu

Kaikki me käytämme sähköpostipalvelua, mutta kuinka moni meistä osaa kertoa mitä sähköpostinjärjestelmässä oikeasti tapahtuu. Toki myös lähetämme tavallisia kirjeitä ja postikortteja, emmekä silloinkaan osa tarkasti kertoa miten kortti kulkee lähetyspaikasta vastaanottajalle. Tulevia tietotekniikka-alan osaajina tämän kurssin osallistujien pitää kuitenkin tietää edes yleisperiaatteet sähköpostipalvelun toiminnasta.

Sähköpostipalvelu, kuten useimmat muutkin internetin palvelut, perustuu erillisten sähköpostipalvelimien käyttöön. Nykyään käyttäjät käyttävät näitä palvelimia erityisten asiakasohjelmistojen kautta. Käyttäjän omalla koneella (tai kännykällä) voi olla erillinen sähköpostiohjelma (kuten Thunderbird, Outlook) tai www-selaimessa toimiva sähköpostipalvelu (kuten gmail, operaattorien sähköpostipalvelut). Tällä kurssille oletetaan, että käytetään erillistä sähköpostiohjelmaa. WWW-palvelimen voi ajatella vastaavaan rooliin sähköpostipalvelimien suhteen.

Sähköpostipalvelimet välittävät viestejä keskenään käyttäen yksinkertaista sähköpostin välitysprotokollaa (Simple Mail Transfer Protocol, SMTP) tai siitä edelleen kehitettyä laajempaa protokollaa (Extended SMTP, ESMTP).

Asiakasohjelma lähettää viestit sähköpostipalvelimelle käyttäen samaa SMTP protokolla kuin sähköpostipalvelin, kun se lähettää sähköpostin toiselle sähköpostipalvelimelle. SMTP-protokolla käytetään siis vain viestin lähettämiseen. Siinä lähettäjä ottaa yhteyden vastaanottajaan ja siirtää sähköpostin vastaanottajalle. Tällaisia protokollia, joissa yhteyden muodostanut lähettäjä 'työntää' viestin vastaanottajalle, kutsutaan englanninkielisen nimen mukaan PUSH-protokolliksi. Valtaosa tällä kurssilla käsiteltävistä protokollista on kuitenkin PULL-protokollia. Niissä yhteyden muodostaja 'vetää' tietoa itselleen. Myös asiakasohjelman käyttämät protokolla POP3 ja IMAP ovat PULL-tyyppisiä protokollia. Niiden avulla asiakasohjelma noutaa käyttäjän sähköpostilaatikon sisällön luettavaksi käyttäjän koneelle.

Käyttäjällä on siis sähköpostiosoite, kuten tiina.niklander@helsinki.fi, jota muut käyttäjät voivat käyttää vastaanottajan osoitteena, kun he haluavat lähettää sähköpostia kyseiselle käyttäjälle. Sähköpostiosoitteessa on kolme elementtiä: käyttäjän tunniste (tiina.niklander), ät-merkki ja postipalvelimen tunniste (helsinki.fi). Ät-merkin käytöstä voit lukea lisää [kielitoimiston ohjepankin ohjeesta](http://www.kielitoimistonohjepankki.fi/ohje/20).

Käyttäjän tunnisteet yksilöivät käyttäjät vain yhden sähköpostipalvelimen sisällä. Sama käyttäjätunniste voi olla käytössä useammassa eri sähköpostipalvelimessa. Niitä voi hallinnoida yksi ja sama henkilö tai ne voivat kuulua useammalle eri henkilölle.

Sähköpostipalvelimen tunniste helsinki.fi ei ole sähköpostipalvelimen oma verkkonimi, vaan sähköpostijärjestelmässä käytettävä tunniste. Sähköpostipalvelimen verkkonimen voi selvittää nimipalvelun avulla. Nimipalvelun resurssitietueessa MX on tieto siitä, mikä sähköpostipalvelin vastaa tietyn verkkoalueen (kuten helsinki.fi) sähköpostipalvelusta. Sähköpostiosoitteet sitoutuvat siis verkkonimiin siten, että tuo sähköpostipalvelimen tunniste on yleensä samalla verkkoalueen nimi. Siksi voimme käyttää aluenimeä sähköpostiosoitteessa varsinaisen palvelimen nimen sijaan.

<quiz id="4c9f0a90-72c1-4219-a4c4-4838a4f859dc"></quiz>

## Sähköpostin lähetys ja vastaanotto

Käyttäjän oma sähköpostipalvelin toimii usein käyttäjän postilaatikkona. Eli käyttäjälle saapuvat viestit tulevat sähköpostipalvelimelle käyttäjän omaan postilaatikkoon. Usein käyttäjä myös lähettää sähköpostit oman sähköpostipalvelimensa kautta.

Koska sähköposti on vanhimpia internetin palveluja, niin se on alunperin suunniteltu vain palvelinten väliseen toimintaan. Aikoinaan käyttäjät käyttivät omia sähköpostilaatikoitaan suoraan palvelimilla komentotulkkien avulla. Nykyisin käyttäjät käyttävät omia sähköpostilaatikoitaan muilta laitteilta ja käyttäjien laitteiden (=asiakas) ja sähköpostipalvelimen välille on kehitetty uusia protokollia.

Keskitytään hetkiseksi vain sähköpostipalvelimien toimintaan ja otetaan vasta myöhemmin mukaan sähköpostipalvelimen ja asiakkaan välinen kommunikointi.

<img src="../img/2-4-postipalvelimet.svg" alt="Kuvassa on kolme sähköpostipalvelinta helsinki.fi, google.com ja rutgers.edu. Jokaisella palvelimelle on joukko nimeämättömiä postilaatikoita. Palvelimet on liitetty internettiin, joka välityksessä ne voivat kommunikoida keskenään. Yhteyksiä ei ole tarkemmin kuvattu. ">

KUVA: Kuvassa on muutama sähköpostipalvelin ja niillä joitakin postilaatikoita.

Sähköpostipalvelimet välittävät käyttäjien toisille käyttäjille lähettämiä viestejä SMTP (Simple Mail Transfer Protocol) protokollalla. Esimerkiksi, jos helsinki.fi:n sähköpostipalvelimella on sen käyttäjän rutgers.edu:n palvelimella olevalle käyttäjälle lähettämä viesti, niin helsinki.fi:n palvelin ottaa SMTP yhteyden rutgers.edu:n palvelimelle ja siirtää käyttäjän lähettämän viestin sinne.

Jos käyttäjällä on omalla koneellaan joku erillinen sähköpostiohjelma kuten outlook, thunderbird tai vastaava, niin nämä ohjelmat tyypillisesti käyttävät joko [IMAP](https://fi.wikipedia.org/wiki/IMAP) (Internet Message  Access Protocol) tai [POP3](https://fi.wikipedia.org/wiki/POP3) prokollia käyttäjän palvelimella sijaitsevan sähköpostilaatikon käsittelyyn.  Huomaa, että näillä protokollilla käsitellään vain jo sähköpostilaatikossa olevia viestejä. Näillä siis luetaan saapuneita viestejä. Viestien lähettämiseen sähköpostiohjelmat käyttävät samaa SMTP protokollaa kuin sähköpostipalvelimetkin.

Näillä sähköpostiprotokollilla on hyvä havainnollistaa protokollien yleisempääkin piirrettä eli PUSH vs PULL. PUSH-tyyppisissä prokollissa, kuten SMTP, viestin tai datan vastaanottajalla ei ole juurikaan sanavaltaa. Toinen osapuoli vain antaa sille dataa, joka pitää ottaa vastaan. Lähettävä sähköpostipalvelin (asiakas-palvelin -mallin mukaan asiakas) ottaa yhteyttä vastaanottavaan sähköpostipalvelimeen (asiakas-palvelija -mallin mukaan palvelin) ja työntää uuden saapuvan viestin sinne. Tähän sähköpostijärjestelmän ominaisuuteen perustuu suuri osa roskapostin toiminnasta. Palvelimille vain työnnetään roskapostia, joka on menossa käyttäjien postilaatikoihin. PULL-tyyppisissä protokollissa, kuten IMAP ja POP3, viestin tai datan vastaanottaja päättää mitä ja milloin se haluaa vastaanottaa.

Nykyään on yhä tyypillisempää, että käyttäjät hyödyntävät www-selaimia omien postilaatikoidensa käsittelyssä. Tällöin käyttäjän selaimen ja www-palvelimen välinen liikennöinti tapahtuu HTTP-protokollalla. WWW-palvelin pääsee käsiksi käyttäjän postilaatikkoon joko samoilla protokollilla (IMAP, POP3) kuin käyttäjän koneen sähköpostiohjelma tai se voi olla samassa järjestelmässä postipalvelimen kanssa, jolloin se voi lukea käyttäjän postilaatikon sisällön suoraan tiedostojärjestelmästä.

Huomaa, että sähköpostijärjestelmässä protokollia käytetään epäsymmetrisesti, koska SMTP on vain postien lähettämistä ja IMAP/POP3 sähköpostien lukemista varten. Aiemmissa käsittelemissämme järjestelmissä käytössä oli vain yksi protokolla, jotka käytettiin viestien välittämisen molempiin suuntiin.  Sähköpostijärjestelmässäkin kaikki protokollat oman yhteytensä aikana välittävät viestejä molempiin suuntiin. Epäsymmetrisyys tässä johtuu toisaalta historiasta ja toisaalta näiden tapahtumien ajallisesta etäisyydestä. Sähköpostin saapuminen postilaatikkoon voi tapahtua ajallisesti paljon ennen kuin käyttäjän asiakasohjelma käy sen postilaatikosta lukemasta.  Jatkossa keskitymme vain sähköpostien lähettämisessä käytettävään SMTP protokollaan ja siihen liittyviin muihin seikkoihin.


## SMTP

SMTP (Simple Mail Transfer Protocol) on sähköpostijärjestelmässä käytetty siirtoprotokolla, jolla sähköposti siirretään vastaanottajan sähköpostipalvelimelle. Sitä voidaan käyttää myös sähköpostiviestin siirtämiseen viestiä lähettävän käyttäjän sähköpostiohjelmasta lähettäjän omalle sähköpostipalvelimelle viestin edelleenlähetystä varten. SMTP-protokolla on suunniteltu vain viestin siirtämiseen lähettäjältä vastaanottajalle. Siksi sitä ei voida käyttää viestin vastaanottavan käyttäjän sähköpostiohjelmassa viestin noutamiseen lukemista varten. Viestien noutamiseen onkin myöhemmin kehitetty erillisiä protokollia kuten POP3 ja IMAP.

Nykyisin käytössä on lähinnä SMTP:stä edellen kehitetty ESMTP (Extended SMTP).  Tällä kurssilla ei paneuduta tähän laajennukseen, vaan opiskellaan sähköpostijärjestelmän toimintaperiaate tuon alkuperäisen SMTP:n kanssa, vaikka nykyään postipavelimien suositellaan käyttävän aina tuota laajennosta. Jos jotakuta laajennus kuitenkin kiinnostaa, niin [suomenkielinen wikipedian sivu](https://fi.wikipedia.org/wiki/ESMTP) on hyvä lähtökohta asian itsenäiselle tutkimiselle.

SMTP-protokollassa on määritelty monivaiheinen viestien vaihto lähettäjän ja vastaanottajan välillä. Niiden täytyy ylläpitää tilatietoa, jotta ne pysyvät kärryillä, missä vaiheessa viestien vaihto on menossa. Tässä SMTP poikkeaa HTTP-protokollasta. HTTP-protokolla, kuten muistamme, oli tilaton, jolloin lähettäjän ja vastaanottajan ei tarvinnut ylläpitää tilatietoa.

Oheinen yhteysesimerkki, eli yhden sähköpostiviestin siirto lähettävältä palvelimelta vastaanottavalla postipalvelimelle, on mukaeltu Visa Hännisen insinöörityössä ["SMTP-releointipalvelun valvonta ja raportointi"](https://www.theseus.fi/bitstream/handle/10024/23284/Hanninen_Visa.pdf?sequence=1&isAllowed=y), Metropolia 2010, olleesta esimerkistä.

* A (lahettava.palvelin.com) avaa TCP-yhteyden B:lle (email.vastaanottaja.com)
* A:   HELO lahettava.palvelin.com
* B:   250 email.vastaanottaja.com Helo lahettava.palvelin.com, nice to interact with you
* A:   MAIL FROM: <joku123@ lahettava.palvelin.com>
* B:   250 sender <joku123@ lahettava.palvelin > OK
* A:   RCPT TO: <kuka123@postipalvelin.com>
* B:   250 recipient <kuka123@ postipalvelin.com > OK
* A:   DATA
* B:   250 OK; enter text, end with .
* A:   Tämä on viestin sisältö.
* A:   .
* B:   250 postipalvelin.com OK; message accepted for delivery
* A:   QUIT

Tästä esimerkistä käy hyvin ilmi monivaiheinen viestien vaihto. Tässä esimerkissä sähköpostin lähettäminen onnistuu. Protokollassa on määritelty tarkasti myös erilaiset virhetilanteet ja niihin liittyvät viestit. SMTP on HTTP-protokollan tavoin tekstipohjainen, jolloin ihmisen on mahdollista itsekin ottaa TELNET-yhteys palvelimeen ja kirjoittaa nuo lähettävän palvelimen viestit ihan suoraan näppäimistöltä. Tällaisessa yhteydessä on toki noudatettava myös niitä sääntöjä, jotka liittyvät itse viestin rakenteeseen. Esimerkissä näistä säännöistä on yksinkertaisuuden vuoksi oikaistu.

Huomaa, että sähköpostiprotokolla edellyttää tilatietoa. Molempien kommunikoinnin osapuolten pitää tietää missä vaiheessa (eli missä tilassa) kommunikointi ja viestienvaihto on menossa, jotta ne osaavat lähettää seuraavat viestit oikein.



## Viestin rakenne

Sähköpostiviestit eivät yleensä ole ihan noin lyhyitä kuin tuossa esimerkissä, jossa viesti on vain yhden rivin mittainen "Tämä on viestin sisältö." Yleensä viestit ovat pidempiä. Koska sähköpostiprotokolla on tekstimuotoinen ja koska viestin on sovittu päättyvän riviin, jossa on vain yksi merkki, piste, on kaikki sähköpostiin liitettävä muu materiaali kuin ASCII-muotoinen teksti koodattava ASCII-merkistölle sopivaksi.

Sähköpostiviestin rakenteesta on hyvä kuvaus [wikipedian sivulla](https://fi.wikipedia.org/wiki/S%C3%A4hk%C3%B6posti).

Useimmat sähköpostiohjelmat käyttävät erillisiä kenttiä sähköpostin otsaketiedoille, kuten vastaanottaja(t) ja viestin otsikko. Näin ne tukevat käyttäjää siinä, että lähtevän sähköpostiviestin otsaketiedot noudattavat sovittuja käytänteitä. Sähköposti on kuitenkin vanha protokolla, jota voisi ihan yhtä hyvin käyttää suoraan komentoriviltä toimivilla tekstipohjaisilla ohjelmilla.

<quiz id="aab49445-87f0-40f3-a919-e1a7f581eb2c"></quiz>

Katso jonkun saamasi sähköpostiviestin täydet otsaketiedot ja etsi sieltä rivit, jotka alkavat "Received:". Huomaa, että jos viesti on tullut saman postipalvelimen joltain toiselta käyttäjältä, niin tuota received-riviä ei välttämättä ole lainkaan, koska postipalvelin ei ole lähettänyt sitä toiselle postipalvelimelle vaan on vain laittanut viestin kyseisen käyttäjän postilaatikkoon. Valitse siis tarkasteltavaksi viesti, joka on tullut jonkun toisen postipalvelimen käyttäjältä. Miten näet viestin otsaketiedot, riippuu käyttämästäsi sähköpostiohjelmasta. Toiminto voi olla 'view full header', 'view source', 'view original'. Joillakin, erityisesti mobiililaitteiden, sähköpostiohjelmilla tämän tiedon katselu ei välttämättä edes ole mahdollista, kun ohjelmassa ei kyseistä toiminallisuutta ole. Vaihda silloin käyttöösi sellainen käyttöliittymä tai ohjelma, jolla nämä tiedot näet.

<quiz id="a704808e-8500-4435-8b3a-dcc7faed2b3c"></quiz>

Sähköpostiviestiin voi nykyään liittää myös muuta materiaalia kuin vain varsinaisen viestin pelkkänä tekstinä. Tällaiset laajennukset on määritelty MIME-standardissa (ks [standardin wikipedia-sivu](https://fi.wikipedia.org/wiki/MIME) ja [sähköpostin koodausohjeistus](https://fi.wikipedia.org/wiki/S%C3%A4hk%C3%B6posti#MIME)). MIME-muotoisessa sähköpostissa on useita osia ja jokainen osa voidaan koodata eri tavalla. Viestin liitteeksi voi laittaa minkä tahansa tiedoston, jonka tyyppi kerrotaan MIME-standardin mukaisissa tiedoissa.  Koska SMTP-protokolla siirtää varmasti vain 7-bittisiä ASCII-merkkejä, on tyypillistä, että erityisesti liitetiedostot ja kuvat koodataan 7-bittiseksi ASCII-koodiksi. Tunnetuin koodaustapa on base64, jossa kolme 8-bittistä tavua (tai merkkiä) koodataan 4:ksi, jotka eivät voi rikkoa sähköpostin siirtoa. Esimerkiksi merkkiyhdistelmä 'rivinvaihto' 'piste' 'rivinvaihto' ei ole sallittu, koska se on sovittu SMTP-protokollassa sähköpostiviestin päättäväksi merkiksi.


## Sähköpostin ongelmia

Huomasithan, että lähettäjän ja vastaanottajan tiedot toimitettiin kahteen kertaan. Toisaalta ne olivat SMTP-protokollan viestien vaihdossa mukana ja toisaalta ne tulivat sitten varsinaisen sähköpostin alussa otsaketietoina kentissä From: ja To:.

Sähköpostin vastaanottava sähköpostipalvelin ei tarkasta, että protokollassa annetut osoitteet ja viestin mukaan tulleet osoitteet ovat samat.  Mieti hetki, että miksi ei! Jos tarkastaisi, niin mitä tapahtuisi, kun viestiä lähetetään postilistalle esim. meidanjoukko@osoite.com.

Valitettavasti tämä tietojen kirjaaminen kahteen kertaan antaa mahdollisuuden piilottaa todellinen lähettäjä. SMTP-protokollan mukainen lähettäjän tunniste voi siis olla eri kuin viestin mukana olevaan From: kenttään kirjoitettu tunniste. Tietenkään normaalisti toimiva sähköpostiohjelma ei anna käyttäjälle mahdollisuutta näiden erilaisuuteen. Aikoinaan, kun käytettiin yksinkertaista SMTP-protokollaa ja luotettiin viestien lähettäjiin ei ollut mitenkään vaikeaa väärentää viestejä. Nykyisin se on jo hiukan hankalampaa, kun luotettavat sähköpostipalvelimet vaativat tunnistautumista. Sähköpostipalvelin voikin kieltäytyä vastaanottamasta yhteyspyyntöjä ja viestejä epäluotettavilta palvelimilta. Sähköpostipalvelimet voivat itse ylläpitää tällaista [mustaa listaa](https://fi.wikipedia.org/wiki/Musta_lista_(s%C3%A4hk%C3%B6posti)) tai ne voivat käyttää jonkun toisen organisaation ylläpitämää mustaa listaa epäluotettavista palvelimista.

Kaikkea [roskapostia](https://fi.wikipedia.org/wiki/Roskaposti) tai [haittaohjelmien](https://fi.wikipedia.org/wiki/Haittaohjelma) lähettämistä mustien listojen käyttöllä ei voi estää. Sähköpostipalvelimien ylläpitäjät ottavat koko ajan käyttöön uusia suojautusmiskeinoja. Valitettavasti mikään suojautuminen ei tepsi järjestelmän todellisten käyttäjätunnusten väärinkäyttöön. Aina on mahdollista, että luvattomasti käyttöön saatua tunnusta käytetään tällaisten haitallisten sähköpostien lähettämiseen luotettavan sähköpostipalvelimen kautta. Siksi käyttäjien pitää pitää hyvää huolta tunnuksistaan. Vaikka itsellä ei olisikaan arvokasta tietoa, niin tunnuksen luvaton käyttö voi aiheuttaa merkittävää haittaa muille internet-verkon käyttäjille.


## Viestien suojaus ja allekirjoitus (salaus)

Jos viestin lähettäjä haluaa varmistua, että vain viestin vastaanottaja voi lukea viestin, täytyy lähettäjän salakirjoittaa viesti jotain salausmenetelmää käyttäen. Vastaanottajan täytyy tietää, miten viesti on salattu, jotta hän voi sen purkaa ja lukea alkuperäisen sisällön. Sähköpostijärjestelmässä viesti kulkee sellaisenaan, joten ilman salausta kuka tahansa, joka viestin saa käsiinsä, voi sen lukea.

Viestien salaus ei sinänsä ole sidoksissa sähköpostijärjestelmään, mutta usein sähköpostia käytetään näiden viestien siirtoon lähettäjältä vastaanottajalle. Tässä kuitenkin tarkastellaan asiaa sähköpostipalvelun kautta. Asioiden yleispätevyys on lukijan kuitenkin hyvä huomata.

Sähköpostijärjestelmän MIME-laajennus ja [julkisen avaimen salausmenetelmät](https://fi.wikipedia.org/wiki/Julkisen_avaimen_salaus) antoivat riittävän teknisen pohjan [PGP-järjestelmälle](https://fi.wikipedia.org/wiki/PGP). PGP oli pitkään suosituin viestein luotettavuuden takaamiseen käytetty menetelmä sähköpostien kanssa.

Sittemmin MIME:ä laajennettiin myös käsittelemään salattuja viestejä. [S/MIMEn](
https://fi.wikipedia.org/wiki/S/MIME) versio 3.1 on kuvattu [RFC:ssä 3851](https://tools.ietf.org/html/rfc3851).

Julkisen avaimen salauksen avulla voidaan varmistua joko vastaanottajasta tai lähettäjästä seuraavasti:
1) Viestin voi lukea vain oikea vastaanottaja!  Viesti salataan vastaanottajan julkisella avaimella.
2) Viestin lähettäjä on varmasti oikea!  Viesti salataan lähettäjän salaisella avaimella. Tämä on digitaalinen allekirjoitus.

[Digitaalisessa allekirjoituksessa](https://fi.wikipedia.org/wiki/Digitaalinen_allekirjoitus) ei aina ole välttämätöntä suojata koko sisältöä ja siksi usein varsinaisesta sisällöstä lasketaan ns. tiiviste ja sitten salataan (eli allekirjoitetaan) vain tämä tiiviste.

Jos halutaan samaan aikaa varmistaa sekä lähettäjä että vastaanottaja, on viesti salattava useampaan kertaan.

Kiinnostuneille tiedoksi, että vuonna 2019 määriteltiin myös postipalvelimien käyttämä autentikointien ketjutus (engl. [Authenticated Received Chain](https://en.wikipedia.org/wiki/Authenticated_Received_Chain), ARC, jolla pyritään tukemaan postilistojen toimintaa silloin, kun postilistan käsittelee joku muu postipalvelin kuin alkuperäisen viestin lähettäjän oman palvelin.



