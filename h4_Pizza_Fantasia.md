_Kurssi: Palvelinten hallinta ICI001AS3A-3011__

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 23.4.2026_

_Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h4 Piza Fantasia. Opettajana toimi Tero Karvinen._

________________________________________________________________________________________________________________________________________________________________________________________

x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.)
Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks (pdf, ̣mirrors: local, archive.org), vain nämä kohdat:
4.12.1 Size and Complexity of Some DSLs (112. Ominaisuuksien määrä.)
4.12.2 Use of DSL Functions in Case Configuration (112-115. Mitä oikeasti käytetään.)
4.12.3.1 Dependencies Between Main Functions (115-117. Tärkeimmät rakennuspalikat.)

________________________________________________________________________________________________________________________________________________________________________________________


### a) Räpylä. 
<sup<Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopputulos.</sup>

Päätin valita käyttööni **crondin**, jonka avulla on mahdollista ajastaa prosesseja, kuten luoda varmuuskopioita, lähettää sähköposteja tai kerätä systeemiraportteja. Demoni hyödyntää crontab -tiedostoja, josta se lukee toimenpiteet ja aikataulun. Asensin crondin käsin 

    apt update
    apt install crond -y

  <img width="658" height="291" alt="image" src="https://github.com/user-attachments/assets/9d5449a5-231b-4d81-8897-89c11c7e36c3" />


Seuravaksi loin tiedoston, jolla crondin toimintaa pystyttiin tastata.

    sudo micro /etc/cron.d/crontest

Tiedostoon kirjoitettin ajastus, joka merkataan tähdillä (*). Jokainen tähti kertoo crondille, kuinka usein toiminto tulee suorittaa: 1 = minuutti, 2 = tunti jne. Kyseinen tiedosto siis tulostaa tekstiä joka minuutti, tunti, päivä ja kuukausi. 

<img width="627" height="49" alt="image" src="https://github.com/user-attachments/assets/c0731453-2ea9-46b5-806d-3121cdfa443b" />

Lopuksi testattiin, toimivatko määritykset. 

<img width="510" height="186" alt="image" src="https://github.com/user-attachments/assets/907e27c6-8ae5-4d99-bfe8-9fff4f71e897" />

Nyt cron siis tulostaa viestin minuutin välein. 


________________________________________________________________________________________________________________________________________________________________________________________

### b) Automaatti. 
<sup>Automatisoi valitsemasi demonin asennus Ansiblella.</sup>


<img width="637" height="533" alt="image" src="https://github.com/user-attachments/assets/d99ca6db-860b-47f9-a7c5-5c82105f15dc" />


<img width="616" height="36" alt="image" src="https://github.com/user-attachments/assets/2841daa2-735c-4ded-80cd-eb6eb3cafb81" />

Toimii
________________________________________________________________________________________________________________________________________________________________________________________


### c) Asetus. 
<sup>Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.</sup>

Muokkasin asetustiedostoa Ansible-koneella (control node) vaihtamalla sinne "Asetuksia muokattu" -rivin. 

      micro apache/roles/cron/files/testcron
      * * * * * root echo "cron toimii $(date)" >> /tmp/cron_test.log #Lisätty "MUUTETTU" -teksti
      
Kun Ansible ajettiin, muutokset olivat nähtävissä cron-logissa.

        cat /tmp/cron_test.log

<img width="644" height="93" alt="image" src="https://github.com/user-attachments/assets/fc341c52-7b47-4eb1-88d3-6f8f344a6670" />



________________________________________________________________________________________________________________________________________________________________________________________


### d) Paikka remonttiin. 
<sup>Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.</sup>

Päätin käyttää esimerkin 'sudo apt-get purge cron' -menetelmää asetusten rikkomiseen. Purge -komento siis poistaa niin cron-paketin että sen asetustiedostot. 

        sudo apt purge cron

Tarkastettiin, että komento todella rikkoi cronin. 

        systemctl status cron
        which cron

<img width="839" height="118" alt="image" src="https://github.com/user-attachments/assets/d88cf5e1-09a2-4fb7-b650-18a719b15538" />

Kun cron oli todistetusti poistettu, voitiin ajaa uudelleen sama tuttu pelikirja. Sen tulisi nyt asentaa cron ja lokiin tulisi päätyä lisää tekstiä. Uutta cron ajoa jouduttiin odottamaan minuutti. 

    ansible-playbook -i hosts.ini site.yml #Aejataan pelikirja
    cat /tmp/cron_test.log

<img width="679" height="77" alt="image" src="https://github.com/user-attachments/assets/90841155-2b99-44c5-800b-2acd5550d1cb" />


________________________________________________________________________________________________________________________________________________________________________________________


### e) Idempotentti. 
<sup>Osoita, että tilasi on idempotentti.</sup>

Idempotentti voidaan todentaa ajamallla playbook. Se kertoo, onnistuiko vai epäonnistuiko tehtävä ja tehtiinko muutoksia (changes). Tavoitteena siis on, että jos mitään muutettavaa ei ole, Ansible ei tee turhaa työtä vaan ohittaa jo voimassaolevat asiat. Kun aiemmassa tehtävässä d) Paikka remonttiin ajoin pelikirjan, cron asennettiin ja pelikirjan Recapissa nähtiin, että muutoksia tehtiin. Nyt kun pelikirjan ajoi uudestaan, cronia ei asennettu uudelleen eikä muutoksia täten tehty. 

<img width="865" height="134" alt="image" src="https://github.com/user-attachments/assets/a2287efa-ad9c-4300-99f2-7fe6be9fe026" />

**_Aiempi cronin asennus teki muutoksia_**


<img width="861" height="183" alt="image" src="https://github.com/user-attachments/assets/275f63fe-79d4-471a-bceb-b48bdabff8c2" />

**_Toinen ajo ei enää tehnyt muutoksia (changes = 0)._**



________________________________________________________________________________________________________________________________________________________________________________________

Lähteet:

Karvinen, T. Configuration Management of Distributed Systems over Unreliable and Hostile Networks. 2024. Luettavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks. Luettu 20.4.2024.

Wilson, J. Crond: Daemon to Execute Scheduled Commands. Rosehosting. 2024. Luettavissa: https://www.rosehosting.com/blog/crond-daemon-to-execute-scheduled-commands/. Luettu 20.4.2026.
