---
path: '/osa-3/4-vaihtoehtoja'
title: 'Suunnitteluperiaatteita'
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- tunnistat mitä suunnitteluperiaatteita protokolla noudattaa.
- Osaat arvioida miten protokollan toiminnassa on otettu huomioon tiettyjä suunnitteluperiaatteita.

</text-box>


## Protokollan suunnittelussa huomioitavaa

Aina kun suunnittelemme protokollaa tai selvitämme jonkun protokollan toimintaa, on hyvä muistaa, että kaikkien protokollien toiminnassa on paljon yhteisiä piirteitä ja toki myös paljon eroja. Keskitytään tässä protokollien yleisiin piirteisiin.

Nyt kun olemme tutustuneet protokollien mallinnukseen ja kuljetuskerroksen protokollien tavoitteisiin, on aika käydä systemaattisemmin läpi protokollien yleiset piirteet eli niiden suhtautuminen viestien oikeellisuuteen, katoamiseen ja siihen liittyviin kuittauksiin ja ajastimiin sekä viestien järjestyksen hallintaan eli järjestysnumeroihin.

## Oikeellisuustarkistus

Koska viestin kuljetuksessa lähettäjältä vastaanottajalle on aina riski esimerkiksi bittivirheille, on protokollan tehtävä huolehtia mahdollisesta viestin oikeellisuuden tarkistuksesta. Kuljetuskerroksen protokolla voi toki jättää viestin oikeellisuuden tarkistamisen sovelluskerroksen protokollan tehtäväksi, mutta yleensä jokainen kerros vastaa vähintään oman otsakkeensa oikeellisuuden tarkistuksesta.

Viestin muuttumattomuus voidaan tarkistaa joko pelkästään virheen havaitsevalla menetelmällä tai menetelmällä, joka voi tarvittaessa myös korjata virheen. Lähettäjä laskee tarvittavan lisätiedon osaksi viestiä ja vastaanottaja tarkistaa, että viesti ei ole muuttunut laskemalla tämän tiedon uudelleen ja vertaamalla sitä viestin mukana tulleeseen tietoon.

