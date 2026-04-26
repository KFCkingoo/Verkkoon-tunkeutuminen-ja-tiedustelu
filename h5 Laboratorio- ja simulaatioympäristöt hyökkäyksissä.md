# 5. Laboratorio- ja simulaatioympäristöt hyökkäyksissä
Harjoitukset on tehty keväällä Teron ja Larin 2026 Verkkoon tunkeutuminen ja tiedustelu - ICI013AS3A-3003 kurssia varten.

## Ympäristö
VMware Workstation Pro

Ubuntu 20.04.6 LTS (Larin Student-lab mininet-vm)

---
## a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii.
Muodostettiin ssh-yhteys hostilta -> mininet-vm `ssh -Y mininet@192.168.x.x`.

Käynnistettiin verkko-ohjain ja mininet

```
ryu-manager ryu.app.simple_switch_13
sudo mn --topo single,3 --mac --switch ovsk --controller remote
```

>_Syötteet näyttävät tältä_

<img width="669" height="130" alt="kuva" src="https://github.com/user-attachments/assets/3e9c83c7-cf94-4507-9cee-87ba3038c657" />

<img width="688" height="292" alt="kuva" src="https://github.com/user-attachments/assets/f3da0269-fe3e-4107-95e0-aeba657204c7" />

VM on lisännyt verkko-ohjaimen ja luonut verkkoympäristön (h1-h3 hostit ja s1-kytkin).


Kun ajaa `pingall` niin verkko-ohjain tulostaa datapaketit näytölle

    packet in 1 00:00:00:00:00:01 ff:ff:ff:ff:ff:ff 1
    packet in 1 00:00:00:00:00:02 00:00:00:00:00:01 2
    packet in 1 00:00:00:00:00:01 00:00:00:00:00:02 1
    packet in 1 00:00:00:00:00:01 ff:ff:ff:ff:ff:ff 1
    packet in 1 00:00:00:00:00:03 00:00:00:00:00:01 3
    packet in 1 00:00:00:00:00:01 00:00:00:00:00:03 1
    packet in 1 00:00:00:00:00:02 ff:ff:ff:ff:ff:ff 2
    packet in 1 00:00:00:00:00:03 00:00:00:00:00:02 3
    packet in 1 00:00:00:00:00:02 00:00:00:00:00:03 2


>_Nähdään MAC-osoitteet ja niiden pingaukset, jossa voidaan olettaa numerojen vertailevan kuhunkin hosteihin ja ff-MAC olevan s1-kytkin_

Syötettiin `xterm h1` ja tuli virhe:

<img width="262" height="37" alt="Näyttökuva 2026-04-26 190655" src="https://github.com/user-attachments/assets/4ae566f8-af9b-49cf-9e81-3b44ab1de7ec" />

Ajettiin Larin antamat xterm-virheiden korjaukset:

```
 ./get_xauth.sh
 sudo -s xauth add mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 22ce67f9c6514c99d2903e2b9d97e496
```

Aikaisempi xterm-virhe oli vielä ennallaan.

Kokeiltiin ajata mininet X11-tuella `sudo -E mn --topo single,3 --mac --switch ovsk --controller remote`. Virhe ennalla.

Asennettiin virtuaalikoneelle X11-työkalut:

```
sudo apt update
sudo apt install xorg xauth xterm
```

Virhe edelleenkin ennalla.

Asennettiin MobaXterm host-koneelle ja ajettiin harjoitus sillä.

Saatiin avattua xterm (sudo `-E` mn --topo single,3 --mac --switch ovsk --controller remote).

Seurattiin tehtävänkulkua Larin annetuilla ohjeilla kunnes päädyttiin virhetilanteeseen:

<img width="538" height="37" alt="kuva" src="https://github.com/user-attachments/assets/f85af97b-5750-462e-92ba-3ed78e36f6e2" />

Virhe johtui siitä, koska `python3 ./arp_poison.py` ei ajettu erillisessä h3 terminaalissa. Jatkettiin ohjeiden mukaan ja saatiin ARP-myrkytys tehtyä.

Tässä vielä h3 logia:

<img width="936" height="279" alt="kuva" src="https://github.com/user-attachments/assets/54008283-ed65-47d5-b403-145a49b5bf87" />

#### IP duplikoitu ARP kyselyssä.

<img width="1917" height="642" alt="kuva" src="https://github.com/user-attachments/assets/386d6acd-fdbb-4c28-aac1-821e9d9d08f9" />

Harjoituksessa luotiin verkkoympäristö, liikennettä kuunneltiin ja ohjattiin data toisen laitteen (h3) kautta. ARP-paketit napattiin Wiresharkilla. Harjoitus muistuttaa MitM-hyökkäyksen simulointia.

---
# Ympäristön laatimisessa ja ajaamisella ilman virheitä vei monta tuntia, harjoituskin aloitettiin täten päivän myöhässä. Raportti palautetaan keskeneräisenä ellen vielä saa tehtäviä tehtyä myöhemmin ennen tuntia.

## b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat.

---
## c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti.

    Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii.




---
## Lähteet
Moodle. Larin materiaalit
