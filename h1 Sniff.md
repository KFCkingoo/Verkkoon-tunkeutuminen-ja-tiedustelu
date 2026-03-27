# h1 Sniff
Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---

## x) Lue ja tiivistä
### Karvinen 2025: Wireshark - Getting Started
Wireshark on johtava verkon snifferi ja analysointityökalu.

### Karvinen 2025: Network Interface Names on Linux
Verkkoliitännät (Network interface) eivät ole aina fyysisiä.
  
| prefix | interface type |
|----------|----------|
| en    | Ethernet   |
| wl    | WLAN   |
| lo    | Loopback adapteri |

---
## b) Osoita, että pystyt katkaisemaan ja palauttamaan virtuaalikoneen Internet-yhteyden.
<img width="350" height="100" alt="kuva" src="https://github.com/user-attachments/assets/6ce582a6-3d69-460a-b6c4-c213b0efd844" />
<img width="350" height="100" alt="kuva" src="https://github.com/user-attachments/assets/7cf7e1fe-3a04-41ba-8b0f-66a90f39ac9a" />

---
## c) Asenna Wireshark. Sieppaa liikennettä Wiresharkilla. 
```
sudo apt-get install wireshark 
sudo adduser abc wireshark
```

<img width="906" height="287" alt="kuva" src="https://github.com/user-attachments/assets/05a6515d-dc1f-4ab5-a451-cfdc3ed3190d" />

---
## d) Osoita TCP/IP-mallin neljä kerrosta yhdestä siepatusta paketista.
<img width="517" height="170" alt="kuva" src="https://github.com/user-attachments/assets/9bef5d3e-ce66-4b6f-acfa-4f6ca35175ad" />

| Layer | Type |
|-|-|
| Application | TLS |
| Transport | TCP |
| Internet | IPv4 |
| Link | Ethernet 2 |

---
## e) Mitäs tuli surffattua? `https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap`
Tiedot saatiin lukemalla Wireshark taulokkoa ja Statistics -> Capture File Properties.

| tyyppi | määrä |
| - | - |
| Koneita | 2 |
| Paketti | 283 |
| Koko | 126 kB |
| Aika | 7.5 |
| Protokollat | IPv4, UDP (QUIC, DNS), TCP (TLS), ARP |

Wireshark taulukossa luki google.com (selaimen aloitussivu?), terokarvinen.com, gc.zgo.at ja goatcounter.netlify.com. 

Kun filteröitiin paketteja käyttämällä `tls` tuli vain esille gc.zgo.at ja terokarvinen.com.

Filtterillä `frame contains ".com"` saatiin esille vain terokarvinen.com ja terokarvinen.goatcounter.com.

Niillä päätellen surffattiin Teron sivustolle terokarvinen.goatcounter.com.

---
## g) Minkä merkkinen verkkokortti käyttäjällä on?
Käytettiin MAC Address Blocks työkalua ja etsittiin `52:54:00` OUI-koodilla ja löytöjä oli 0. Voitiin olettaa, että kyseessä oli VM verkkokortti.

Tarkistettiin Copilotilla ja se antoi tulokseksi QEMU/KVM virtuaalikoneiden käyttämät verkkokortit.

---
## h) Millä weppipalvelimella käyttäjä on surffaillut?
Aikaisemman tehtävän e) perusteella ja käyttäen `dns.qry.name` filtteriä, saatiin samat sivut. Mutta vain tietyissä sivustoilla oli ip-osoitteet näkyvillä. Onkohan sitten surffailtu sivut google.com, terokarvinen.com ja terokarvinen.goatcounter.com?

## i) Analyysi. Sieppaa pieni määrä omaa liikennettäsi. Analysoi se
Liikenteen sieppaamiseen otettiin Firefox-selaimen aloitussivun uudelleenlataus.

Liikenteessä tuli esille DNS-kyselyitä ja IP-osoitteita kuten ads-img.mozilla.org ja tosi monelta eri sivuilta.

<img width="1131" height="176" alt="kuva" src="https://github.com/user-attachments/assets/8770695c-60df-491f-ac3e-c519f56c09f6" />

<img width="445" height="72" alt="kuva" src="https://github.com/user-attachments/assets/5de8d4a9-9ff7-4ab7-9da1-2e23b9570ad2" />


>_Kuvakaappaukset mielenkiintoisista DNS-kyselyistä._

Huomattiinkin myöhemmin kun suljettiin Wireshark ikkuna, että niitä käytettiin selaimen aloitussivun pikalinkkeihin.

<img width="1032" height="427" alt="kuva" src="https://github.com/user-attachments/assets/b686f39f-664f-4869-9f0b-b60de5fa3a8f" />


---
## Lähteet
Karvinen 2025: Network Interface Names on Linux. https://terokarvinen.com/network-interface-linux/

Karvinen 2025: Wireshark - Getting Started. https://terokarvinen.com/wireshark-getting-started/

