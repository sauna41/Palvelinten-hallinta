_Kurssi:_ _Palvelinten hallinta ICI001AS3A-3011_

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 6.4.2026_

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h2 Voileipä. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### x) Lue ja tiivistä

  [**Karvinen 2026: Sudo without password**](https://terokarvinen.com/passwordless-sudo/)
  - Käyttäjälle voidaan mahdollistaa sudottaminen ilman salasanaa lisäämällä tämä määriteltyyn ryhmään.
  - Lisäämällä visudoon määrityksiä, esimerkiksi %sudoless ALL = (ALL) NOPASSWD: ALL mahdollistaa     kaikkien "sudoless" -ryhmässä olevien ajaa sudo -komentoja ilman salasanaa.
  - Toimii käytännössä logiikalla %group COMPUTERS: (RUNAS) TAG: COMMANDS. 

  
  [Munroe 2006: xkcd 149: Sandwitch](https://xkcd.com/149/)
  - Määrittelemällä _become: yes_ (kuvan "sudo make a sandwich) voidaan muuttua automaattisesti superkäyttäjäksi, jolloin hallittavat koneet eivät kyseenalaista vaan tottelevat pääkäyttäjää.
  
  [**Karvinen 2026: Passwordless Sudo with Ansible**](https://terokarvinen.com/passwordless-sudo-with-ansible/)
  - Käyttäjälle voidaan mahdollistaa salasanaton sudottaminen myös automatisoidusti Ansible-roolin avulla
  - Sudo-oikeudet määritellään sudoers.d -hakemistoon.
  - Esimerkin rooli lisää myös julkisen SSH-avaimen, jotta SSH-kirjautuminen toimii ilman salasanaa. Tämä mahdollistaa Ansiblen toiminnan automaattisesti.

  **Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.**
  - 'ansible-doc copy': Tiedostojen siirtäminen hallintakoneelta kohdekoneelle
      - content: sisältö (tekstimuotoista)
      - dest: kohdepolku 
      - src: lähdetiedoston polku
      - owner: asettaa omistajat
      - group: asettaa ryhmän
      - mode: asetaa oikeudet (oktaali)
        
  - 'ansible-doc apt': Pakettien hallintaa (asennus, päivitys, poistaminen)
      - name: paketin nimi
      - state: paketin tila
      - update_cache: paketin päivitys ennen asennusta
        
  - 'ansible-doc file': Tiedostojen ominaisuuksien muuttamiseen, hakemistojen luomiseen
      - path: polku
      - recurse: rekursiivinen oikeuksien käyttö polulle
      - src: lähdepolku
      - state: hakemisto/tiedosto/linkki/poisto
      - owner: omistaja
      - group: ryhmä
      - mode: oikeusasetukset (oktaali)
      
  -  'ansible-doc user': Käyttäjien hallinta (luominen, muokkaus, poisto)
      - name: käyttäjätunnus
      - create_home: luodaanko kotihakemisto
      - comment: käyttäjän kuvaus
      - groups: ryhmät joihin käyttäjä määritellään
      - shell: oletuskomentotulkki (bin/bash)
      - state: luo tai poistaa käyttäjän
  
  - 'ansible-doc authorized_key': Lisää tai poistaa julkisia SSH-avaimia
      - user: käytäjä jonka avainlistaa muokataan
      - key: julkinen avain (merkkijono tai tiedostopolku)

________________________________________________________________________________________________________________________________________________________________________________________

### a) Sudoless. Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.

Loin aluksi uuden käyttäjän "sudoless".

<img width="360" height="162" alt="image" src="https://github.com/user-attachments/assets/cad892cb-dab5-449f-9c37-0b86474b0977" /> 

Avasin tämän jälkeen komennolla

    sudo visudo

määritykset, johon lisäsin viimeiselle riville uuden "sudoless" käyttäjän. Tämä määritti, että kyseinen käyttäjä pystyy sudottamaan ilman salasanaa. 

<img width="233" height="56" alt="image" src="https://github.com/user-attachments/assets/e38462f5-06c0-4338-8e6c-10ac1063aff8" />


Tämän jälkeen kopioin aiemmin luodun julkisen SSH-avaimen suoraan sudoless käyttäjälle. 

    ssh-copy-id sudoless@localhost


<img width="508" height="173" alt="image" src="https://github.com/user-attachments/assets/c2a5ea20-ed77-49d5-a665-2cd1d81905fd" />

Lopuksi testasin ottamalla SSH-yhteyden, mikä onnistui SSH-avaimella ilman salasanan kyselyä. 

<img width="544" height="125" alt="image" src="https://github.com/user-attachments/assets/c6674856-63fe-4050-8c96-1ac42b73926d" />


________________________________________________________________________________________________________________________________________________________________________________________

### b) Antero. Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella.

Aloitin luomalla uuden pelikirjan. Luotiin uusi "ansri" käyttäjä, lisättiin tälle julkinen SSH-avain ja määritettiin käyttäjä sudottajaksi. 

<img width="440" height="415" alt="image" src="https://github.com/user-attachments/assets/4a0c3175-294b-41f5-ae7f-012cfd26d8aa" />


Ajettin komennolla

      ansible-playbook sudolessSSH.yml

<img width="799" height="311" alt="image" src="https://github.com/user-attachments/assets/164fbb18-2efa-46d9-882a-7dfd5ceef888" />

Testattiin uuden käyttäjän luonti ottamalla SSH-yhteys, mikä onnistuikin ilman salasanaa. 

<img width="531" height="165" alt="image" src="https://github.com/user-attachments/assets/327d21ea-76e5-4ea7-b918-3f05b2783e78" />

<br>

Kokeilin vielä ajaa ansrilla sudokomennon, joka onnistui ilman salasanan syöttämistä.

    komento sudo echo "moi" onnistui ilman salasanaa

<br>

<img width="312" height="32" alt="image" src="https://github.com/user-attachments/assets/79cae8a9-fc25-4467-8492-11f3f1895c85" />



________________________________________________________________________________________________________________________________________________________________________________________

### c) Package. Asenna kaksi pakettia ansiblella.

Loin uuden paketit.yml tiedoston, johon määritin taskiksi asentaa paketit **Git & Nginx**. 

<img width="303" height="263" alt="image" src="https://github.com/user-attachments/assets/fbef9848-cbd7-49e5-856a-3b43da541517" />

Tämä ajettiin seuraavaksi komennolla

    ansible-playbook paketit.yml

Kun pelikirja oli ajettu, testasin vielä, että kummatkin paketit olivat asentuneet onnistuneesti. 

    git --version
    systemctl is-active nginx

<img width="419" height="57" alt="image" src="https://github.com/user-attachments/assets/685a7773-830b-4c98-862f-205908f6d067" />


________________________________________________________________________________________________________________________________________________________________________________________

### d) File. Kirjoita orjalle useamman rivin mittainen tiedosto Ansiblella. Määrittele sen omistaja, omistava ryhmä ja oikeudet. Käytä oikeuksille oktaalinumeroa, esim. "0600". Kerro, mitä oikeudet ovat symbolisessa muodossa, esim. "-rwxr--r--". Selitä, mitä kukin käyttäjä saa tehdä tuolle tiedostolle.


Loin uuden file.yml -tiedoston, joka loi uuden tekstitiedoston käyttäjän ansri -kotihakemistoon. Määritin pelikirjalle omistajan sekä ryhmän. Tekstitiedosto sisälsi useamman rivin tekstiä, joka oli tarkoitettu vain omistajan, eli ansrin käyttöön. Vain omistaja pystyi siis muokkaamaan ja katselemaan tiedostoa. Ryhmän jäsenille tai muille ei annettu kirjoitus tai katseluoikeuksia, joten oktaalinumeroksi muodostui "0600".  

<img width="543" height="234" alt="image" src="https://github.com/user-attachments/assets/4aa62532-a359-417d-a4cc-5a3b971c1a91" />


Ajoin pelikirjan komennolla

    ansible-playbook file.yml


Ja lopuksi testasin lopputuloksen. Otin siis SSH-yhteyden ansriin ja tarkastelin kotihakemistoa. Sieltä löytyi nyt tarkeat_ohjeet.txt, eli lopputulos oli onnistunut. 



<img width="542" height="180" alt="image" src="https://github.com/user-attachments/assets/88377630-be41-4396-a658-e2723cfa5290" />

<br>

#### Oikeuksista:

Esimerkiksi "-rwxr--r--" sisältää kolme eri kolmen kirjaimen pätkää eri omistajille. Kolme ensimmäistä merkkiä kuuluvat omistajalle, toiset kolme ryhmällä ja kolme viimeistä muille. Merkit voivat olla joko r (read), w (write) tai x (execute). Ne määrittelevät, ketkä voivat muokata, lukea tai ajaa tiedoston. Jos oikeus puuttuu, puuttuu myös merkki ja tilalla on vain viiva [RedHat](https://www.redhat.com/en/blog/linux-file-permissions-explained).

Oikeuksien oktaalinumerot kertovat samaa tietoa numeraalisesti. Read, Write & Execute ovat kaikki edustettuina numeroina ja niiden summa kertoo saman tiedon oikeuksista kuin symbolisissa muodossa. Kaikkien eri käyttäjäryhmien (User, Groups & Others) numerot muodostavat siis esimerkiksi oktaaliluvun 600, johon usein lisätään eteen luku 0. [GeeksForGeeks](https://www.geeksforgeeks.org/linux-unix/set-file-permissions-linux/)

<br>

<img width="506" height="291" alt="image" src="https://github.com/user-attachments/assets/bd2aeac5-fc8c-41f8-8e31-503516865a25" /> 

**[Oktaalinumerot GeeksForGeeks -sivustolta](https://www.geeksforgeeks.org/linux-unix/set-file-permissions-linux/)**





________________________________________________________________________________________________________________________________________________________________________________________

#### e) Jotain muuta. Näytä esimerkki ansiblen käskystä, jota ei ole vielä käsitelty kurssilla tai kotitehtävissä.

Loin service-moduulin, joka tarkistaa, että nginx on käynnissä ja tarvittaessa käynnistää sen. Loin uuden service.yml -tiedoston, johon määrittelin tarvittavat tiedot:

<img width="410" height="177" alt="image" src="https://github.com/user-attachments/assets/85da4eb1-4bc3-46a0-99af-6b2b73a05ef6" />


Varmistin testaamista varten, että nginx ei ollut valmiiksi päällä komennolla

      sudo systemctl stop nginx

Jonka jälkeen ajoin ansiblella service.yml ja testasin, menikö demoni päälle

<img width="657" height="72" alt="image" src="https://github.com/user-attachments/assets/75f3c5a6-2d1d-4929-b16e-702994f8da84" />

________________________________________________________________________________________________________________________________________________________________________________________

### Lähteet

Karvinen, T. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h2-voileipa. Luettu 6.4.2026.

Karvinen, T. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu 6.4.2026.

Karvinen, T. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/. Luettu 6.4.2026.

Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla. Luettu 6.4.2026.

GeeksForGeeks. Linux Permissions. Luettavissa: https://www.geeksforgeeks.org/linux-unix/set-file-permissions-linux/. Luettu 6.4.2026.

McBrien, S. Linux file permissions explained. Redhat. Luettavissa: https://www.redhat.com/en/blog/linux-file-permissions-explained. Luettu 6.4.2026.
