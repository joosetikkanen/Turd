# Ohjelman oletettu toiminta (l. speksi)

Ohjelma on webbipalvelu, johon käyttäjä voi kirjautua. Kirjautunut käyttäjä voi
ladata sivulle tiedostoja, jakaa ja poistaa niitä. Muut käyttäjät eivät pääse
käsiksi kuin käyttäjän jakamiin tiedostoihin.

# Kääntäminen & Käynnistäminen

Ohjelma on Python 3 ohjelma joka on suunniteltu toimimaan linux-pohjaisessa
ympäristössä.

   $ pip3 install -r requirements.txt

Tämän jälkeen voit suorittaa ohjelman komennolla 

   $ FLASK_APP=Turd.py flask run

Lopuksi osoita selain osoitteseen <http://localhost:5000/login>.

## Docker

Toinen vaihtoehto on käyttää dockeria. Tällä tavalla voit testata
ohjelmaa myös windows-ympäristössä.

Dockerin avulla ohjelmaa käytetään näin:

1) Luo docker kuva kirjoittamalla

   $ docker build . -t turd

2) Aja ohjelma kirjoittamalla

   $ docker run -it -p5000:5000 turd

3) Osoita selain osoitteseen http://localhost:5000/login


