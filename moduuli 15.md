# Moduuli 15 - IP Static routing

## Next-Hop
Next- Hop väylän konfigurointi tapahtuu reitittimellä alla olevien komentojen mukaisesti. Samalla kertaa voi muodostaa useammalle osoitteelle väylät.
Huom! Muista että ensin syötettävä IP-osoite on laitteen käyttämä ja toinen on sen ulostulos polku. 

### IPv4:n Next-Hop

    enable
    configure
    ip route IP-osoite 255.255.255.0 Ulostulo IP-osoite - ip route 172.168.8.8 255.255.255.0 178.168.88.88
    ip route 192.168.8.9 255.255.255.0 178.168.88.88
    ip route 192.168.10.1 255.255.255.0 178.168.88.88

### IPv6:n Next-Hop

    enable
    configure
    ipv6 unicast-routing
    ipv6 route 2001:db8:acad:1::/64 via 2001:db8:acad:2::2
    ipv6 route 2001:db8:cafe:1::/64 via 2001:db8:acad:2::2
    ipv6 route 2001:db8:cafe:2::/64 via 2001:db8:acad:2::2

## Suoraan yhdistetyt staattiset väylät
Staattinen reititys voidaan myös saavuttaa ilmoittamalla halutun IP-osoitteen sijasta, se portti josta tieto halutaan välittää ulos.

### IPv4:n Directly Connected Static Route

    enable
    configure
    ip route IP-osoite 255.255.255.0 Portti - ip route 172.16.1.0 255.255.255.0 s0/1/0
    ip route 192.168.1.0 255.255.255.0 s0/1/0
    ip route 192.168.2.0 255.255.255.0 s0/1/0

### IPv6:n Directly Connected Static Route
    
    enable
    configure
    R1(config)# ipv6 route IPv6-Oosite via Portti - ipv6 route 2001:db8:acad:1::/64 via s0/1/0
    R1(config)# ipv6 route 2001:db8:cafe:1::/64 via s0/1/0
    R1(config)# ipv6 route 2001:db8:cafe:2::/64 via s0/1/0

## Fully Specified Static Route - Täysin määritetty Staattinen väylä
Pyrkimyksenä on next-hopin sekä staattisen ulostulon määrittämisen kautta varmistaa, että laite ei sekoita polkujaan useamman laitteen ja porttiyhteyksien muodostuessa.

Erityishuomiona on mainittava IPv6-yhteydet, joissa vastaavaa rakennetta tarvitaan jos verkossa on link-local tyyppinen järjestely.
Tämä johtuu siitä että verkossa voi olla osoitteellisa samankaltaisuuksia, joilta voidaan välttyä spesifioimalla porttien tiedot, yhdessä ulostulon IP-osoitteeseen.

### IPv4 Fully Specified Static Route

    enable
    configure
    ip route IP-osoite 255.255.255.0 Porttitiedot Ulostulo IP-osoite - ip route IP-osoite 255.255.255.0 172.168.8.8 255.255.255.0 GigabitEthernet 0/0/1 178.168.88.88
    ip route 192.168.8.9 255.255.255.0 172.168.8.8 255.255.255.0 GigabitEthernet 0/0/1 178.168.88.88
    ip route 192.168.10.1 255.255.255.0 172.168.8.8 255.255.255.0 GigabitEthernet 0/0/1 178.168.88.88
    ip route 192.168.10.1 255.255.255.0 172.168.8.8 255.255.255.0 GigabitEthernet 0/0/1 178.168.88.88

### IPv6 Fully Specified Static Route

    enable
    configure
    ipv6 route IP-osoite Portti Ulostulo IP-osoite - ipv6 route 2001:db8:acad:1::/64 s0/1/0 fe80::2


### Aiheeseen liittyviä yleishyviä komentoja

    show ip route
    show ip route static

    show ipv6 route static
    show ipv6 route

    ping
    traceroute
    show running-config | section ip route

## Default Static Route

Asetus, jossa polku vastaa kaikkia paketteja. Sillä tarkoitetaan myös kaikkia verkkoja, jotka eivät ole osana routing tablea.
Reitittimet tulevat yleensä valmiiksi ohjelmoituna default routeilla tai ne on asetettu oppimaan niiden tietoja toiselta reitittimeltä.
Default Static Route on siis yleensä viimeinen vaihtoehto, jos automaattinen toteutus ei toimisi tai turva-asetukset määräisivät näin.

Huom! Tällöin IP-osoitteeksi ja maskiksi asetetaan kaiken välittävät tiedot: IPv4:ssä 0.0.0.0 ja 0.0.0.0 kun taas IPv6:ssa ::/0

