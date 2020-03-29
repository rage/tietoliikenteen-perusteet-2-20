---
path: '/osa-5/5-toiseen-verkkoon'
title: 'Lähettäminen toiseen verkkoon'
hidden: false
---


<text-box variant='learningObjectives' name='Oppimistavoitteet'>

- Osaat kertoa mitä IP-paketin eteenpäin lähetyksessä tapahtuu verkkokerroksen ja linkkikerroksen yhteistoimintana
- Osaat kuvata miten IP-paketin käsittely reitittimen linkkikerroksella tapahtuu. Verkkokerroksen käsittely oli jo aiemmin.

</text-box>


## Paketin lähettäminen toiseen verkkoon

Olemme jo useammassa kohdassa kurssia tutustuneet reitittimen toimintaan eri näkökulmista. Nyt kun myös linkkikerroksen toiminnallisuus on käsitelty, niin voimme katsoa tätä myös linkkikerroksen näkökulmasta. Samalla voimme yhdistää kaikki kurssilla käydyt teemat ja tarkastella yksityiskohtaisesti, mitä tapahtuu kun reitittimelle saapuu paketti ja se lähettää sen eteenpäin toiseen aliverkkoon.


<img alt="Kuvassa on  kaksi aliverkkoa, jotka on yhdistetty kolmannella reitittimen välisellä aliverkolla. Ensimmäisessä on A ja B, jotka on yhdistetty reitittimeen R1 ja toisessa reititin R2 on yhdistetty C:hen ja D:hen. Reitittimet on yhdistetty toisiinsa. A:n IP-osoite on 192.68.12.4 ja MAC-osoite 11-22-33-44-55-66. B:n IP-osoite on 192.168.12.6 ja MAC-osoite 77-88-99-AA-BB-CC. Reitittimillä on kaksi rajapintaa, joten niillä on myös useampia IP- ja MAC-osoitteita. aliverkon suuntaan R1:n IP-osoite on 192.168.12.1 ja MAC 12-12-12-12-12-12. Toisen reitittimen suuntaan R1:n IP-osoite on 111.111.111.1 ja MAC 12-12-12-12-12-13. Reitittimet yhdistävässä aliverkossa R2:n IP-osoite on 111.111.111.2 ja MAC 23-23-23-23-23-01. Aliverkon suuntaan R2:n IP-osoite on 10.0.0.1 ja MAC 23-23-23-23-23-03. C:n IP-osoite on 10.0.0.2 ja MAC A1-A1-A1-A1-A1-A1. D:n IP-osoite on 10.0.0.3 ja MAC 12-13-14-15-16-17.">

KUVA: Kuvassa on kaksi aliverkkoa ja niitä yhdistävät reitittimet. Kummallakin aliverkolla on oma reititin. Tällöin reitittimien välinen yhteys muodostaa vielä kolmannen aliverkon. Huomaa, että kaikki IP- ja MAC-osoitteet ovat muodoltaan oikeita, mutta tätä esimerkkiä varten keksittyjä. Lisäksi IP-osoitteet ovat yksityisverkon osoitteita, joita ei voi yleisessä internetissä käyttää.


Oletetaan, nyt että A haluaa lähettää yhden paketin D:lle. Tämä voisi olla vaikkapa UDP-paketti tai TCP-yhteyden aloittava ACK, mutta koska viestin sisältö on vain A:n ja D:n kannalta merkityksellinen, niin jätetään se tästä tarkastelusta pois. A siis laittaa kuljetuskerroksen segmentin IP-pakettiin, jossa lähettäjänä on A:n oma IP eli 192.162.12.4 ja vastaanottajana D:n IP eli 10.0.0.3. A näkee omasta reititystaulustaan, että tämä paketti pitää ohjata R1:lle, joka osaa käsitellä 10.x verkkoon menevät viestit. IP-paketti laitetaan siis kehykseen, jossa vastaanottajana on R1:den MAC-osoite ja lähettäjänä A:n MAC-osoite. Jos A ei syystä tai toisesta tiedä vielä R1:n MAC-osoitetta, niin se kysyy tiedon ARP-protokollalla.


<img alt="Kehys, jonka sisällä IP-paketti. Kehyksessä vastaanottajana R1:n MAC osoite, lähettäjänä A:n MAC osoite. Kehyksen sisällä olevassa IP-paketissa lähettäjänä A IP-osoite ja vastaanottajana D:n IP-osoite.">

KUVA: Kuvassa on A:n lähettämän kehyksen sisältöä. Tämän esimerkin kannalta meitä kiinnostavat erityisesti osoitteet sekä kehyksessä että IP-paketissa. Muut otsakkeen kentät ja data on jätetty tässä tyhjiksi.

Kun tämä A:n D:lle osoittama viesti saapuu reitittin R1:lle, niin R1 on linkkikerroksen osoitteen mukaan kehyksen vastaanottaja, joten kehys otetaan vastaan ja annetaan kehyksen sisällä oleva IP-paketti reitittimen verkkokerroksen tarkasteltavaksi. Tästä IP-paketista reititin voi nähdä lopullisen vastaanottajan IP-osoitteen. Reititin katsoo omasta reititystaulusta, mihin suuntaan kyseiselle IP-osoitteelle lähetettävä paketti pitää ohjata. Tässä tapauksessa siis paketti pitää lähettää edelleen R2:lle, jonka takaa löytyy 10.x osoitteet.

R1 lisää nyt tämän IP-paketin, jossa lähettäjänä on A ja vastaanottajana D, ympärille uuden kehyksen. Kehyksessä vastaanottajan osoite on R2:n MAC-osoite ja lähettäjänä on R1:n MAC-osoite. Näin koska linkkikerroksella tämän kehyksen lähettäjä on R1 ja kehys on menossa R2:lle, joten ne ovat kehyksessä lähettäjänä ja vastaanottajana.

<img alt="Kuvassa on kehys, jonka sisällä on IP-paketti. Kehyksessä vastaanottajana on R2:n MAC-osoite ja lähettäjänä R1:n MAC-osoite. Kehyksen sisällä olevassa IP-paketissa lähettäjän on edelleen A:n IP-osoite ja vastaanottajana D:n IP-osoite.">

KUVA: R1:n lähettämä kehys, joka on menossa R2:lle. Huomaa, että kehyksessä lähettäjänä on R1 ja vastaanottajana R2, mutta kehyksen sisällä olevassa IP-paketissa lähettäjänä on edelleen A ja vastaanottajana D.


Kun paketti aikanaan saavuttaa D:n, niin silloin kaikki kerrokset käsitellään vuorollaan alhaalta ylöspäin. Eli linkkikerros poistaa ensin oman kehyksensä, koska viesti oli tulossa tälle laitteelle. Verkkokerros tarkistaa vastaanottajan IP:n ja päättää että viesti on nyt perillä eikä sitä tarvitse enää lähettää eteenpäin. Se purkaa sitten IP-paketin ja antaa sisältä löytyvän segmentin kuljetuskerrokselle. Kuljetuskerros puolestaan välittää varsinaisen sovelluskerroksen viestin oikealle sovellukselle pistokkeen ja siihen liitetyn porttinumeron avulla.

<quiz id="abe1ee87-88e0-4090-8975-e336516dd15b"> </quiz>

