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

Jos loki on liian suuri, niin haluttua tietoa voidaan hakea komennolla `sudo grep -i`.

---
## e) Wire sharking. Sieppaa verkkoliikenne porttiskannatessa Wiresharkilla. Huomaa, että localhost käyttää "Loopback adapter" eli "lo". Tallenna pcap. Etsi kohdat, joilla on sana "nmap" ja kommentoi niitä. Jokaisen paketin jokaista kohtaa ei tarvitse analysoida, yleisempi tarkastelu riittää.
Avataan wireshark ja porttiskannataan localhost `sudo nmap -A localhost`.

<img width="817" height="501" alt="kuva" src="https://github.com/user-attachments/assets/3a9fd2b3-1ad0-4933-b0e7-f6620ebcceaa" />

Verkkoliikenteessä oli paljon HTTP-metodien pyyntöjä, pääosin GET POST OPTIONS ja PROPFIND. Verkkoliikenteessä oli myös IPP Request, mikä viittaa printterien viestintään. Paketeissa tuli esille User-Agent ja pyynnöt oli tehty NSE:llä.

---
## f) Net grep. Sieppaa verkkoliikenne 'ngrep' komennolla ja näytä kohdat, joissa on sana "nmap".
ngrep asennus ja verkkoliikenteen sieppaus
```
sudo apt install ngrep
sudo ngrep -d lo -i nmap
sudo nmap -A localhost       # toisella terminaalilla

T 127.0.0.1:52778 -> 127.0.0.1:631 [AP] #2130
  GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
  k/nse.html)..Host: localhost:631..Connection: close....                                         
#########
T 127.0.0.1:52784 -> 127.0.0.1:631 [AP] #2139
  OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
  /book/nse.html)..Host: localhost:631..Connection: close....                                     
#######
T 127.0.0.1:42256 -> 127.0.0.1:80 [AP] #2146
  GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
  k/nse.html)..Host: localhost..Authorization: NTLM TlRMTVNTUAABAAAAB4IIoAAAAAAAAAAAAAAAAAAAAAA=..
  Connection: close....                                                                           
##
```

