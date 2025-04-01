x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.)
Karvinen 2023: Run Salt Command Locally
- Saltilla voi hallita lukuisia orja koneita netin yli
- Tärkeimmät komennot ovat: 
- pkg: asentaa paketin
- file: luo tiedoston
- service: käynnistää demonin uudestaan
- user: tekee käyttäjän
- cmd: ajaa komennon (ei suositella, viimeinen keino, koska pitää manuaalisesti luoda idempotenssi)
Oma huomio: saltissa komennot annetaan, siten että haluttu lopputilanne saavutetaan. Esim. komennon user lopputila, on että käyttäjä on tehty.

Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux
- Saltissa tärkeimmät käsiteet ovat master ja slave: masterilla hallintaan niitä
- Ensiksi pitää asentaa master, sen jälkeen orja ja sen jälkeen pitää orjan pitää tietää missä master on. Lisäksi orjilla pitää olla oma uniikki id.
- Sen jälkeen orja pitää käynnistää uudelleen ja antaa orjalle avain, jonka master hyväksyy
Oma huomio

Karvinen 2006: Raportin kirjoittaminen
- Hyvän raportin pitää olla:
    - Muille toistettavissa ja ympäristö hyvin kirjattu
    - Täsmällinen: kellonajat, toimenpiteet, onnistumiset, epäonnistumiset, muut mahdolliset viat esim. viat konessa tai työkaluissa
    - Lisäksi raportti olisi hyvä olla imperfektissä
    - Helppolukuinen: väliotsikot, oikea kielioppi- ja asu
    - Lähteiden viittaus
    - Vilpin välttö: plagionti, luvaton kuvien käyttö ja sepittäminen eli selostu asioista joita ei oikeasti ole tehnyt.
Oma huomio

WMWare Inc: Salt Install Guide: Linux (DEB) (poimi vain olennainen osa)
- Repojenn asennus
- Saltin palveluiden asennus: salt-master, salt-minion salt-ssh, jne.
- Asennettujen käyttöön otto (enable)
Oma huomio


a) Asenna Debian 12-Bookworm virtuaalikoneeseen. (Poikkeuksellisesti tätä alakohtaa ei tarvitse raportoida, jos siinä ei ole mitään ongelmia. Mutta jos on ongelmia, sitten täsmällinen raportti, jotta voidaan ratkoa niitä yhdessä.)

Minulla ei varsinaisesti ollut ongelmia, mutta en saanut debian-live-12.10.0-amd64-xfce.iso toimimaan. Se ei antanut minun suorittaan asennusta loppuun (tein asetukset, se heitti pois kesken latauksen ja pyyti kirjautumaan. En pystynyt kirjautumaan, koska en ehtinyt viimeistellä asennusta.) Turhauttavan testailun jälkeen ongelma korjaantui asentamalla debian-live-12.10.0-amd64-cinnamon.iso sen sijaan ja sen jälkeen loput sujuivat ongelmitta.


b) Asenna Salt (salt-minion) Linuxille (uuteen virtuaalikoneeseesi).
![Image](https://github.com/user-attachments/assets/b5c3b493-6d09-4059-9444-2347777b1945) 

c) Viisi tärkeintä. Näytä Linuxissa esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.

![Image](https://github.com/user-attachments/assets/48831425-f49b-4b95-9e13-70ac6886f1e9) 
pkg asennus, haluttu lopputulos on asennettu paketti

![Image](https://github.com/user-attachments/assets/078bae58-3205-4ef2-a258-35a231976e55)
file, lopputulos tehty tiedosto

![Image](https://github.com/user-attachments/assets/91925f32-447a-42df-b628-644e71ca2de6)
luodun tiedoston tarkastus cat-komennolla

![Image](https://github.com/user-attachments/assets/1537af09-4d2e-4776-b24d-bce0c4fed865)
service, haluttu lopputulos päällä oleva palvelu


![Image](https://github.com/user-attachments/assets/71a8fd71-b562-45ff-9531-c558cacfa9a7)
service, palvelun sammutus (dead)

![Image](https://github.com/user-attachments/assets/40067908-3e98-48a7-9a75-055e93054d9e)
user, lopputulos uuden käyttäjän luominen

![Image](https://github.com/user-attachments/assets/0fcfe67a-cf0a-4988-b0e1-a6dbc8fd81ef)
cmd.run, lopputulos ajaa komennon





d) Idempotentti. Anna esimerkki idempotenssista. Aja 'salt-call --local' komentoja, analysoi tulokset, selitä miten idempotenssi ilmenee.

Idempotentti lyhyesti tarkoittaa, että jonkun toiminnan voi suorittaa lukuisia kertoja ja lopputulos pysyy samana. Aikaisemmassa kohdassa jo päästiinkiin aiheeseen. Otetaan esimerkiksi c kohdan ensimmäinen asennus pkg. Olen jo asentanut kyseisen paketin. 

![Image](https://github.com/user-attachments/assets/0c7b11f7-48d2-4242-8dcd-ce7e28a0ba43)

Näin käy jos yritän asentaa sen uudestaan. Se pyrkii pääsemään aina samaan lopputulokseen eli tässä tapauksessa paketin asentamiseen. Paketti on jo asennettu, mutta järjestelmä havaitsee, että se on jo asennettu. Komennon voi ajaa niin monta kertaa kuin haluaa, mutta lopputulos on aina sama. Tämä on yksi esimerkki miten idempotenssi esiintyy.

Lähteet:

https://terokarvinen.com/2021/salt-run-command-locally/ 

https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/ 

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html

https://www.youtube.com/watch?v=EF6fqnnl3Uk 

https://loadfocus.com/fi-fi/glossary/what-is-idempotency 
