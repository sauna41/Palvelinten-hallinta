Kurssi: Palvelinten hallinta ICI001AS3A-3011
Tekijä: Henri Äikäs

Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)

Päivämäärä: 30.3.2026

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h1 Hello Ansiblen Maailma. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### x) Lue ja tiivistä. 

(Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.)
Karvinen 2026: SSH public key - Login without password

________________________________________________________________________________________________________________________________________________________________________________________

### a) Sshecrects: _Asenna SSH-demoni ja testaa se kirjautumalla SSH:lla_

Jotta SSH-yhteys voidaan ottaa, tarvitaan ensin SSH-palvelin. Asensin demonin komennoilla

      sudo apt update
      sudo apt install openssh-server -y

<img width="658" height="148" alt="SSH install" src="https://github.com/user-attachments/assets/76b349c1-6515-4976-8945-4c0541d09b1c" />


Kun asennus oli valmis, varmistin, että SSH oli käynnissä komennolla

      sudo systemctl status ssh
      
Lopuksi otin SSH-yhteyden localhostiin

      ssh henria@localhost


<img width="725" height="336" alt="image" src="https://github.com/user-attachments/assets/e0137d32-c83c-4beb-8bad-175891cb6033" />


________________________________________________________________________________________________________________________________________________________________________________________



### b) Pubkey: _Automatisoi ssh-kirjautuminen julkisella avaimella._

Loin uuuden avainparin (julkinen ja yksityinen). Se tapahtui komennolla

      ssh-keygen -t ed25519

Tämä loi avainparin, josta kopioin julkisen avaimen localhostiin. 

<img width="1056" height="202" alt="PublicToLocalhost" src="https://github.com/user-attachments/assets/81fe58f6-60e9-46f2-8260-a8d22d7ae33b" />


<img width="981" height="200" alt="SSHSuccess" src="https://github.com/user-attachments/assets/e06bd155-6433-4559-951e-31c009443a69" />


________________________________________________________________________________________________________________________________________________________________________________________

### c) Hei Ansible: __Tee hei maailma ansiblella ja kokeile sitä SSH:n yli._

Loin uuden hosts.ini tiedoston, johon määrittelin tehtävän. Sen tuli tulostaa teksti "Hei maailma! Tämä viesti tulee Ansiblen kautta SSH-yhteydellä". 



<img width="727" height="155" alt="image" src="https://github.com/user-attachments/assets/6b832aa6-3cdf-4743-ba75-ee2d101f800c" />


<img width="1061" height="348" alt="HeiAnsibletulostus" src="https://github.com/user-attachments/assets/428aa874-de9e-4583-a24b-7c00293dc6f0" />



________________________________________________________________________________________________________________________________________________________________________________________

### d) Vapaaehtoinen bonus: Uuden käyttäjän luominen Ansiblella


<img width="1063" height="298" alt="image" src="https://github.com/user-attachments/assets/c724def8-6bac-46ea-9df6-ca0296ac27f7" />


________________________________________________________________________________________________________________________________________________________________________________________

### Lähteet

Karvinen, T. Palvelinten hallinta kurssimateriaali. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu 30.3.2026.

Karvinen, T. Hello Ansible. Luettavissa: https://terokarvinen.com/hello-ansible/. Luettu 30.3.2026.

Karvinen, T. SSH public key - Login without password. Luettavissa: https://terokarvinen.com/ssh-public-key-login-without-password/. Luettu 30.3.2026.