### IPv4 Default Static Route

    enable
    configure
    ip route IP-osoite Maski Kohde - ip route 0.0.0.0 0.0.0.0 172.16.2.2
    
    exit
    show ip route static

### IPv6 Default Static Route

    enable
    configure
    ipv6 route ::/0 2001:db8:acad:2::2
    
    exit
    show ipv6 route static

Muutoksen tarkistus tapahtuu komennolla "show ip route static" ja IPv6:lla "show ipv6 route static"

## Floating Static Route

Tarjoavat varayhteydet pääasiallisille staattisille verkoille. Tulevat aktiivisiksi ainoastaan kun pääyhteys ei ole aktiivisena.
Pääyhteyksiin voi kuulua manuaalisesti muodostettu staattinen verkko tai reitittimien automaattisesti oppima rakenne.

Tämä varmistetaan asettamalla varaverkolle iso AD-arvo, jolloin verkko ohjaa liikenteen alemman AD:n porteista, kunnes yhteys katkeaisi ja aiempi ei käytössä ollut polku otettaisiin sen löytymisen jälkeen käyttöön (AD: Administrative Distance).

Korkean AD:n verkkoa ei käytetä, jos alemman AD:n polku löytyy.

    Directly connected - 0
    Static Route - 1
    EIGRP summary route - 5
    External BGP - 20
    Internal BGP - 90


### IPv4 Floating Static Route

Floatin muodostaminen IP4lle tarkoittaisi uuden staattisen yhteyden muodostamista, 

    enable
    configure
    ip route
    ip route 172.168.8.8 255.255.255.0 178.168.88.88 - Oletuksellinen staattinen yhteys, AD arvona 1
    ip route 172.168.8.8 255.255.255.0 10.10.10.2 5 - Toissijainen staattinen yhteys, AD arvona 5

    exit
    show ip route
    traceroute
    Kokeile katkaista pääasiallista staattista yhteyttä
    show ip route
    traceroute

### IPv6 Floating Static Route

    enable
    configure
    ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2 - Oletuksellinen staattinen yhteys, AD arvona 1
    ipv6 route 2001:db8:acad:1::/64 2001:db8:feed:10::2 5 - Toissijainen staattinen yhteys, AD arvona 5
    
    exit
    show ip route
    traceroute
    Kokeile katkaista pääasiallista staattista yhteyttä
    show ip route
    traceroute

### IPv4 Floating Static Default Route

Jos muodostettavalle vara polulle haluttaisiin mahdollisuus välittää kaikki paketit eteenpäin, tehtäisiin se Default Static Routen ohjeita mukaillen.

    enable
    configure
    ip route 0.0.0.0 0.0.0.0 172.16.2.2 - Oletuksellinen staattinen yhteys, AD arvona 1
    ip route 0.0.0.0 0.0.0.0 10.10.10.2 5 - Toissijainen staattinen yhteys, AD arvona 5
    
    exit
    show ip route

### IPv6 Floating Static Default Route

    enable
    configure
    ipv6 route ::/0 2001:db8:acad:2::2 - Oletuksellinen staattinen yhteys, AD arvona 1
    ipv6 route ::/0 2001:db8:feed:10::2 5 - Toissijainen staattinen yhteys, AD arvona 5
    
    exit
    show ipv6 route

## Static Host Routes

Ciscon reitittimillä yhteys muodostuu automaattisesti IP-osoiteen muodostumisen yhteydessä.

Tässä osiossa käydään läpi sen manuaalisesti toteuttaminen, syitä tälle voivat olla tehostettu reititin pakettien välittäminen sekä tietoturva.

Ennen muutosta laitteilla on jo tietoja, jos verkko on ollut toiminnallinen.

    enable
    configure
    ip route 209.165.200.238 255.255.255.255 198.51.100.2
    ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2
    exit
    show ip route | begin Gateway


Next-Hopin asettaminen

### IPv4 Static Host Route with Link-Local Next-Hop

    enable
    configure
    no ip soute 209.165.200.238 255.255.255.255 198.51.100.2
    ip route 209.165.200.238 255.255.255.255 GigabitEthernet 0/0/1 198.51.100.2

### IPv6 Static Host Route with Link-Local Next-Hop

    enable
    configure
    no ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2 - tämä poistaaa jo olemassaolevan polun käytöstä, jonka jälkeen voidaan lisätä uusi.
    ipv6 route IP-osoite Portti Ulos menevä IP-osoite - ipv6 route 2001:db8:acad:2::238/128 serial 0/1/0 fe80::2
    
