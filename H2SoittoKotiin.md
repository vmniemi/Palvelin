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

-


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

Tein vagrantille oman kansion

![Image](https://github.com/user-attachments/assets/c8c7da05-a752-420e-946a-ca8623356a19)

Sen jälkeen yritin ajaa komennon, mutta mitään ei tapahtunut. 

Tarkastin tilannetta komennolla:

          vagrant global-status
  Ja mitään ei ollut ylhäällä. Loin ympäristön 

    vagrant init debian/bullseye64 --box-version 11.20241217.1

  - Sain ympäristön netistä
  - Sen jälkeen menin laittamaan opettajan config-tiedoston Vagrantfile tiedostoon.
  - 
    

![vagrantup](https://github.com/user-attachments/assets/189b316d-795e-4f34-9cba-027ccc11c5e8)


![Image](https://github.com/user-attachments/assets/b212885f-a4c9-41e1-b100-dcb2613836c5)

Sen jälkeen ajoin komennon vagrant up uudestaan ja se alkoi toimimaan.


![Image](https://github.com/user-attachments/assets/e6b39407-dee8-4af2-af7f-5f76ce7fcef8)

Sitten asensin tarvittavat repot saltille ja ajoin 

        sudo apt-get update
Jotta paketit päivittyvät

Asensin molemmille virtuaalikoneelle saltin eli 

        mkdir -p /etc/apt/keyrings

        curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp

        curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
        
  -Ja vielä lopuksi 
        





c) Kaksin kaunihimpi. Tee kahden Linux-tietokoneen verkko Vagrantilla. Osoita, että koneet voivat pingata toisiaan. 

d) Herra-orja verkossa. Demonstroi Salt herra-orja arkkitehtuurin toimintaa kahden Linux-koneen verkossa, jonka teit Vagrantilla. Asenna toiselle koneelle salt-master, toiselle salt-minion. Laita orjan /etc/salt/minion -tiedostoon masterin osoite. Hyväksy avain ja osoita, että herra voi komentaa orjakonetta.


Tein t001 masterin 

![t001master](https://github.com/user-attachments/assets/a5a36844-c5f5-42c3-954f-b1c3ade1982b)


Ja t002 orjan 

![t002orja](https://github.com/user-attachments/assets/d3c4e222-cfa2-41cc-ac18-7c652cc02234)


orjalle
        
        sudo systemctl restart salt-minion.service


Lisäsin orjalle masterin ip kohteeseen (sudoedit komennolla)

        /etc/salt/minion 


![slaveip](https://github.com/user-attachments/assets/c9066285-596a-4bf7-b36c-64c909576ff3)


Menin takaisin masterille ja hyväksyin orjan avaimen 

![Saltkey](https://github.com/user-attachments/assets/e7cbd6be-4f77-4f04-9633-019996404728)


Minulle tuli pieni ongelma salt-masterin kanssa


![masterongelma](https://github.com/user-attachments/assets/b67d5562-e671-41c3-ac6a-08b5ee86f901)


Mutta se lokien katselun ja ihmettelyn jälkeen ratkesi simppelillä tavalla: poistamalla 


![saltmasterpurge](https://github.com/user-attachments/assets/5be5aa2d-5789-497b-bcc5-f1d7aeec8820)


Ja tietenkin uudelleen asentamalla:


![saltmre](https://github.com/user-attachments/assets/14cabb7b-d24d-4cad-b02e-e3771ed4e4f7)




![toimiii](https://github.com/user-attachments/assets/b0ebe8d1-1434-4b33-8297-4f3c4b082f4b)




e) Kokeile vähintään kahta tilaa verkon yli (viisikosta: pkg, file, service, user, cmd)

Lähteet

https://terokarvinen.com/ 

https://terokarvinen.com/palvelinten-hallinta/ 

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ 

https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?

fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux 

https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file 

https://forums.virtualbox.org/viewtopic.php?t=96290 

https://portal.cloud.hashicorp.com/vagrant/discover/debian/bullseye64/versions/11.20241217.1 