Viestin tarkistaminen edellyttää aina tämän lisätiedon eli [tarkistussumman](https://fi.wikipedia.org/wiki/Tarkistussumma) lisäämistä viestiin. Tarkistussummalle varataan kiinteänkokoinen osa protokollan viestistä. Tarkistussumman laskentaan ja tarkistamiseen on tarjolla useita erilaisia menetelmiä.

Jos tyydytään virheen havaitsemiseen, niin silloin ainoa vaihtoehto on hävittää vioittunut paketti. Mikä tahansa laite joka vastaanottaa paketin, jonka tarkistussumma ei vastaanottajalla enää vastaa viestin sisältöä, hävittää tällaisen paketin. Virheen havaitsemiseen voidaan käyttää ainakin jotain seuraavista:
* [pariteetti](https://fi.wikipedia.org/wiki/Pariteetti_(tietotekniikka)) on näistä menetelmistä yksinkertaisin ja se voidaan laskea joko bitti tai tavutasolla.
* yhteenlaskuun perustuva tarkistussumma, kuten UDP:n tarkistussumma.
* [tiiviste](https://fi.wikipedia.org/wiki/Tiiviste_(tietotekniikka)) voidaan laskea monella erilaisella tiivistefunktiolla tai tiivistealgoritmilla.

Jos halutaan havaitsemisen lisäksi myös tarvittaessa korjata vikaantunut viesti, niin viestin mukana täytyy kuljettaa enemmän tarkistusinformaatiota, jotta korjaaminen onnistuu. [Virheenkorjaukseenkin](https://fi.wikipedia.org/wiki/Virheenkorjauskoodi) on tarjolla paljon erilaisia menetelmiä, kuten Hamming-koodi. Näitä ei useinkaan käytetä tietoliikenteen ylemmillä kerroksilla, koska viestit ovat varsin suuria ja virheenkorjauksen vaatima lisätila olisi tarpeettoman suuri hyötyyn nähden.

Yksittäisten protokollien käyttämiin menetelmiin tutustutaan näiden protokollien yhteydessä.

## Ajastin

Lähettäjä yleensä aina tarvitsee ajastimen, jolla voidaan katkaista liian pitkään kestänyt vastauksen odotus. Jos lähettäjän odotusta ei katkaista ajastimen laukeamisella, protokollan toiminta ei voi edetä. Vastaanottajan odotukseen ajastin ei ole tarpeen, koska vastaanottaja ei kuitenkaan voi edetä ilman saapuvaa viestiä.

Ajastimen kanssa keskeistä on arvioida, mikä on sopiva maksimiodotusaika, jonka jälkeen ajastimen olisi hyvä laueta mikäli vastausta ei ole saapunut.  Liian aikaisin laukeava ajastin aiheuttaa tarpeettoman uudelleenlähetyksen ja tarpeettoman myöhään laukeava viivästää järjestelmän toimintaa. Hyvä lähtökohta on käyttää ajastimen arvona kaksikertaista kiertoviivettä.


## Kuittaukset


Aiemmissa osioissa on käsitelty niin sanottuja positiivisia kuittauksia (engl. positive ACK). Niillä vastaanottaja kuittaa saapuneita viestejä.

Protokolla voi käyttää positiivisten kuittausten sijasta tai lisäksi myös negatiivisia kuittauksia (engl. negative acknowledge, NAK). Negatiivisella kuittauksella vastaanottaja kertoo, että se ei ole saanut jotain viestiä vaikka se sitä odottaa. Mikäli vastaanottaja käyttää negatiivisia kuittauksia, se saattaa tarvita ajastinta, mutta yleensä vastaanottaja käyttää saapuvia viestejä apuna.

Esimerkiksi valikoivien kuittausten kanssa vastaanottaja voisi lähettää negatiivisen kuittauksen välittömästi, kun joku paketti jää pois välistä eli kyseistä pakettia seuraavan paketin saapuessa. Näin vastaanottaja voisi päästä lähettämään viestin uudelleen paljon aiemmin kuin vasta ajastimen laukeamisen jälkeen.

Järjestelmässä täytyy aina myös varautua ns. tuplakuittauksiin, eli saman kuittauksen lähettämiseen ja vastaanottamiseen useampia kertoja. Tältä ei voi välttyä, koska kun vastaanottaja saa jo saapuneen paketin uudelleen, sen täytyy lähettää kyseiseen pakettiin liittyvä kuittaus uudelleen. Mikäli tätä lähettämistä ei tehtäisi, niin lähettäjä ei ehkä saisi koskaan kuittausta kyseisestä viestistä.

Toisaalta näitä tuplakuittauksia alkuperäinen lähettäjä voi myös pyrkiä hyödyntämään. Esimerkiksi Paluu-N:ään menetelmässä vastaanottaja kuittaa aina samalla kuittauksella kaikki epäjärjestyksessä saapuvat viestit. Lähettäjä voi hyödyntää tätä tietoa ja tehdä uudelleenlähetyksen jo näiden tuplakuittausten perusteella ennen kuin ajastin laukeaa.

Eri protokollat tai niiden eri versiot käyttävät näitä kaikkia vaihtoehtoja.

##  Järjestysnumero

Protokolla käyttävät viesteissä usein järjestysnumeroa tai tunnistenumeroa, jolla viestit voidaan erottaa toisistaan. Taas protokollan omien tarpeiden mukaan tämän numerointi voi olla vain tunnistamiseen tai myös järjestämiseen.

Kun numeroa käytetään järjestämiseen, on tärkeää, että numerointi on yhtäjaksoinen, jotta mahdolliset puuttuvat viestit voidaan havaita. Yhtäjaksoisuus ei kuitenkaan aina tarkoita, että viestinumerot olisivat peräkkäisiä kokonaislukuja. Esimerkiksi TCP numeroi viestinsä siten, että kahden peräkkäisen viestin numeroiden erotus on edellisen viestin datan koko tavuina.

Lähdetään seuraavaksi tutkimaan TCP:n toimintaa tarkemmin.
