# h7 WiFi
Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VirtualBox - Debian 13 Trixie VM

AMD Ryzen 7

---
## a) Tutustu wifi challenge lab 2.1 harjoitus ympäristöön ja käytä tarvittaessa hyväksesi jo olemassa olevia ohjeita.
Labissa on harjoituksia ja valmiit työkalut verkon tiedusteluun. Tunnilla tutkittiin miten saadaan ympäristössä tiedot kuten WLANin kanavat ja laitteiden MACit.

Tunnilla käytiin läpi myös miten WEP ja WPA-PSK hyökkäykset toimivat Labissa. Käynnistettiin WLAN ja monitoroitiin se, etsittiin sieltä mitä WLAN kanavia kuunneltiin ja generoitiin sinne liikennettä hyökkäystä varten.

**WPA on turvallisempi kuin WEP.**

---
## b) Kirjoita raportti siitä mitä opit ja mitkä asia yllättivät sinut kun tutustuit harjoitukseen.
Harjoituksessa opin miten tiedustellaan WiFi verkossa, kuten WLANin monitorointi ja sieltä tietojen poimiminen hyökkäystä varten.

Yllätyin kuinka helposti joku lähettyvillä voi kuunnella ja tunkeutua verkkoon. 
Yllätyin myös tosi paljon, että verkkoja voi myös kuunnella ilman internettiä sillä paketit ja WPA:n 4-way-handshake kulkevat radiotaajuuksilla.

---
## c) Miten suhtautumisesi WLanin turvallisuuteen muuttui sen jälkeen kun teit harjoitukset?
Suhtautuminen WLANin turvallisuuteen on laskenut paljon tämän tehtävän myötä. Hyvä, että WEP-protokollaa ei enään pyritä käyttämään ja WPA2 on nykyinen standardi.
Tosin WPA2 heikkoutena voi olla heikko salasana, tämän vahvistaa WPA3. Tarkistin myös, että itselläni ei ole WEP käytössä. :D

Jos on pakko yhdistää julkiseen avoimeen WLAN‑verkkoon, käyttäisin VPN:ää lisäsuojana ja suosittelisin myös HTTPS‑yhteyksien käyttöä. Muuten suosin mobiilidatan käyttöä.

---
## Lähteet
Moodle. Larin materiaali.

WiFiChallenge Lab. https://lab.wifichallenge.com/

