# Moduuli 11: Reitittimien turvakeinoja ja komentoja

poista turhat portit käytöstä, jolloin luvattomat laitteet eivät voi yhdistää itseään niiden kautta suoraan.

Portti turvallisuudessa on hyvä huomioida Cisco-laitteiden oletuksellinen trunking aktiivisuus, joka estää turva-asetuksien suoran aktivoimisen.

    interface range f/0 - x antaa mahdollisuuden muokata monia portteja samalla kertaa
    interface f0/1 muokkaisi vain kyseistä 1-porttia

    interface f0/1
    switchport port-security
    Jos tämä epäonnistuu, tee alla olevan mukaisesti!
    switchport mode access
    switchport port-security
    end

Porttien turva-asetuksia on mahdollista tarkistaa komennoilla

    show port-security interface ja haluttu portti
    show port-security interface f0/1

## MAC-taulukkon turvallisuus asetuksien huomiot ja kontrollointi

MAC-taulukkoon talletettavien tietojen määrää on mahdollista muokata ja täten lisätä turvallisuutta mahdollisien MAC-tulvahyökkäyksien osalta.

    switchport port-security maximum lukumäärä, joka on oletuksellisesti 1 ja maksimissaan 8192

Laitteen voi asettaa oppimaan MAC-taulukon tietoja kolmella eri tavalla: Manuaalisesti, dynaamisesti ja tahmealla dynaamisyydellä.

    interface portti
    switchport port-security mac-address mac-osoite

    switchport port-security -> lisää sillä hetkellä kiinnitetyt laitteet MAC-listaan, uudelleen käynnistys saa taulun resetoitumaan.

     switchport port-security mac-address sticky -> tallettaa mac-osoitteet NVRAMille

Kokonaisuudessaan turva-asetuksien muodostaminen voisi olla siis muotoa

     interface portti
     switchport mode access
     switchport port-security
     switchport port-security maximum MAC-taulukon maksimi määrä
     switchport port-security mac-address halutun mac-osoitteen tiedot manuaalisesti
     switchport port-security mac-address sticky
     end

Jos talletettuja tietoja haluttaisiin tarkastella, onnistuisi tämä komennolla

    show port-security interface portti
    show port-security address

Talletettujen MAC-osoitteiden tiedot voidaan asettaa ajallisesti poistuviksi (inactivity) tai ikuisiksi (absolute).

     interface portti
     switchport port-security aging time 10
     switchport port-security aging type inactivity
     end

Mahdollisien huomio-tilanteiden sattuessa, reititin toteuttaa asetettujen asetuksien mukaiset virhe-toimet.
Nämä ovat shutdown (oletuksellinen), restrict ja protect.

    switchport port-security violation shutdown | restrict | protect

Shutdown - virhe aiheuttaa portti-yhteyden välittömän katkaisemisen, syslog-viestin muodostamisen sekä erhemäärän kasvattamisen. Sammutettu portti voidaan avata adminin toimesta syöttämällä shutdown ja no shutdown komentoja.

Restrict - Portti tiputtaa paketit tuntemattomasta lähteestä, kunnes porttimäärän maksimiksi osoitettu määrä alitetaan ja uusia osoitteita voidaan ottaa MAC-tauluun. Tapahtumasta muodostuu virheluku ja syslog-viesti.

Protect (Vähiten turvallinen) - Portti tiputtaa tuntemasttomasta MAC-lähteestä tulevat paketit, kunnes maksimi MAC-taulu osoitteisto alitetaan tai sen määräää kasvatetaan. Syslog-viestiä ei muodostu.

Muuttaminen tapahtuu komennoilla

    interface portti
    switchport port-security violation tyyppi
    switchport port-security violation restrict
    end

Muutoksen tilan voi tarkistaa tämän jälkeen komennolla

    show port-security interface portti



## VLAN-turvallisuus asetuksien huomiot ja kontrollointi


