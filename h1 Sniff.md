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
| Network Access | Ethernet 1 |

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

Niillä päätellen surffattiin Teron sivustolle goatcounter.com.

---
## g) Minkä merkkinen verkkokortti käyttäjällä on? `https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap`








---
## Lähteet
Karvinen 2025: Network Interface Names on Linux. https://terokarvinen.com/network-interface-linux/

Karvinen 2025: Wireshark - Getting Started. https://terokarvinen.com/wireshark-getting-started/

