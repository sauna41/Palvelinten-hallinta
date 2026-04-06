_Kurssi:_ _Palvelinten hallinta ICI001AS3A-3011_

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 6.4.2026_

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h1 Hello Ansiblen Maailma. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### x) Lue ja tiivistä

  [**Karvinen 2026: Sudo without password**](https://terokarvinen.com/passwordless-sudo/)
  - Käyttäjälle voidaan mahdollistaa sudottaminen ilman salasanaa lisäämällä tämä määriteltyyn ryhmään.
  - Lisäämällä visudoon määrityksiä, esimerkiksi %sudoless ALL = (ALL) NOPASSWD: ALL mahdollistaa     kaikkien "sudoless" -ryhmässä olevien ajaa sudo -komentoja ilman salasanaa.
  - Toimii käytännössä logiikalla %group COMPUTERS: (RUNAS) TAG: COMMANDS. 

  
  [Munroe 2006: xkcd 149: Sandwitch](https://xkcd.com/149/)
  - Määrittelemällä _became: yes_ (kuvan "sudo make a sandwich) voidaan muuttua automaattisesti superkäyttäjäksi, jolloin hallittavat koneet eivät kyseenalaista vaan tottelevat pääkäyttäjää.
  
  [**Karvinen 2026: Passwordless Sudo with Ansible**](https://terokarvinen.com/passwordless-sudo-with-ansible/)
  - Käyttäjälle voidaan mahdollistaa salasanaton sudottaminen myös automatisoidusti Ansible-roolin avulla
  - Sudo-oikeudet määritellään sudoers.d -hakemistoon.
  - Esimerkin rooli lisää myös julkisen SSH-avaimen, jotta SSH-kirjautuminen toimii ilman salasanaa. Tämä mahdollistaa Ansiblen toiminnan automaattisesti.

  **Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.**
  - 
Kustakin vain
Johdantokappale (Usein MODULE alla, päättyy OPTIONS alkuun)
Nimetyt optiot selityksineen
Esimerkeistä (EXAMPLES) jokin helppo ja keskeinen esimerkki
'ansible-doc copy': content, dest, src; owner, group, mode;
'ansible-doc apt': name, state, update_cache.
'ansible-doc file': path, recurse, src, state. owner, group, mode;
'ansible-doc user': name; create_home, comment, groups, shell, state, system.
'ansible-doc authorized_key': user, key.

________________________________________________________________________________________________________________________________________________________________________________________

a) Sudoless. Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.



<img width="360" height="162" alt="image" src="https://github.com/user-attachments/assets/cad892cb-dab5-449f-9c37-0b86474b0977" />


<img width="233" height="56" alt="image" src="https://github.com/user-attachments/assets/e38462f5-06c0-4338-8e6c-10ac1063aff8" />

<img width="508" height="173" alt="image" src="https://github.com/user-attachments/assets/c2a5ea20-ed77-49d5-a665-2cd1d81905fd" />

<img width="544" height="125" alt="image" src="https://github.com/user-attachments/assets/c6674856-63fe-4050-8c96-1ac42b73926d" />


________________________________________________________________________________________________________________________________________________________________________________________

b) Antero. Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella.

<img width="799" height="311" alt="image" src="https://github.com/user-attachments/assets/164fbb18-2efa-46d9-882a-7dfd5ceef888" />

<img width="531" height="165" alt="image" src="https://github.com/user-attachments/assets/327d21ea-76e5-4ea7-b918-3f05b2783e78" />

<br>

Kokeilin vielä ajaa ansrilla sudokomennon, joka onnistui ilman salasanan syöttämistä.

    komento sudo echo "moi" onnistui ilman salasanaa

<br>

<img width="312" height="32" alt="image" src="https://github.com/user-attachments/assets/79cae8a9-fc25-4467-8492-11f3f1895c85" />



________________________________________________________________________________________________________________________________________________________________________________________

c) Package. Asenna kaksi pakettia ansiblella.

Loin uuden paketit.yml tiedoston, johon määritin taskiksi asentaa **Git & Nginx**. 

<img width="303" height="263" alt="image" src="https://github.com/user-attachments/assets/fbef9848-cbd7-49e5-856a-3b43da541517" />

Kun pelikirja oli ajettu, testasin vielä, että kummatkin paketit olivat asentuneet onnistuneesti

<img width="419" height="57" alt="image" src="https://github.com/user-attachments/assets/685a7773-830b-4c98-862f-205908f6d067" />


________________________________________________________________________________________________________________________________________________________________________________________

d) File. Kirjoita orjalle useamman rivin mittainen tiedosto Ansiblella. Määrittele sen omistaja, omistava ryhmä ja oikeudet. Käytä oikeuksille oktaalinumeroa, esim. "0600". Kerro, mitä oikeudet ovat symbolisessa muodossa, esim. "-rwxr--r--". Selitä, mitä kukin käyttäjä saa tehdä tuolle tiedostolle.

Esimerkiksi "-rwxr--r--" sisältää kolme eri kolmen kirjaimen pätkää eri omistajille. Kolme ensimmäistä merkkiä kuuluvat omistajalle, toiset kolme ryhmällä ja kolme viimeistä muille. Merkit voivat olla joko r (read), w (write) tai x (execute). Jos oikeus puuttuu, puuttuu myös merkki ja tilalla on vain viiva [RedHat](https://www.redhat.com/en/blog/linux-file-permissions-explained).

Oikeuksien oktaalinumerot kertovat samaa tietoa numeraalisesti. 

<img width="343" height="225" alt="image" src="https://github.com/user-attachments/assets/99833b90-7245-4677-8c8d-549772df21d4" />

<br>


<img width="542" height="180" alt="image" src="https://github.com/user-attachments/assets/88377630-be41-4396-a658-e2723cfa5290" />


________________________________________________________________________________________________________________________________________________________________________________________

e) Jotain muuta. Näytä esimerkki ansiblen käskystä, jota ei ole vielä käsitelty kurssilla tai kotitehtävissä. Voit ottaa jonkun muun modulin kuin apt, file, copy, user tai authorized_key. Tai voit käyttää ominaisuutta, jota ei vielä ole demonstroitu. Jos tiivistystehtävässä x on mainittu ominaisuuksia, joita ei tunneilla tai läksyissä kokeiltu, nekin kelpaavat.

________________________________________________________________________________________________________________________________________________________________________________________

### Lähteet

Karvinen, T. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h2-voileipa. Luettu 6.4.2026.

Karvinen, T. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu 6.4.2026.

Karvinen, T. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/. Luettu 6.4.2026.
