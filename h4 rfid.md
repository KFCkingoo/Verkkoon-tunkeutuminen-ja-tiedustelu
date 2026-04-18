# h4 RFID ja NFC
Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---

## 1. Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta?
Käytössä olevat RFID tuotteita ovat sirukortit kuten maksukortti, henkilökortti, HSL-matkakortti, avainlätkä ja älypuhelin.

| RFID tuote        | Suojaus              |
|--------------|------------------------------|
| Sirukortit   | RFID-suojatussa lompakossa   |
| Älypuhelin   | NFC on pois päältä           |
| Avainlätkä   | Ei suojausta                 |

Jos RFID-suojattu lompakko on toiminnallinen, niin uskon että olen yleisesti jonkin verran suojattu RFID urkinnalta paitsi avainlätkästä.

---
## 2. Tutustu APDU komentojen rakenteeseen (voit käyttää tekoälyä tutustumiseen)
APDU on RFID-korttien ja kortinlukijoiden viestintä (ISO 7816 -standardi). Se koostuu kahdesta tyypistä **komento (Command APDU) ja vastaus (Response APDU)**.

APDU-komentokehys koostuu 4 pakollisesta tavusta **CLA, INS, P1, P2** ja valinnaisesti **dataa (Lc + Data)** sekä **odotetun vastauskoon (Le)**.

#### APDU-komento
| Tavu   | Selitys                                      |
|----------|-----------------------------------------------|
| CLA      | class = luokka                               |
| INS      | instruction = komento (SELECT, READ)         |
| P1       | parametri 1                                   |
| P2       | parametri 2                                   |
| (Lc)       | length command = datan pituus                 |
| (Data)     | lähetettävä data                              |
| (Le)       | length expected = odotetun vastausdatan pituus |

#### APDU-vastaus
| Tavu   | Selitys                                      |
|----------|-----------------------------------------------|
| (Data)     | vastausdata                    |
| SW1 SW2  | Status Words = status-koodi (esim. 90 00 = OK)      |


#### Esimerkki APDU-komennosta SELECT (AID)
`00 A4 04 00 07 A0 00 00 00 03 10 10`

| Tavu                     | Selitys                     |
|--------------------------|-----------------------------|
| 00                       | CLA                         |
| A4                       | INS (SELECT)                |
| 04                       | P1 (valitse AID)            |
| 00                       | P2                          |
| 07                       | Lc (7 tavua dataa)          |
| A0 00 00 00 03 10 10     | AID                         |

>_Esimerkki otettu suoraan Microsoft Copilotilta_

---
## 3. Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit. (Vinkki, useimmat liittyvät henkilökortteihin)


---
## Lähteet

Microsoft Copilot osassa 2 APDU-komennon rakenteen tutustumiseen.


