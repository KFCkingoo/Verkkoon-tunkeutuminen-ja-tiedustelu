# h2 Lempiväri: violetti
Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---
## x) Lue ja vastaa lyhyesti kysymyksiin

### Selitä tuskan pyramidin idea 1-2 virkkeellä
- Pyramidi kertoo indikaattorien merkityksestä ja sen suhteesta hyökkääjän tuskaan kun niitä estetään.

### Selitä timanttimallin (Diamond Model) idea 1-2 virkkeellä.
- Timanttimallilla analysoidaan hyökkäykset. Se koostuu 4 avaintekijöistä ja niiden suhteesta toisistaan: **_adversary, infrastructure, capability, victim_**.

---
## a) Apache log. Asenna Apache-weppipalvelin paikalliselle virtuaalikoneellesi. Surffaa palvelimellesi salaamattomalla HTTP-yhteydellä, http://localhost . Etsi omaa sivulataustasi vastaava lokirivi.
Apachen asennus.
```
sudo apt-get install apache2
sudo systemctl start apache2
```
Tulostettiin apachen loki.
```
sudo tail -F /var/log/apache2/access.log
127.0.0.1 - - [11/Apr/2026:02:48:58 +0300] "GET / HTTP/1.1" 200 3383 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0"
```

| parametri | tiedot |
|-|-|
| 127.0.0.1 | ip osoite |
| 11/Apr/2026:02:48:58 +0300 | aika |
| GET / HTTP/1.1 | HTTP/1.1 protokolla GET-pyynnöllä |
| 200 3383 | statuskoodi ja pyynnön koko |
| Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0 | User-Agent, Firefox 140, Linux-järjestelmä |

---
## b) Nmapped. Porttiskannaa oma weppipalvelimesi käyttäen localhost-osoitetta ja 'nmap -A' päällä. Selitä tulokset. (Pelkkä http-portti 80/tcp riittää)
nmap asennus ja http-portti 80/tcp porttiskannaus.
```
sudo apt -y install nmap
sudo nmap -A localhost

PORT    STATE SERVICE VERSION
80/tcp  open  http    Apache httpd 2.4.66 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.66 (Debian)
Device type: general purpose
Running: Linux 2.6.X|5.X
OS details: Linux 2.6.32, Linux 5.0 - 6.2
```

| parametri | tiedot |
|-|-|
| 80/tcp open http | TCP-portti 80 on auki ja palvelu on HTTP |
| Apache httpd 2.4.66 ((Debian)) | Debianissa palvelimena Apache |
| http-title: Apache2 Debian Default Page: It works | Debianin Apache oletussivu toimii |
| http-server-header: Apache/2.4.66 (Debian) | Apachen versio |
| Device type: general purpose | Yleinen tietokone |
| OS details: Linux 2.6.32, Linux 5.0 - 6.2 | Käyttöjärjestelmä |

---
## c) Skriptit. Mitkä skriptit olivat automaattisesti päällä, kun käytit "-A" parametria? (Näkyy avoimien porttinumeroiden alta, http-blah, http-blöh...)
-A: Enable OS detection, version detection, script scanning, and traceroute

|skripti | tieto |
|-|-|
| http-methods | Varmistaa http-metodit GET, POST, HEAD, OPTIONS |
| http-title | Testaa sivun otsikon toimivuuden |
| http-server-header | Hakee palvelimen headerin |

---
## d) Jäljet lokissa. Etsi weppipalvelimen lokeista jäljet porttiskannauksesta (NSE eli Nmap Scripting Engine -skripteistä skannauksessa). Löydätkö sanan "nmap" isolla tai pienellä? Selitä osumat. Millaisilla hauilla tai säännöillä voisit tunnistaa porttiskannauksen jostain muusta lokista, jos se on niin laaja, että et pysty lukemaan itse kaikkia rivejä?
Tässä esimerkkinä 1 lokeista:
`127.0.0.1 - - [11/Apr/2026:03:53:21 +0300] "OPTIONS / HTTP/1.1" 200 181 "-" "Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)"`

Lokista löytyi sekä Nmap että nmap lokirivin lopusta. Lokissa tehtiin OPTIONS-pyyntö, kysyi mitä http-metodeja on tuettu.
Lokin lopusta selviää, että pyyntö tuli NSE:n kautta. URL-linkki on dokumentaation siainti.

Jos loki on liian suuri, niin haluttua tietoa voidaan hakea `sudo grep -i` komennolla.

---
## e) Wire sharking. Sieppaa verkkoliikenne porttiskannatessa Wiresharkilla. Huomaa, että localhost käyttää "Loopback adapter" eli "lo". Tallenna pcap. Etsi kohdat, joilla on sana "nmap" ja kommentoi niitä. Jokaisen paketin jokaista kohtaa ei tarvitse analysoida, yleisempi tarkastelu riittää.



---
## Lähteet
