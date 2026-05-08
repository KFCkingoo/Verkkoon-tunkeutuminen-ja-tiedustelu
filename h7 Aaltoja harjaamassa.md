## h7 Aaltoja harjaamassa

Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---
## x) Lue ja tiivistä
#### Hubacek 2019: [Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs](https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s)
- Nopea hhje URH-työkalun käyttöön, sillä napataan 433 MHz radiotaajuuksia.
- Spectrum Analyzer -> Record Signal
- Modulation: ASK / Show Signal as: Hex

#### Cornelius 2022: [Decode 433.92 MHz weather station data](https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html)
- Blogi näyttää tarkemman esimerkin rtl433 ja urhin käytöstä
- Record Signal 20-100 kHz (0,02-0,1 MHz) offsetilla kuin mitä Spectrum Analyzer antaa
- Build a modulation decoding scheme: Edit -> Decoding -> Invert -> Morse Code.

---
## a) Lähteet ja läppä. Tarkista, että jokaisessa kotitehtäväraportissasi on viitattu lähteisiin (kurssiin, tehtäviin, kirjoihin, ohjeisiin...).
Tehty

## b) rtl_433. Asenna rtl_433 automaattista analyysia varten. Kokeile, että voit ajaa sitä. './rtl_433' vastaa "rtl_433 version 25.02 branch..."
Asennettu ja testattu tunnilla.

```
sudo apt install rtl-433
rtl_433
```

## c) Automaattinen analyysi. Mitä tässä näytteessä tapahtuu? Mitä tunnisteita (id yms) löydät? Converted_433.92M_2000k.cs8. Analysoi näyte 'rtl_433' ohjelmalla.
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


Näytteessä sama id / House code.






## d) Too compex 16? Olet nauhoittanut näytteen 'urh' -ohjelmalla .complex16s-muodossa. Muunna näyte rtl_433-yhteensopivaan muotoon ja analysoi se. Näyte Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s

## e) Ultimate. Asenna URH, the Ultimate Radio Hacker.

## Tarkastele näytettä 1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s. Siinä Nexan pistorasian kaukosäätimen valon 1 ON -nappia on painettu kolmesti. Käytä Ultimate Radio Hacker 'urh' -ohjelmaa.

## f) Yleiskuva. Kuvaile näytettä yleisesti: kuinka pitkä, millä taajuudella, milloin nauhoitettu? Miltä näyte silmämääräisesti näyttää?

## g) Bittistä. Demoduloi signaali niin, että saat raakabittejä. Mikä on oikea modulaatio? Miten pitkä yksi raakabitti on ajassa? Kuvaile tätä aikaa vertaamalla sitä johonkin. (Monissa singaaleissa on line encoding, eli lopullisia bittejä varten näitä "raakabittejä" on vielä käsiteltävä)

