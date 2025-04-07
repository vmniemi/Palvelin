x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää.)
Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant (Huomaa: nykyinen Debian stable on 12-Bookworm, Vagrantissa "debian/bookworm64". Vanhentunutta 11-bullseye:ta ei enää käytetä)

- Vagrantilla voi tehdä nopeasti virtuaalikoneita erittäin nopeasti.
- Sen saa asennettua helposti komennoilla Linuxilla
  
        sudo apt-get update
        sudo apt-get install vagrant virtualbox

  
- Valitettavasti en ole (vielä) Linux käyttäjä joten latasin sen osoitteesta https://developer.hashicorp.com/vagrant/install Windows-version ja AMD64 version.
- Vagrantille pitää tehdä oma kansio ja jonne pitää laittaa konfiguraatiotiedosto. Artikkelissä oli opettajan oma esimerkki versio siitä.
- orjille pääsee ottamalla ssh-yhteyden komennolla

        ssh@koneen_id
  
  Takaisin koneelle pääsee takaisin exit komennolla

  Komenolla
  
          vagrant destroy
  tuhotaan koneet ja kaikki sen tiedostot
  ja komennolla

      vagrant up
  voi luoda koneita

Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux (Huomaa: Nykyisin ennen Saltin asentamista on asennettava ensin varasto [package repository], ohje h1 vinkeissä)


Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves, vain kohdat

Infra as Code - Your wishes as a text file
top.sls - What Slave Runs What States

a) Hello Vagrant! Osoita jollain komennolla, että Vagrant on asennettu (esim tulostaa vagrantin versionumeron). Jos et ole vielä asentanut niitä, raportoi myös Vagrant ja VirtualBox asennukset. (Jos Vagrant ja VirtualBox on jo asennettu, niiden asennusta ei tarvitse tehdä eikä raportoida uudelleen.)

Minulla oli jo valmiiksi asennettuna virtualbox, joten minun täytyi vain asentaa Vagrant. Tein se kuten jo aikaisemmassa selostetussa vaiheessa eli latasin sen https://developer.hashicorp.com/vagrant/install. Ladattuani tiedoston, siinä oli setup wizard ja sain sen asennuttua. Käynnistin koneeni uudestaan ja ajoin komennon

          vagrant --version
Windowsin cmd:ssä

![Image](https://github.com/user-attachments/assets/1a4635db-60b7-4abd-a91e-cc1a44ad9b0e)

Tulos oli tämä joten Vagrant on nyt asennettu isäntäkoneelle.

b) Linux Vagrant. Tee Vagrantilla uusi Linux-virtuaalikone.

c) Kaksin kaunihimpi. Tee kahden Linux-tietokoneen verkko Vagrantilla. Osoita, että koneet voivat pingata toisiaan.

d) Herra-orja verkossa. Demonstroi Salt herra-orja arkkitehtuurin toimintaa kahden Linux-koneen verkossa, jonka teit Vagrantilla. Asenna toiselle koneelle salt-master, toiselle salt-minion. Laita orjan /etc/salt/minion -tiedostoon masterin osoite. Hyväksy avain ja osoita, että herra voi komentaa orjakonetta.

e) Kokeile vähintään kahta tilaa verkon yli (viisikosta: pkg, file, service, user, cmd)

Lähteet
https://terokarvinen.com/ 
https://terokarvinen.com/palvelinten-hallinta/ 
https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ 
https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux 
https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file 
https://forums.virtualbox.org/viewtopic.php?t=96290 
