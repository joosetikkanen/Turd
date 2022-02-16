
## Ongelma 1
Tyyppi:   Polkuinjektio

Sijainti: checkPath()

Kuvaus:

Aliohjelmassa checkPath ei tarkasteta polkuinjektiota oikeaoppisesti normalisoimalla annettua polkua.

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Tee pyyntö /delete_file?file=../sue/tiedosto.txt
3) Tulos: Suen tiedosto tuhotaan

### Korjaus:

Korjasin tämän näin, että polku normalisoidaan ja verrataan sen alkua merkkijonoon "/app/WebData/" + username, jotta ohjelma suojautuu paremmin polkuinjektioilta myös jatkokehittäessä (esim. poikkeuksen sijaan 
kokematon kehittäjä koittaa poistaa ../ polusta, jolloin tarkistuksen voi ohittaa syöttämällä ....//)

Korjaus toimii, koska käyttäjä ei voi syöttää toisen käyttäjän hakemistoa esim. tiedostojen hakemiseksi tai poistamiseksi

Korjaus estää uuden polkuinjektion syntymisen, sillä samaa aliohjelmaa käytetään kaikissa polkuinjektiolle alttiissa kohdissa.



## Ongelma 2
Tyyppi:   Polkuinjektio

Sijainti: delete_file()

Kuvaus:

Aliohjelmassa delete_file ei tarkasteta käyttäjän syöttämää polkua
millään tavalla. Tämä mahdollistaa sen, että käyttäjä tuhoaa minkä
tahansa tiedoston, esimerkiksi toisen käyttäjän tiedoston tai vaikkapa
jonkin järjestelmätiedoston.

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Tee pyyntö /delete_file?file=../sue/tiedosto.txt
3) Tulos: Suen tiedosto tuhotaan

### Korjaus:

Korjasin tämän näin, että lisäsin checkPath() kutsun, jolla tarkistetaan käyttäjän antama polku tiedoston poistamiseksi

Korjaus toimii, koska polku tarkistetaan ennen kuin tiedosto poistetaan

Korjaus estää uuden polkuinjektion syntymisen, sillä uutta reittiä luotaessa polku tarkistetaan ennen tiedoston poistamista


## Ongelma 3
Tyyppi:   Shell-injektio

Sijainti: checkerLoop()

Kuvaus:

Aliohjelmassa checkerLoop käynnistetään shellin kautta "file"-ohjelma, joka muodostetaan merkkijonona käyttäjän antaman tiedoston nimen perusteella

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Lataa tiedosto upload_file-endpointin kautta nimeltä "image.png; rm -rf /"
3) Tulos: Kaikki tiedostot palvelimelta poistetaan

### Korjaus:

Korjasin tämän näin, että muutin subprocess.run() käyttämään listaa, johon annetaan komento ja tiedoston nimi

Korjaus toimii, koska "file" ohjelmalle välitetään rakenteellisesti käyttäjän syöttämän tiedoston nimi argumenttina

Korjaus estää uuden shell injektion, koska "file" komentoa ei enää ajeta shellin kautta liimamalla käyttäjän syötettä merkkijonona



## Ongelma 4
Tyyppi:   XSS

Sijainti: serve_file()

Kuvaus:

Aliohjelmassa serve_file ei sanitoida jaettujen tiedostojen linkkilistaa, joka näkyy jokaiselle käyttäjälle

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Tee pyyntö /share_file?file="<script>hack()</script>"
3) Tulos: Kaikki palvelun käyttäjät suorittavan pahantahtoisen skriptin sivulle kirjautuessaan

### Korjaus:

Korjasin tämän näin, että html data sanitoidaan bleach kirjaston avulla, jotta ohjelma ei salli käyttäjien tekemien skriptien lataamista sivulle

Korjaus toimii, koska jaettujen linkkien lista sanitoidaan, jolloin mahdolliset skriptit rikkoutuvat

Korjaus estää uuden XSS yrityksen linkkilistan kautta



## Ongelma 5
Tyyppi:   Arkaluontoista tietoa palvelimen lokissa

