Kurssi: Palvelinten hallinta ICI001AS3A-3011

Tekijä: Henri Äikäs

Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)

Päivämäärä: 13.4.2026

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h3 Demoni. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### **x) Lue ja tiivistä**

Karvinen 2026: Apache installed with Ansible - quick notes

Ansible Community Documentation: Handlers: running operations on change

Handlers: running operations on change (johdantokappale pääotsikon alta)
Notifying handlers

'ansible-doc service':
johdantokappale (MODULE alta)
enabled
name
state
EXAMPLES

________________________________________________________________________________________________________________________________________________________________________________________


### **a) Apassi** 
<sup>_Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia._</sup>

Olin asentanut jo aiemmin käyttööni Apache2 -demonin. Sen asennus tapahtui komennoilla 

    sudo apt install apache2 -y  # asentaminen
    sudo systemctl start apache2  # käynnistäminen
    
Tämän jälkeen palvelimelle yhdistäessä saatiin esiin Apachen etusivu, mikä kertoi sen olevan toiminnassa. 

<img width="957" height="566" alt="image" src="https://github.com/user-attachments/assets/ef48ed90-d057-4d62-8112-b18f4a527065" />

Sivua tuli pystyä muokkaamaan ilman root- tai sudo-oikeuksia tavallisena käyttäjänä. Nämä määritykset pystyttiin asettamaan komennoilla

    sudo chown -R $USER:$USER /var/www/html #hakemiston omistajuus käyttäjälle
    chmod -R u=rwx,g=rx,o=rx /var/www/html #oikeuksien määritys 

<img width="626" height="39" alt="image" src="https://github.com/user-attachments/assets/c645423c-3a3a-415a-9caa-c916cdac386f" />

Lopuksi muokattiin vielä index.html sisältöä, jotta voitiin testata, näkyivätkö muutokset palvelimella. 

    micro /var/www/html/index.html #HTML-tiedoston muokkaaminen

<img width="603" height="253" alt="image" src="https://github.com/user-attachments/assets/66700574-8863-49c7-9ffd-614fa89b92b9" />


Lopputuloksena Apachella pyörivä sivu, jonka sisältöä voi ilman liian kovia oikeuksia. 

<img width="589" height="264" alt="image" src="https://github.com/user-attachments/assets/7408b487-446d-49aa-ab6b-c6c3e74524ce" />
________________________________________________________________________________________________________________________________________________________________________________________

  
### **b) Moottorix** 
<sup>_Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.)_</sup>

Tehtävänanto oli hyvin samanlainen kuin aiemmassa Apache2 -tehtävässä. Nginx eli engine x asennettiin käyttöön ja käynnistettiin. Huomioitavaa oli kuitenkin se, että portti 80 on HTTP-oletusportti, joka oli vielä varattuna Apachelle ja se tuli sammuttaa ennen kuin nginx pystyi ottamaan siihen yhteyttä.

      sudo apt install nginx # asennetaan gninx
      sudo systemctl stop apache2 #sammutetaan apache
      sudo systemctl start nginx #köynnistetään nginx

Verkkoselaimessa navigoidessa localhostiin ei kuitenkaan ilmestynyt nginxin etusivu, vaan aiemman Apache-tehtävään tarkoitettu index.html sisältö. Tajusin tässä kohtaa, että nyt Apache2 ja Nginx olivat konfattu käyttämään samaa root-polkua, eli _/var/www/html._ Päätin luoda kummallekin omat hakemistot niin selkeyden kun monikäyttöisyyden vuoksi. Tämä mahdollisti kummankin demonin "rinnakkaisen" toiminnan ja kahden eri palvelimen pyörittämisen. 

Loin uudet hakemistot _/var/www/apache_ & _/var/www/nginx_. Apache-hakemistoon kopioin vanhan, käytössä olevan index.html -tiedoston ja Nginx -hakemistoon siirsin Nginxin oman index.nginx-debian.html -tiedoston.

    sudo mkdir /var/www/apache #luodaan hakemisto apachelle
    sudo mkdir /var/www/nginx #luodaan hakemisto nginxille
    sudo mv /var/www/html/index.html /var/www/apache/ #siirretään index.html uuteen hakemistoon
    sudo mv /var/www/html/index.nginx-debian.html /var/www/nginx #siirretään nginxin index oikeaan paikkaan

Tämän jälkeen käynnistin vielä Nginxin uudelleen, jolloin sain esiin sen oletussivun. 

<img width="720" height="361" alt="image" src="https://github.com/user-attachments/assets/0e9d7185-6d40-4398-9188-ff9708803d65" />

Muokkasin vielä sen index.html:ään samaan malliin kuin aiemmassa Apache2 -tehtävässä. Lopputuloksena sivu näytti seuraavanlaiselta:


<img width="545" height="261" alt="image" src="https://github.com/user-attachments/assets/f35d22dd-654a-40ea-a7f6-b88addf96ff7" />


#### Loppusilaukset

Tajusin vielä lopuksi, että vaikka olin aiemmassa tehtävässä määritellyt oikeudet niin, että palvelinta voisi hallita ilman root- tai sudo-oikeuksia, ne määritykset eivät enää koskeneet näitä kahta uutta hakemistoa. Lisäsin siis samat määritykset uudelleen uusille hakemistoille.

