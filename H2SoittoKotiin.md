x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää.)
Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant (Huomaa: nykyinen Debian stable on 12-Bookworm, Vagrantissa "debian/bookworm64". Vanhentunutta 11-bullseye:ta ei enää käytetä)

- Vagrantilla voi tehdä nopeasti virtuaalikoneita erittäin nopeasti.
- Sen saa asennettua helposti komennoilla Linuxilla
  
        sudo apt-get update
        sudo apt-get install virtualbox
        sudo apt-get install vagrant 

  
- Valitettavasti en ole (vielä) Linux käyttäjä joten latasin sen osoitteesta https://developer.hashicorp.com/vagrant/install Windows-version ja AMD64 version.
- Vagrantille pitää tehdä oma kansio ja jonne pitää laittaa konfiguraatiotiedosto. Artikkelissä oli opettajan oma esimerkki versio siitä.

  
  ![Image](https://github.com/user-attachments/assets/b74c0cd9-1bbe-4995-8b58-cb10e9316a43)

  Tässä on minun (alkeellinen) käsitys mitä tiedosto tekee
  


  $tscript = <<TSCRIPT
  
Määrittelee kielen
  
set -o verbose
apt-get update
apt-get -y install tree

Päivittää ja asentaa tree-paketin

echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"

Tulostaa Done - set up test environment - https://terokarvinen.com/search/?q=vagrant kun on valmis
TSCRIPT

Vagrant.configure("2") do |config|
	config.vm.synced_folder ".", "/vagrant", disabled: true
	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	config.vm.provision "shell", inline: $tscript
	config.vm.box = "debian/bullseye64"
Tekee jaetun kansion "shared"
Hankkii shellin, jolla ajetaan tscriptejä
Määrittää käyttöjärjestelmän Debian bullseye64
 

	config.vm.define "t001" do |t001|
		t001.vm.hostname = "t001"
		t001.vm.network "private_network", ip: "192.168.88.101"
	end

 Luo ensimmäisen virtuaalikoneen t001, nimen voi vaihtaa vaihtamalla t001.vm.hostname = "haluttu nimi"
 Lisäksi verkko määritetään yksityiseksi ja sille annetaan ip-osoite

	config.vm.define "t002", primary: true do |t002|
		t002.vm.hostname = "t002"
		t002.vm.network "private_network", ip: "192.168.88.102"
	end
 
Samat vaiheet toiselle koneelle
 
end

Scripti päätetään

}
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
Ensiksi koneille pitää asentaa tarvittavat repot

# Luodaan kansio avaimille
	mkdir -p /etc/apt/keyrings
# Tehdään luottamussuhde koneiden välille
	curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
# Tehdään repo
	curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources

-Salt-masterin asennus yhdelle koneelle, jolla ohjataan orjia
 
		master$ sudo apt-get update
		master$ sudo apt-get -y install salt-master
  
- Ip-osoitteen hakeminen orjaa varten komennolla

		master$ hostname -I

-Salt-minionin asennus toiselle koneelle, jotta master voi ohjata sitä

 		orja$ sudo apt-get update
		orja$ sudo apt-get -y install salt-minion
  
  		slave$ sudoedit /etc/salt/minion
		master: Masterin ip-osoite, joka saatiin hostname -I komennolla
  
-Orjan uudeellenkäynnistys, jotta asetukset päivittyvät

 		slave$ sudo systemctl restart salt-minion.service

-Orja-avaimen hyväksyminen, jotta koneet voivat kommunikoida keskenään.

  		master$ sudo salt-key -A
Unaccepted Keys:
orja1
Proceed? [n/Y]
Key for minion orja1 accepted.

  


Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves, vain kohdat

Infra as Code - Your wishes as a text file

Init.sls tiedostolla voidaan kätevästi hallita tiloja. Tehdään hakemisto, jonk sisälle init.sls

 		sudo mkdir -p /srv/salt/hello
		$ sudoedit /srv/salt/hello/init.sls
.
Katsotaan sisälle

		$ cat /srv/salt/hello/init.sls

  Sisällä on file managed (sen voisi varmasti vaihtaa ihan miksi haluaisi) eli haluttu lopputulos on uusi luoto tiedosto
  
		/tmp/infra-as-code:
  			file.managed

			$ sudo salt '*' state.apply hello

   Kun hello tiedostoa kutsutaan, se ajaa mitä sen sisällä on, tässä tapauksessa file.managed

top.sls - What Slave Runs What States

top.sls:llä voidaan määrittää mitkä orjat ajavat mitäkin lopputiloja (state)

 	sudo salt '*' state.apply hello^C
	$ sudoedit /srv/salt/top.sls
	$ cat /srv/salt/top.sls
	base:
  		'*':
    		- hello

