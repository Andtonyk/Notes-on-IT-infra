# Moduuli 9: FHRP-yhteyden muodostamisen perushuomioita

Lyhyesti FHRP tarkoittaa reitittimille muodostettavaa tukiverkkoa, joka varmistaa verkon eheyden HSRP-protokollallisten reitittimien verkossa.

Tämä tapahtuu määrittelemällä aktiiviset-reitittimet ja valmius-reitittimet, jotka voivat aktiivisen reititin yhteyden katketessa ottaa yhteyden välityksen kontolleen.

Tämä tapahtuu määrittämällä ensin aktiivi reititin ja tämän jälkeen muokkaamalla verkon muista reitittimistä valmius-reitittimiä (Huom! mahdollista ulospäin suuntaavaa reititintä lukuunottamatta, ellei niitäkin ole useita).

Tämä onnistuu määrittämällä reitittimen asetukset halutun mukaisiksi.

    enable
    configure
    interface gigabitEthernet 0/x tai interface g0/x
    standby version 2 (standby version 1 tukee pelkkää ipv4-yhteyttä)

Reitittimille voi asettaa useita HSRP instansseja, näiden tulee kuulua keskenään samoin nimettyihin ryhmiin, jotta ne voidaan tunnistaa virtuaalisessa ympäristössä.
Esimerkissä käytetään ryhmätunnuksena numeroa: 1

    standby 1 ip tähän tulisi virtuaalisen gatewayn ip-osoite

Kun osoite olisi syötetty aktiiviseksi muodostettavalle reitittimella, haluttaisiin sen oletuksellinen prioriteetti määrittää, jotta se olisi ja pysyisi aktiivisena reitittimenä, jopa mahdollisien katkoksista normalisoitumisen jälkeenkin.

    standby 1 priority tähän tulisi prioriteetin numero esim. standby 1 priority 150
    standby 1 preempt -> varmistaa että muokattava reititin pitää itsensä aktiivisena reitittimenä.

Mahdollisilla valmius-reitittimillä riittäisi ylläolevista vaiheista seuraavat: valmiustyyppi, ryhmä ja sen ip-osoite.

    standby version 2
    standby 1 ip 
    
Reitittimillä tehdyt muutokset olisi mahdollista tarkistaa komennoilla

    show standby
    show standby brief