<img width="527" height="87" alt="image" src="https://github.com/user-attachments/assets/1bb52915-0992-495c-961b-002763365905" />

Kuten kuvasta nähdään, hakemistot ovat vielä rootin alla. Tavallinen käyttäjä ei siis pysty muokkaamaan niitä ilman riittäviä oikeuksia. Määriteltiin riittävät oikeudet:

    sudo chown -R $USER:$USER /var/www/apache #omistajuus apacheen
    sudo chown -R $USER:$USER /var/www/nginx #omistajuus nginxiin
    chmod -R u=rwx,g=rx,o=rx /var/www/apache #oikeudet apacheen
    chmod -R u=rwx,g=rx,o=rx /var/www/nginx #oikeudet nginxiin


Testattiin vielä, että oltiin päästy haluttuun lopputulokseen kirjoittamalla kansioon ilman sudoa.

<img width="885" height="119" alt="image" src="https://github.com/user-attachments/assets/10abc762-5e9b-4b88-b88c-b732c491a7e4" />


________________________________________________________________________________________________________________________________________________________________________________________

  
### **c) Automoottorix** 
<sup>_Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin._</sup>

Nyt kun asennus oli tehty manuaalisesti, oli järkevää kokeilla automatisoida se. Tutkiessani Tero Karvisen artikkelia Apache installed with Ansible - quick notes, en ollut vielä luottavainen taitoihini projektin hajottamisen files/handlers/taks osuuksiin. Aloitin lähestymisen siis yksinkertaisemmalla tavalla ja tarkastelemalla, saanko sen ensin toimintaan. Loin siis ensin uuden projektin ja sinne pelikirja

    mkdir /ansible/automoottorix #oma hakemisto
    micro ~/ansible/automoottorix/nginx_install.yml #pelikirjan luonti

<img width="293" height="334" alt="image" src="https://github.com/user-attachments/assets/2fad631c-e6a4-4707-9457-49d3e4986950" />

Hakemistoon luotiin vielä inventaario

    micro inventory #inventaarion luonti
    
Ajettiin pelikirja

    ansible-playbook -i inventory nginx_install.yml
<img width="955" height="397" alt="image" src="https://github.com/user-attachments/assets/fc97e9a8-cf07-48a7-980d-a9a29b21f6dd" />

Virhe ilmoituksia ei tässä kohtaa ilmaantunut. Minulla oli kuitenkin edellisen tehtävän jäljiltä nginx pyörimässä, joten playbook ei tehnyt helposti näkyviä muutoksia. Päätin todentaa pelikirjan toiminnan sammuttamalla nginxin ja muokkaamalla hieman index.html -tiedostoa aiemman Moottorix-tehtävän jäljiltä.

    sudo systemctl stop nginx

Varmistin, että nginx oli pois päältä: localhost näyttikin vekkoselaimessa "Unable to Connect". 

<img width="786" height="448" alt="image" src="https://github.com/user-attachments/assets/104ca081-0dd5-47b1-8443-08b62d26e0bc" />


Ajoin jälleen pelikirjan ja tarkastin taas verkkoselaimessa muutokset:


<img width="505" height="270" alt="image" src="https://github.com/user-attachments/assets/26c4000f-8d01-4fb4-a175-e606b567baec" />

#### Osa 2

Päätin kokeilla luoda projektin "oikeaoppisesti" niin, että hakemistopuu näytti tältä:

<img width="284" height="218" alt="image" src="https://github.com/user-attachments/assets/461b3291-1659-4d03-b143-9b8ab77f5f64" />


Aloitin luomalla nginx_install.yml playbookin automoottorix -hakemistoon. 

<img width="331" height="147" alt="image" src="https://github.com/user-attachments/assets/4553d84f-98d6-4d0f-b373-d81e43a6a0c5" />


**Tasks -hakemistoon loin**

-  copy siirtää tiedoston Ansible koneelta palvelimelle.
-  src kertoo lähdetiedoston
-  dest kertoo minne tiedosto web-palvelimella menee

<img width="354" height="339" alt="image" src="https://github.com/user-attachments/assets/951490c7-f3f8-49fc-98cb-ae09743e3071" />


**Handlers -hakemisto**

<img width="249" height="115" alt="image" src="https://github.com/user-attachments/assets/1d1bc595-f393-4373-b9f1-d608953b1ca9" />


**Files -hakemisto**

<img width="420" height="234" alt="image" src="https://github.com/user-attachments/assets/ef4e977f-a90d-498e-a994-edb2ccd15584" />


Lopuksi ajettiin ~/ansible/automoottorix -hakemistossa

    ansible-playbook -i inventory nginx_install.yml


Ja tarkateltiin verkkoselaimessa lopputulosta:
<img width="520" height="230" alt="image" src="https://github.com/user-attachments/assets/abe9fa2a-1ab1-4409-8174-11c42f80207f" />

________________________________________________________________________________________________________________________________________________________________________________________


### Lähteet

Karvinen, T. Palvelinten Hallinta. 2026. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu 13.4.2026.

Ansible Community Documentation. Kappaleet Handlers: running operations on change & Notifying handlers. 2026. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html#handlers-running-operations-on-change. Luettu 13.4.2026.

Karvinen, T. Apache installed with Ansible - quick notes. 2026. Luettavissa: 
https://terokarvinen.com/apache-ansible/. Luettu 13.4.2026.