Sen voi ajaa kaikilla orjilla

Now you don't have to name any modules on state.apply:


		$ sudo salt '*' state.apply

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

![ping1](https://github.com/user-attachments/assets/26b4ce9d-652c-47be-84b7-358c00db5bb0)

![ping2](https://github.com/user-attachments/assets/2c147d5f-6360-47d9-9e14-07a10089ba34)

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

Tässä kuva virheestä 

![Image](https://github.com/user-attachments/assets/d3790f3e-f8bf-4d2e-a11e-a64ebc24ba79)

Myöskin tämmöinen tuli

![mastervirhe2](https://github.com/user-attachments/assets/15bc52c4-0860-4ddf-8a51-e4a5c9f831d6)

![mastervirhe3](https://github.com/user-attachments/assets/3c5ffbe7-7cde-447b-80cc-52409c4ee329)

Nämä kaikki kuintenkin katosivat, ja kaikki toimi niinkuin pitikin salt-masterin poiston ja uudelleen asennuksen jälkeen. (Ainakin tähän asti)


e) Kokeile vähintään kahta tilaa verkon yli (viisikosta: pkg, file, service, user, cmd)




Kokeilin komentoa

 		sudo salt '*' cmd.run 'whoami'

   ![EItoimi1](https://github.com/user-attachments/assets/a8f640b0-a488-4bf6-baca-3c4274965e6a)

   ei toiminut 
![eitoimi2](https://github.com/user-attachments/assets/2a6056d7-7b91-46ae-80c2-77a6605ee91c)


 Tulee jatkuvasti Salt configured to run as user "salt" but unable to switch.


 Sen jälkeen ajoin 

   		vagrant destroy

ja 		

		vagrant up

  Saadakseni tuoreet koneet.

  Tein kaikki samat asennukset uudestaan 

      			 mkdir -p /etc/apt/keyrings


   Alkaen ja tein t001 masterin ja t002 orjan aivan kuin aikasemmin

   En saanut aivainta t002:lle 

   
![eiavaimia](https://github.com/user-attachments/assets/76f6a9aa-79a8-4f01-9bbf-ee30c43d0ed8)


Koetin pingata minonilla masteria ja tuli vaikka minkälaista. Pääasiallisesti Unable to sign_in to master: Attempt to authenticate with the salt master failed with timeout error ja [Errno 1] Operation not permitted: '/etc/salt/pki/minion'

![Minionping](https://github.com/user-attachments/assets/6b96c2c1-8dd0-4885-ad51-93d5afce0ac4)


  			
![minionerror](https://github.com/user-attachments/assets/d3e4b626-4a1d-491c-a958-f028b92adf67)

Ja nyt en edes avainta saadakseni luottamussuhdetta masterin kanssa. Se oli alunperinkin vaikeaa, mutta nyt sitä ei tule ollenkaan. Olen tarkastanut conf tiedostot ja avannut portit 4505 ja 4506 palomuurista.

Isoin ongelma on ollut avaimen näkymiseen saaminen

![miniondebug](https://github.com/user-attachments/assets/266ce654-84d4-414e-9dbc-e200b42403bf)


![mastererror2](https://github.com/user-attachments/assets/18c71c2d-f8ad-4a9e-8f55-0c1f28641acb)


Tämä olisi varmasti helpompi tehdä Linuxilla, mutta olen yllättynyt kuinka pitkällä olen päässyt. En tiedä yhtään mistä ongelmat johtuvat, olen kokeillut googlailla tuntikaupalla. Uskon, että se on yhdistelmä lukuisista käyttäjävirheistä ja Windowsistä (enemmän varmaan käyttäjän puolella jos mitään)

Olisin vielä ajanut esimerkiksi nämä komennot, jos olisin pystynyt. En tiedä kyllä miksi avainta ei edes tule näkyviin, jotta voisin hyväksyä sen.

			sudo salt '*' state.single user.present käyttäjä1
   			sudo salt '*' state.single pkg.installed apache2
      			sudo salt '*' state.single service.running apache2

 
Lähteet

Lähteet: Karvinen, Tero:,Nettisivu https://terokarvinen.com/ 

Karvinen, Tero: Nettisivu, 2025-26-03 https://terokarvinen.com/palvelinten-hallinta/ 

Karvinen, Tero: Nettisivu, 2021 https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ 

Karvinen, Tero: Nettisivu, 2018https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux  


Karvinen, Tero: Nettisivu,2023 ,https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file 

https://forums.virtualbox.org/viewtopic.php?t=96290 

https://portal.cloud.hashicorp.com/vagrant/discover/debian/bullseye64/versions/11.20241217.1 
