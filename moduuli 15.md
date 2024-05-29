# Moduuli 15 - IP Static routing

Next- Hop väyläb konfigurointi tapahtuu reitittimellä alla olevien komentojen mukaisesti. Samalla kertaa voi muodostaa useammalle osoitteelle väylät.
Huom! Muista että ensin syötettävä IP-osoite on laitteen käyttämä ja toinen on sen ulostulos polku. 


    enable
    configuration
    ip route IP-osoite 255.255.255.0 Ulostulo IP-osoite - 172.168.8.8 255.255.255.0 178.168.88.88
    ip route 192.168.8.9 255.255.255.0 178.168.88.88
    ip route 192.168.10.1 255.255.255.0 178.168.88.88
    
