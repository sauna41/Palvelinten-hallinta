_Kurssi:_ Palvelinten hallinta ICI001AS3A-3011
_Tekijä:_ Henri Äikäs

_Alusta:_ Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)

_Päivämäärä:_ 30.3.2026

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h1 Hello Ansiblen Maailma. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### x) Lue ja tiivistä. 

**Karvinen 2026: SSH public key - Login without password**

- Julkisen avaimen SSH autentikoinnin avulla vältytään kirjoittamasta salasanaa joka kerta, kun halutaan ottaa SSH-yhteys. Se lisää myös tietoturvaa, sillä "salasana" ei voi vuotaa samanlailla. Kun salasana on luotu, se voidaan kopioida _authorized keys_ muistiin, jolloin avainta voidaan käyttää kirjautumiseen. Samaa avainta voi käyttää useammassa hostissa.

**Karvinen 2026: Hello Ansible**

- Ansible on konfiguraationhallintatyökalu, jossa infrastruktuuri kuvataan koodina (IaC). Se toimii SSH-yhteyden yli. Sen avulla voidaan kuvata lopputilanne ja Ansible suorittaa toimenpiteitä vain silloin, kun ne eivät täyty. Tämän avulla esimerkiksi jo voimassa olevaa ominaisuutta ei tehdä turhaan useaan otteeseen.

- Se tarvitsevat toimiakseen Inventoryn ja Playbookin. Niiden avulla voidaan määritellä mihintoimenpiteet kohdennetaana ja mitä tehdään. 
  
________________________________________________________________________________________________________________________________________________________________________________________

### a) Sshecrects: _Asenna SSH-demoni ja testaa se kirjautumalla SSH:lla_

Jotta SSH-yhteys voidaan ottaa, tarvitaan ensin SSH-palvelin. Asensin demonin komennoilla

      sudo apt update
      sudo apt install openssh-server -y

<img width="658" height="148" alt="SSH install" src="https://github.com/user-attachments/assets/76b349c1-6515-4976-8945-4c0541d09b1c" />


Kun asennus oli valmis, varmistin, että SSH oli käynnissä komennolla

      sudo systemctl status ssh
      
Lopuksi otin SSH-yhteyden localhostiin. Ensimmäisellä kerrralla tuli varoitus "The authenticity of host localhost can't be established". Tämä johtui siitä, että palvelin ei ollut vielä SSH-demonille tuttu. Vastatessa "yes", yhteys otettiin ja julkinen avain tallentui talteen known_hosts. Jatkossa SSH tiesi, että kyseinen palvelin on turvallinen ja otti yhteyden ilman varoitusta.

      ssh henria@localhost


<img width="725" height="336" alt="image" src="https://github.com/user-attachments/assets/e0137d32-c83c-4beb-8bad-175891cb6033" />


________________________________________________________________________________________________________________________________________________________________________________________



### b) Pubkey: _Automatisoi ssh-kirjautuminen julkisella avaimella._

Loin uuuden avainparin (julkinen ja yksityinen). Se tapahtui komennolla

      ssh-keygen -t ed25519

Tämä loi avainparin, josta kopioin julkisen avaimen localhostiin komennolla

      ssh-copy-id -i ~/.ssh/id_ed25519.pub henria@localhost



<img width="1056" height="202" alt="PublicToLocalhost" src="https://github.com/user-attachments/assets/81fe58f6-60e9-46f2-8260-a8d22d7ae33b" />


Tämän jälkeen varmistin yhteyden kirjautumalla käyttäjänä henria sisään. 


<img width="981" height="200" alt="SSHSuccess" src="https://github.com/user-attachments/assets/e06bd155-6433-4559-951e-31c009443a69" />


________________________________________________________________________________________________________________________________________________________________________________________

### c) Hei Ansible: __Tee hei maailma ansiblella ja kokeile sitä SSH:n yli._

