_Kurssi:_ _Palvelinten hallinta ICI001AS3A-3011_

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 6.4.2026_

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h1 Hello Ansiblen Maailma. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.)
Karvinen 2026: Sudo without password
Munroe 2006: xkcd 149: Sandwitch
Karvinen 2026: Passwordless Sudo with Ansible
Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.
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


________________________________________________________________________________________________________________________________________________________________________________________

b) Antero. Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella.

________________________________________________________________________________________________________________________________________________________________________________________

c) Package. Asenna kaksi pakettia ansiblella.

________________________________________________________________________________________________________________________________________________________________________________________
d) File. Kirjoita orjalle useamman rivin mittainen tiedosto Ansiblella. Määrittele sen omistaja, omistava ryhmä ja oikeudet. Käytä oikeuksille oktaalinumeroa, esim. "0600". Kerro, mitä oikeudet ovat symbolisessa muodossa, esim. "-rwxr--r--". Selitä, mitä kukin käyttäjä saa tehdä tuolle tiedostolle.

________________________________________________________________________________________________________________________________________________________________________________________

e) Jotain muuta. Näytä esimerkki ansiblen käskystä, jota ei ole vielä käsitelty kurssilla tai kotitehtävissä. Voit ottaa jonkun muun modulin kuin apt, file, copy, user tai authorized_key. Tai voit käyttää ominaisuutta, jota ei vielä ole demonstroitu. Jos tiivistystehtävässä x on mainittu ominaisuuksia, joita ei tunneilla tai läksyissä kokeiltu, nekin kelpaavat.

________________________________________________________________________________________________________________________________________________________________________________________

### Lähteet

Karvinen, T. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h2-voileipa. Luettu 6.4.2026.

Karvinen, T. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu 6.4.2026.

Karvinen, T. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/. Luettu 6.4.2026.