<details>
  <summary>Näytä kaikki nmap osumat</summary>
  
    T 127.0.0.1:52778 -> 127.0.0.1:631 [AP] #2130
      GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
      k/nse.html)..Host: localhost:631..Connection: close....                                         
    #########
    T 127.0.0.1:52784 -> 127.0.0.1:631 [AP] #2139
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost:631..Connection: close....                                     
    #######
    T 127.0.0.1:42256 -> 127.0.0.1:80 [AP] #2146
      GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
      k/nse.html)..Host: localhost..Authorization: NTLM TlRMTVNTUAABAAAAB4IIoAAAAAAAAAAAAAAAAAAAAAA=..
      Connection: close....                                                                           
    ##
    T 127.0.0.1:52790 -> 127.0.0.1:631 [AP] #2148
      GET /nmaplowercheck1775911689 HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engi
      ne; https://nmap.org/book/nse.html)..Host: localhost:631..Connection: close....                 
    ##
    T 127.0.0.1:42262 -> 127.0.0.1:80 [AP] #2150
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost..Connection: close....                                         
    ##
    T 127.0.0.1:52804 -> 127.0.0.1:631 [AP] #2152
      POST /sdk HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost:631..Content-Length: 441..Connection: close....<soap:Envelope x
      mlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance
      " xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"><soap:Header><operationID>00000001-0000
      0001</operationID></soap:Header><soap:Body><RetrieveServiceContent xmlns="urn:internalvim25"><_t
      his xsi:type="ManagedObjectReference" type="ServiceInstance">ServiceInstance</_this></RetrieveSe
      rviceContent></soap:Body></soap:Envelope>                                                       
    ##
    T 127.0.0.1:52812 -> 127.0.0.1:631 [AP] #2154
      GET /.git/HEAD HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nma
      p.org/book/nse.html)..Host: localhost:631..Connection: close....                                
    ##
    T 127.0.0.1:42266 -> 127.0.0.1:80 [AP] #2156
      GET /nmaplowercheck1775911689 HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engi
      ne; https://nmap.org/book/nse.html)..Host: localhost..Connection: close....                     
    ####
    T 127.0.0.1:42278 -> 127.0.0.1:80 [AP] #2160
      GET /.git/HEAD HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nma
      p.org/book/nse.html)..Host: localhost..Connection: close....                                    
    ####
    T 127.0.0.1:52824 -> 127.0.0.1:631 [AP] #2164
      POST / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Connection: close..Content-Length: 88..Host: localhost:631..Content-Type: applicat
      ion/x-www-form-urlencoded....<methodCall> <methodName>system.listMethods</methodName> <params></
      params> </methodCall>                                                                           
    ###
    T 127.0.0.1:42282 -> 127.0.0.1:80 [AP] #2167
      POST /sdk HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost..Content-Length: 441..Connection: close....<soap:Envelope xmlns
      :xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xm
      lns:soap="http://schemas.xmlsoap.org/soap/envelope/"><soap:Header><operationID>00000001-00000001
      </operationID></soap:Header><soap:Body><RetrieveServiceContent xmlns="urn:internalvim25"><_this 
      xsi:type="ManagedObjectReference" type="ServiceInstance">ServiceInstance</_this></RetrieveServic
      eContent></soap:Body></soap:Envelope>                                                           
    ##
    T 127.0.0.1:42294 -> 127.0.0.1:80 [AP] #2169
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost..Depth: 0..Connection: close....                              
    ####
    T 127.0.0.1:52838 -> 127.0.0.1:631 [AP] #2173
      GET /robots.txt HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nm
      ap.org/book/nse.html)..Host: localhost:631..Connection: close....                               
    ####
    T 127.0.0.1:52844 -> 127.0.0.1:631 [AP] #2177
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: HEAD..Connection: close..Ho
      st: localhost:631....                                                                           
    ##
    T 127.0.0.1:42296 -> 127.0.0.1:80 [AP] #2179
      POST / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Connection: close..Content-Length: 88..Host: localhost..Content-Type: application/
      x-www-form-urlencoded....<methodCall> <methodName>system.listMethods</methodName> <params></para
      ms> </methodCall>                                                                               
    ###########
    T 127.0.0.1:52858 -> 127.0.0.1:631 [AP] #2190
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost:631..Depth: 0..Connection: close....                          
    ##
    T 127.0.0.1:42322 -> 127.0.0.1:80 [AP] #2192
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: HEAD..Connection: close..Ho
      st: localhost....                                                                               
    ##
    T 127.0.0.1:42338 -> 127.0.0.1:80 [AP] #2194
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost..Connection: close....                                         
        ######################################################################################################################################################
    T 127.0.0.1:52866 -> 127.0.0.1:631 [AP] #2344
      PSAQ / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Host: localhost:631..Connection: close....                                        
    ##
    T 127.0.0.1:52870 -> 127.0.0.1:631 [AP] #2346
      GET /evox/about HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nm
      ap.org/book/nse.html)..Host: localhost:631..Connection: close....                               
    ###
    T 127.0.0.1:42340 -> 127.0.0.1:80 [AP] #2349
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Connection: close..Content-Length: 0..Host: localhost..Depth: 1....           
    ##########
    T 127.0.0.1:42344 -> 127.0.0.1:80 [AP] #2359
      GET /HNAP1 HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost..Connection: close....                                        
    ##
    T 127.0.0.1:52886 -> 127.0.0.1:631 [AP] #2361
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: GET..Connection: close..Hos
      t: localhost:631....                                                                            
    ##
    T 127.0.0.1:42360 -> 127.0.0.1:80 [AP] #2363
      YCNV / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Host: localhost..Connection: close....                                            
    ########
    T 127.0.0.1:42366 -> 127.0.0.1:80 [AP] #2371
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: GET..Connection: close..Hos
      t: localhost....                                                                                
    ##
    T 127.0.0.1:52892 -> 127.0.0.1:631 [AP] #2373
      GET /HNAP1 HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost:631..Connection: close....                                    
    ##
    T 127.0.0.1:42378 -> 127.0.0.1:80 [AP] #2375
      GET /evox/about HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nm
      ap.org/book/nse.html)..Host: localhost..Connection: close....                                   
    ##
    T 127.0.0.1:52904 -> 127.0.0.1:631 [AP] #2377
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost:631..Depth: 0..Connection: close....                          
    ##
    T 127.0.0.1:42392 -> 127.0.0.1:80 [AP] #2379
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Host: localhost..Depth: 0..Connection: close....                              
    ####
    T 127.0.0.1:42404 -> 127.0.0.1:80 [AP] #2383
      GET /robots.txt HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nm
      ap.org/book/nse.html)..Host: localhost..Connection: close....                                   
    ####
    T 127.0.0.1:52918 -> 127.0.0.1:631 [AP] #2387
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost:631..Connection: close....                                     
    ####################################################################################################
    T 127.0.0.1:52940 -> 127.0.0.1:631 [AP] #2487
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: POST..Connection: close..Ho
      st: localhost:631....                                                                           
    ##
    T 127.0.0.1:42420 -> 127.0.0.1:80 [AP] #2489
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: POST..Connection: close..Ho
      st: localhost....                                                                               
    #####################
    T 127.0.0.1:52960 -> 127.0.0.1:631 [AP] #2510
      GET /favicon.ico HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://n
      map.org/book/nse.html)..Host: localhost:631..Connection: close....                              
    ##
    T 127.0.0.1:42430 -> 127.0.0.1:80 [AP] #2512
      GET /favicon.ico HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://n
      map.org/book/nse.html)..Host: localhost..Connection: close....                                  
    #############
    T 127.0.0.1:52966 -> 127.0.0.1:631 [AP] #2525
      PROPFIND / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.or
      g/book/nse.html)..Connection: close..Content-Length: 0..Host: localhost:631..Depth: 1....       
    ########################
    T 127.0.0.1:42432 -> 127.0.0.1:80 [AP] #2549
      GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
      k/nse.html)..Host: localhost..Connection: close....                                             
    #######################
    T 127.0.0.1:52980 -> 127.0.0.1:631 [AP] #2572
      GET / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/boo
      k/nse.html)..Host: localhost:631..Connection: close....                                         
    #########
    T 127.0.0.1:52984 -> 127.0.0.1:631 [AP] #2581
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: PUT..Connection: close..Hos
      t: localhost:631....                                                                            
    ##
    T 127.0.0.1:42448 -> 127.0.0.1:80 [AP] #2583
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: PUT..Connection: close..Hos
      t: localhost....                                                                                
    ###############################################
    T 127.0.0.1:53014 -> 127.0.0.1:631 [AP] #2630
      HEAD / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Host: localhost:631..Connection: close....                                        
    #######
    T 127.0.0.1:53024 -> 127.0.0.1:631 [AP] #2637
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: DELETE..Connection: close..
      Host: localhost:631....                                                                         
    ##
    T 127.0.0.1:42450 -> 127.0.0.1:80 [AP] #2639
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: DELETE..Connection: close..
      Host: localhost....                                                                             
    #######################
    T 127.0.0.1:53038 -> 127.0.0.1:631 [AP] #2662
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: TRACE..Connection: close..H
      ost: localhost:631....                                                                          
    ##
    T 127.0.0.1:42456 -> 127.0.0.1:80 [AP] #2664
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: TRACE..Connection: close..H
      ost: localhost....                                                                              
    #####
    T 127.0.0.1:53046 -> 127.0.0.1:631 [AP] #2669
      POST / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/bo
      ok/nse.html)..Host: localhost:631..Content-Length: 0..Connection: close....                     
    #########################
    T 127.0.0.1:53060 -> 127.0.0.1:631 [AP] #2694
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: OPTIONS..Connection: close.
      .Host: localhost:631....                                                                        
    ##
    T 127.0.0.1:42460 -> 127.0.0.1:80 [AP] #2696
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: OPTIONS..Connection: close.
      .Host: localhost....                                                                            
    ########
    T 127.0.0.1:53072 -> 127.0.0.1:631 [AP] #2704
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Host: localhost:631..Connection: close....                                     
    #################
    T 127.0.0.1:53078 -> 127.0.0.1:631 [AP] #2721
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: CONNECT..Connection: close.
      .Host: localhost:631....                                                                        
    #####
    T 127.0.0.1:42470 -> 127.0.0.1:80 [AP] #2726
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: CONNECT..Connection: close.
      .Host: localhost....                                                                            
    ###############
    T 127.0.0.1:53082 -> 127.0.0.1:631 [AP] #2741
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: PATCH..Connection: close..H
      ost: localhost:631....                                                                          
    #####
    T 127.0.0.1:42486 -> 127.0.0.1:80 [AP] #2746
      OPTIONS / HTTP/1.1..User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org
      /book/nse.html)..Origin: example.com..Access-Control-Request-Method: PATCH..Connection: close..H
      ost: localhost....  
 </details>