Sijainti: login()

Kuvaus:

Aliohjelmassa login haetaan käyttäjän kirjautumisiedot queryparametreista, 
jolloin myös käyttäjän salasana tulostetaan lokiin

### Esimerkkihyökkäys: 

Hyökkääjä pääsee käsiksi palvelimen lokitietoihin, josta näkee suoraan käyttäjien tunnukset 
ja vastaavat salasanat

### Korjaus:


Korjasin tämän näin, että muutin kirjautumisen käyttämään HTTP POST -metodia, jotta palvelimen lokiin ei tulostuisi arkaluontoista tietoa

Korjaus toimii, koska lokiin ei tulostu arkaluontoista tietoa

Korjaus estää käyttäjätietojen urkkimisen lokitiedoista

(varsinainen lokitus ja lokien tallentaminen puuttuu ohjelmasta)



## Ongelma 6
Tyyppi:   Ajastushyökkäysmahdollisuus

Sijainti: login()

Kuvaus:

Aliohjelmassa login verrataan käyttäjän antamaa salasanaa "tietokantaan" tallennettuun 
salasanaan tavallisella merkkijonovertailulla, mikä on altis ajastushyökkäykselle

### Esimerkkihyökkäys: 

Hyökkääjä suorittaa ajastushyökkäysskriptin, jolla saa kartoitettua tietojärjestelmään 
tallennetut tunnukset ja lopulta niitä vastaavat salasanat

### Korjaus:

Korjasin tämän näin, että merkkijonovertailut suoritetaan compare_digest-metodilla, jotta kirjautumiseen liittyvissä vertailuissa kestäisi aina vakiopituinen aika

Korjaus toimii, koska käyttäjätietojen tarkistamiseen kuluu vakiomittainen aika riippumatta siitä, 
löytyykö vastaavaa käyttäjätunnusta tai tunnuksia vastaavia salasanoja tietojärjestelmästä

Korjaus estää käyttäjätietojen urkkimisen ajastushyökkäyksillä


## Ongelma 7
Tyyppi:   XSS

Sijainti: share_file()

Kuvaus:

Aliohjelmassa share_file ei sanitoida käyttäjän syöttämää tiedoston nimeä. Ongelma on korjattu jo 
ongelmassa 4 tämän ohjelman tapauksessa, mutta jatkokehityksen kannalta olisi hyvä sanitoida käyttäjän 
syöttämä tiedosto jo tässä vaiheessa.

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Tee pyyntö /share_file?file="<script>hack()</script>"
3) Tulos: Kaikki palvelun käyttäjät suorittavan pahantahtoisen skriptin sivulle kirjautuessaan ja ladatessaan resurssin, jossa jaetut tiedostot näkyvät

### Korjaus:

Korjasin tämän näin, että lisäsin käyttäjän syötteen sanitointia bleach kirjaston avulla

Korjaus toimii, koska käyttäjä ei voi ladata palvelimelle html skriptejä

Korjaus estää XSS yritykset tiedostojen avulla myös jatkossa, mikäli jaettuja tiedostoja näytetään muissakin ohjelman osissa



## Ongelma 8
Tyyppi:   XSS

Sijainti: serve_file()

Kuvaus:

Aliohjelmassa serve_file ladataan jokaisen käyttäjän selaimeen bad_file_log, joka voi sisältää 
pahantahtoisia skriptejä

### Esimerkkihyökkäys: 

1) Kirjaudu sisään käyttäjänä "sam".
2) Lähetä tiedosto /upload_file endpointin kautta, jonka nimi on muotoa <script>hack()</script>
3) Tulos: tiedoston nimi menee "some files were rejected" -listaan, joka ladataan jokaisen käyttäjän selaimeen

### Korjaus:

Korjasin tämän näin, että sanitoin bad_file_log sisällön, joka ladataan selaimeen

Korjaus toimii, koska bad_file_log sisältämät mahdolliset skriptit eivät lataudu käyttäjien selaimissa

Korjaus estää XSS yritykset hylättyjen tiedostojen kautta
