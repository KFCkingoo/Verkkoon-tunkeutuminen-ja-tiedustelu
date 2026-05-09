## h7 Aaltoja harjaamassa

Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---
## x) Lue ja tiivistä
#### Hubacek 2019: [Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs](https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s)
- Nopea ohje URH-työkalun käyttöön
- Spectrum Analyzer -> Record Signal
- Hyödylliset toiminnot:
  - Modulation: ASK
  - Show Signal as

#### Cornelius 2022: [Decode 433.92 MHz weather station data](https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html)
- Blogi näyttää tarkemman esimerkin rtl_433- ja URH-ohjelman käytöstä
- Record Signal 20-100 kHz (0,02-0,1 MHz) offsetilla kuin mitä Spectrum Analyzer antaa
- Build a modulation decoding scheme: Edit -> Decoding -> Invert -> Morse Code.

---
## a) Lähteet ja läppä. Tarkista, että jokaisessa kotitehtäväraportissasi on viitattu lähteisiin (kurssiin, tehtäviin, kirjoihin, ohjeisiin...).
Tehty

---
## b) rtl_433. Asenna rtl_433 automaattista analyysia varten. Kokeile, että voit ajaa sitä. './rtl_433' vastaa "rtl_433 version 25.02 branch..."
Asennettu ja testattu tunnilla.

```
sudo apt install rtl-433
rtl_433
```

---
## c) Automaattinen analyysi. Mitä tässä näytteessä tapahtuu? Mitä tunnisteita (id yms) löydät? Converted_433.92M_2000k.cs8. Analysoi näyte 'rtl_433' ohjelmalla.
Analysoitiin näyte:
```
rtl_433 -r Converted_433.92M_2000k.cs8
```
<img width="976" height="226" alt="kuva" src="https://github.com/user-attachments/assets/70261683-158a-4491-804e-5c692a7567c8" />
<br>

| Kategoria | Tieto |
|-------------------------------|-------------------------------|
| Model | KlikAanKlikUit-Switch, Proove-Security, Nexa-Security |
| id / House Code | 8785315 |
| Channel  |  3   |
| State | Off |
| Unit  |  0 tai 3  |
| Command | Off |
| Group | 0 |


Näytteessä sama id / House code. Näytteessä tuotteet lähettää radiotaajuuksia kanavassa 3 ja ovat mahdollisesti pois käytöstä, koska State-tila on Off ja Command on Off?
Switch-näytteessä on Unit 0 kun taas turvallisuus-näytteissä Unit on 3, se voi indikoida eri laitteita tai eri radiotaajuuksia.

---
## d) Too compex 16? Olet nauhoittanut näytteen 'urh' -ohjelmalla .complex16s-muodossa. Muunna näyte rtl_433-yhteensopivaan muotoon ja analysoi se. Näyte Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s
Teron vinkkien mukaan muutoksessa pitää olla samat taajuudet tiedoston nimessä ja muunnos tapahtuu suoraan nimenmuutoksella.

```
mv Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.cs8
```

Ajettiin näyte rtl_433-ohjelmalla. Näyte on sama kuin edellisessä tehtävässä c) mutta eri nimellä.

---
## e) Ultimate. Asenna URH, the Ultimate Radio Hacker.
Asennettiin tunnilla ja testattiin toimivuus. Asennus sama kuin Teron ohjeissa:
```
sudo apt-get update
sudo apt-get -y install pipx
pipx install urh
pipx ensurepath
## sulje ja avaa terminaali
```

---
## Tarkastele näytettä 1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s. Siinä Nexan pistorasian kaukosäätimen valon 1 ON -nappia on painettu kolmesti. Käytä Ultimate Radio Hacker 'urh' -ohjelmaa.
Käynnistettiin ohjelma `urh`-komennolla. Avattiin näyte.

<img width="1911" height="472" alt="kuva" src="https://github.com/user-attachments/assets/d855b57d-c7a5-4282-b4e3-da02c1c9e681" />

---
## f) Yleiskuva. Kuvaile näytettä yleisesti: kuinka pitkä, millä taajuudella, milloin nauhoitettu? Miltä näyte silmämääräisesti näyttää?
<img width="768" height="235" alt="kuva" src="https://github.com/user-attachments/assets/7bf887d1-d9de-42b7-be76-598ccac31978" />

Infotiivistelmässä kesto oli 5.49s, mutta valittuaan kaikki bitit se näytti keston olevan 5.42s.



Tiedoston luontiaikaa ei pidä sekoittaa milloin näytettä nauhoitettiin. Voidaan olettaa luontiajan olevan ajankohta mikä on tiedoston nimessä jos sitä ei ole muutettu `1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s` eli **pvm 12.4.2025 ja klo 11.38.05**.

Replay signal toiminnossa näkyi 433.92 Mhz. Tiedoston nimessä `1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s` näkyi, että **taajuus on 433.912MHz**.
<img width="250" height="58" alt="kuva" src="https://github.com/user-attachments/assets/9ce6398e-3fbc-441f-b424-948c1855b39f" />


Jos oletetaan, että tiedoston nimessä olevat tiedot ovat oikeita niin osa URH-ohjelman antamat tiedot voivat olla harhaanjohtavia tai ei saatavilla (ei löydetty esim. nauhoitusajankohtaa).

---
## g) Bittistä. Demoduloi signaali niin, että saat raakabittejä. Mikä on oikea modulaatio? Miten pitkä yksi raakabitti on ajassa? Kuvaile tätä aikaa vertaamalla sitä johonkin. (Monissa singaaleissa on line encoding, eli lopullisia bittejä varten näitä "raakabittejä" on vielä käsiteltävä)
Valittiin Signal view: Demodulated ja bittilistalta valittiin bitit kunnes se täytti yhden signaalin.

<img width="706" height="474" alt="kuva" src="https://github.com/user-attachments/assets/4ad2c853-6c34-4fd7-a585-307116ae563f" />

Valittuja näytteitä oli 521 ja kesto 521 mikrosekuntia(µs). Vertaamiseen käytettiin Microsoft Copilottia: 0.521ms on 0,5% eli 1/200 silmänräpäyksestä.


---
## Lähteet
Cornelius 2022: [Decode 433.92 MHz weather station data](https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html)

Hubacek 2019: [Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs](https://www.youtube.com/watch?t=199&v=sbqMqb6FVMY&feature=youtu.be)

Karvinen 2026: [Verkkoon tunkeutuminen ja tiedustelu](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#laksyt)

Microsoft Copilot. Hyödynnetty tehtävässä g) 0.521ms ajan vertailuun.