---
## g) Agentti. Vaihda nmap:n user-agent niin, että se näyttää tavalliselta weppiselaimelta
Testattiin Teron antamaa vinkkiä `sudo nmap -A localhost --script-args http.useragent="BSD experimental on XBox350 alpha (emulated on Nokia 3110)"`.

```
T 127.0.0.1:54110 -> 127.0.0.1:631 [AP] #4935
  GET /nmaplowercheck1775912598 HTTP/1.1..Connection: close..Host: localhost:631..User-Agent: BSD 
  experimental on XBox350 alpha (emulated on Nokia 3110)....                                      
##########
T 127.0.0.1:56900 -> 127.0.0.1:80 [AP] #4945
  GET /nmaplowercheck1775912598 HTTP/1.1..Connection: close..Host: localhost..User-Agent: BSD expe
  rimental on XBox350 alpha (emulated on Nokia 3110)....
```

User-Agent muutos onnistunut.

---
## h) Pienemmät jäljet. Porttiskannaa weppipalvelimesi uudelleen localhost-osoitteella. Tarkastele sekä Apachen lokia että siepattua verkkoliikennettä. Mikä on muuttunut, kun vaihdoit user-agent:n? Löytyykö lokista edelleen tekstijono "nmap"?
Avattiin Wireshark ja ajettiin `sudo nmap -A localhost --script-args http.useragent="BSD experimental on XBox350 alpha (emulated on Nokia 3110)"`.