Ansiblen toimintaan vaadittiin ns. "osoitekirja" (inventory) ja "pelikirja" (playbook). Osoitekirja määrittää missä asiat tapahtuvat ja pelikirja taas mitä tehdään. (Red Hat)

Loin aluksi hosts.ini -tiedoston, johon listataan kaikki tietokoneet, joita Ansiblen halutaan hallita. Itselläni hosts.ini kattoi siis hallittavan koneen IP-osoitteen (localhost), käyttäjänimen ja polun SSH-avaimeen.

<img width="686" height="42" alt="image" src="https://github.com/user-attachments/assets/4fc19631-21bb-4790-a109-1a1e88acf91d" />


Playbookkina taas puolestaan toimi toinen .yml tiedosto. Loin hei_maailma.yml tiedoston, johon määrittelin kuvauksen (name), kohderyhmän (local) ja tehtävän (task). Sen tuli tulostaa teksti "Hei maailma! Tämä viesti tulee Ansiblen kautta SSH-yhteydellä".  Tarkoitukseni oli siis se, että Ansible pystyi muodostamaan SSH-yhteyden SSH-avaimella ja suorittamaan komentoja kohdekoneella.


<img width="727" height="155" alt="image" src="https://github.com/user-attachments/assets/6b832aa6-3cdf-4743-ba75-ee2d101f800c" />



Ajoin kyseisen playbookin komennolla

      ansible-playbook -i hosts.ini hei_maailma.yml 


<img width="1061" height="348" alt="HeiAnsibletulostus" src="https://github.com/user-attachments/assets/428aa874-de9e-4583-a24b-7c00293dc6f0" />


________________________________________________________________________________________________________________________________________________________________________________________

### d) Vapaaehtoinen bonus: _Uuden käyttäjän luominen Ansiblella_

Loin uuden .yml tiedoston nimeltään uusi_kayttaja.yml. Kuten aiemmassa osiossa, määritin sille kuvauksen, ryhmän ja tehtävän. Koska uuden käyttäjän luominen on järjestelmänlaajuinen muutos, se vaatii "sudoa". Ansiblessa "become" tarkoittaa muuttumista root-käyttäjäksi eli se on ikäänkuin Ansiblen oma sudo. Sen avulla siis saatiin riittävät oikeudet uuden käyttäjän luomiseen. 

Lisättiin vielä "state: present", minkä avulla Ansible tarkistaa, onko "testihekilö" olemassa. Jos käyttäjä on jo olemassa, Ansible ei enää yritä lisätä toista (Idempotenssi). ShelL:/bin/bash puolestaan varmistaa, että testihenkilöllä on käytössään standardi shelli, eikä esimerkiksi riisuttu versio /bin/sh.

<img width="313" height="220" alt="image" src="https://github.com/user-attachments/assets/dfbda4f0-ab31-4a86-ad33-db3d4f041634" />


Testataan uuden käyttäjän luominen:


<img width="1063" height="298" alt="image" src="https://github.com/user-attachments/assets/c724def8-6bac-46ea-9df6-ca0296ac27f7" />


Varmistettiin uuden käyttäjän luominen 


<img width="392" height="42" alt="image" src="https://github.com/user-attachments/assets/2f47586f-b858-4067-8bf8-c1f49126c57e" />

________________________________________________________________________________________________________________________________________________________________________________________

### Lähteet

Karvinen, T. Palvelinten hallinta kurssimateriaali. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu 30.3.2026.

Karvinen, T. Hello Ansible. Luettavissa: https://terokarvinen.com/hello-ansible/. Luettu 30.3.2026.

Karvinen, T. SSH public key - Login without password. Luettavissa: https://terokarvinen.com/ssh-public-key-login-without-password/. Luettu 30.3.2026.

Red Hat. How Ansible works. Luettavissa: https://www.redhat.com/en/ansible-collaborative/how-ansible-works. Luettu 30.3.2026.
