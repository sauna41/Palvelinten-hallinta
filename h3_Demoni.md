Kurssi: Palvelinten hallinta ICI001AS3A-3011

Tekijä: Henri Äikäs

Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)

Päivämäärä: 13.4.2026

Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h3 Demoni. Opettajana toimi Tero Karvinen.

________________________________________________________________________________________________________________________________________________________________________________________

### **x) Lue ja tiivistä**

  (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio.)
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

Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia.

  ________________________________________________________________________________________________________________________________________________________________________________________

  
### **b) Moottorix** 

Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.)

  ________________________________________________________________________________________________________________________________________________________________________________________

  
### **c) Automoottorix** Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin.

________________________________________________________________________________________________________________________________________________________________________________________


### **d) Vapaaehtoinen bonus**

Osiris-T. Osiris-T asentaa Wazuhin ja tarvittavat virtuaalikoneet. Voit lähettää vihamielistä verkkoliikennettä (tcpreplay), siepata sen (suricata) ja saat tulokset suoraan dashboardille (wazuh). Enemmän alpha kuin se kreikkalainen kirjain. Mutta Oskari, Nico ja Arttu ilahtuvat, jos kokeilet. Häkämies, Saario, Mukkula 2026: Osiris-T. Häkämies etal 2026: How to Install.