<img width="700" height="250" alt="kuva" src="https://github.com/user-attachments/assets/cde28bdf-8d61-4f5a-aff0-9585f83fded5" />

<img width="700" height="250" alt="kuva" src="https://github.com/user-attachments/assets/c702422b-6634-4f96-980f-290d5fa37c7f" />

Wiresharkissa vähemmän pyyntöjä ja nmap oli vielä esillä 2 Wiresharkin datapaketissa. Apache-lokissa ei ollut nmap näkyvillä.

---
## i) Hieman vaikeampi: LoWeR ChEcK. Poista skritiskannauksesta viimeinenkin "nmap" -teksti. Etsi löytämääsi tekstiä /usr/share/nmap -hakemistosta ja korvaa se toisella. Tee porttiskannaus ja tarkista, että "nmap" ei näy isolla eikä pienellä kirjoitettuna Apachen lokissa eikä siepatussa verkkoliikenteessä. (Tässä tehtävässä voit muokata suoraan lua-skriptejä /usr/share/nmap alta, 'sudoedit'. Muokatun version paketoiminen siis rajataan ulos tehtävästä.)
Etsittiin hakemistosta nmaplowercheck komennolla `grep -ir "nmaplowercheck"`. Hakemistosta löytyi lua-skripti nselib/http.lua:  local URL_404_1 = '/nmaplowercheck' .. os.time(os.date('*t')). Etsittiin lua-skriptistä nmap `cat nselib/http.lua | grep -i "nmap"`.

<img width="1162" height="380" alt="kuva" src="https://github.com/user-attachments/assets/7e8ca7c9-7744-4d36-bcc1-00e5b56041d6" />

Microlla `ctrl+F` etsittiin nmaplowercheck. Muokattiin se mikrolla `abclowercheck`. Ajettiin nmap `sudo nmap localhost -A localhost  --script-args http.useragent="BSD experimental on XBox350 alpha (emulated on Nokia 3110)"`.

<img width="222" height="97" alt="kuva" src="https://github.com/user-attachments/assets/c6d48459-2f13-4727-a578-ec44b68b4b41" />

Wiresharkissa ei enään näkynyt nmap.

Apache-lokia piti tyhjentää ennen tarkistusta `sudo truncate -s 0 /var/log/apache2/access.log`.

Ajettiin nmap uudelleen ja tarkistettiin apache-loki `grep -i "nmap" /var/log/apache2/access.log`.

Kokeiltiin etsiä muokattu "abc" ja grep löysi lokista 2 osumaa.

```
127.0.0.1 - - [11/Apr/2026:21:23:44 +0300] "GET /abclowercheck1775931824 HTTP/1.1" 404 491 "-" "BSD experimental on XBox350 alpha (emulated on Nokia 3110)"
127.0.0.1 - - [11/Apr/2026:21:23:52 +0300] "GET /abclowercheck1775931832 HTTP/1.1" 404 491 "-" "BSD experimental on XBox350 alpha (emulated on Nokia 3110)"
```

Poistettiin skriptiskannauksesta "nmap"-teksti onnistuneesti.

---
## j) FCC ID. Etsi valitsemasi langattoman laitteen tiedot FCC ID:llä. Mitä liikenteen purkamista tai manipuloimista hyödyttävää tietoa löydät?
Tosi monessa langattomassa laitteissa ei ollut FCC ID helposti saatavilla. Otettiin laitteeksi Gamesir Cyclone 2.

<img width="1041" height="351" alt="Näyttökuva 2026-04-11 222550" src="https://github.com/user-attachments/assets/3d17871c-9ba8-4398-b009-2f50712f2611" />

Sivustossa on annettu tietoa antennista, sisäiset ja ulkoiset kuvat, testiraportti bluetoothista.

---
## Lähteet
Bianco 2013. The Pyramid of Pain. https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

Caltagirone et al 2013. Diamond Model. https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf

FCC. https://www.fcc.gov/oet/ea/fccid

Karvinen 2026. https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#laksyt

Microsoft Copilot hyödynnetty joissakin tulkinnoissa.  https://copilot.microsoft.com/
