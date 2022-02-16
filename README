# ITKA2050 Harjoitustyön runko

Tämä on pieni tiedostonjako-ohjelma, jossa on _aivan kaikki_ täysin pielessä. Harjoituksen
tarkoituksena on etsiä ja paikata siitä virheitä. Katso ohjeet
sivulta <http://itka2050.it.jyu.fi/Harjoitustyo.html>.

Virheet voivat olla mitä tahansa virheitä, joiden avulla palvelua voi käyttää
'väärin' tai vaarallisesti.

# Ohjelman oletettu toiminta (l. speksi)

Ohjelma on webbipalvelu, johon käyttäjä voi kirjautua. Kirjautunut käyttäjä voi
ladata sivulle tiedostoja, jakaa ja poistaa niitä. Muut käyttäjät eivät pääse
käsiksi kuin käyttäjän jakamiin tiedostoihin.

# Kääntäminen & Käynnistäminen

Ohjelma on Python 3 ohjelma joka on suunniteltu toimimaan linux-pohjaisessa
ympäristössä. Eli jos käytät linuxia asenna ensin Python osoitteesta
https://www.python.org/downloads/.

Pythonin mukana tulee Pythonin paketinhallintaohjelma, eli pip (jos ei tullut,
saat sen osoitteesta https://pip.pypa.io/en/stable/installing/). Pip:n avulla 
voit asentaa muut tarvittavat ohjelmat komennolla

   $ pip3 install -r requirements.txt

Tämän jälkeen voit suorittaa ohjelman komennolla 

   $ FLASK_APP=Turd.py flask run

Lopuksi osoita selain osoitteseen <http://localhost:5000/login>.

## Docker

Toinen vaihtoehto on käyttää docker 'virtuaalikonetta'. Tällä tavalla voit testata
ohjelmaa myös windows-ympäristössä. Tällä tavalla ei myöskään ei
tarvitse asentaa muita ohjelmia kuin docker ja samalla oppii käyttämään tätäkin
vallan yleisesti käytettyä työkalua.

Dockerin avulla ohjelmaa käytetään näin:

1) Luo docker kuva kirjoittamalla

   $ docker build . -t turd

2) Aja ohjelma kirjoittamalla

   $ docker run -it -p5000:5000 turd

3) Osoita selain osoitteseen http://localhost:5000/login


# Vinkkejä

1) Jos et tiedä mistä etsiä virhettä voit pääsääntöisesti vain osoittaa sormella
satunnaista kohtaa koodissa ja kohtuullisella todennäköisyydellä sormen alle
jääneessä koodissa on jotain pielessä.

2) Jos haluat testata ohjelmaa selaimessa useammalla yhtäaikaisella eri käyttäjällä
selaimen 'incognito' moodi voi auttaa.


