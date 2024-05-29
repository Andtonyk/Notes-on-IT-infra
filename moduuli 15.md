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


## Aiheeseen liittyviä yleishyviä komentoja

    show ip route
    show ip route static

    show ipv6 route static
    show ipv6 route

    ping
    tracewroute
    show running-config | section ip route

