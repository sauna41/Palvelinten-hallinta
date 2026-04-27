_Kurssi: Palvelinten hallinta ICI001AS3A-3011__

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 27.4.2026_

_Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h4 Piza Fantasia. Opettajana toimi Tero Karvinen._

________________________________________________________________________________________________________________________________________________________________________________________

Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Kannattaa lisätä mukaan myös jokin oma havainto, idea tai kysymys.)
Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git?
Gitin käyttö on lähinnä 'git add --all && git commit; git pull && git push'. Selitä tuon komennon jokainen osa. Käytä apuna itse valitsemiasi lähteitä ja viittaa niihin.

________________________________________________________________________________________________________________________________________________________________________________________

### a) Online. 
<sup>Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun).Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "sunshine". Aiemmin tehty varasto ei kelpaa. (Muista tehdä varastoon tiedostoja luomisvaiheessa, esim README.md ja GNU General Public License 3) Edistyneemmille voi olla hauskaa etsiä ja kokeilla jokin muu palvelu kuin Github.</sup>

Loin GithHubiin uuden repositorion nimeltään "Sunshine-Palvelimet". Lisäsin ohjeiden mukaisesti README- ja GNU -tiedostot. 

<img width="1204" height="605" alt="image" src="https://github.com/user-attachments/assets/d3d63927-fd29-4549-85c4-21aef8ebda0e" />


________________________________________________________________________________________________________________________________________________________________________________________


### b) Dolly. 
<sup>Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.</sup>

Varmistin aluksi, että virtuaalikoneellani oli asennettu git.

<img width="473" height="85" alt="image" src="https://github.com/user-attachments/assets/3a209a40-2c6d-4678-937e-07179a6cd23c" />

Määritin gitin käyttöön käyttäjätiedot 

<img width="661" height="56" alt="image" src="https://github.com/user-attachments/assets/86cf4369-ce31-45e4-86ad-5cdc59e54f2a" />

Tämän jälkeen päästiin itse asiaan. Githubissa sijaitseva sunshine-palvelinten varasto kopioitiin paikalliseen käyttöön.

      git clone https://github.com/sauna41/sunshine-palvelimet.git #URL kopioitiin suoraan GitHubista ja loppuun lisättiin .git

<img width="667" height="119" alt="image" src="https://github.com/user-attachments/assets/173f1b9a-3cb9-4e12-bfa3-c04dfd8d0cfb" />

Nyt sunshine-palvelinten hakemistoon pystyttiin navigoimaan komentoriviltä. Kirjoitin repon luomisen yhteydessä tehtyyn README-tiedostoon päivityksen suoraan komentoriviltä

    echo "### README päivitetty komeentoriviltä käsin" >> README.md

Muutos ei näkyi nyt paikallisesti mutta ei vielä GitHubissa. Muutos täytyi siis committaa ja pushata ulos, jotta se synkronoituisi GitHubin näkymän kanssa. 

    git add README.md    # Lisätään tiedosto staging -alustalle
    git commit -m "Lisätty tekstiä README-tiedostoon    # Kirjoitetaan commit-message, joka kuvaa tapahtumia
    git pull   # Tarkistetaan onko GitHubin päässä ollut muutoksia ja tarvittaessa vedetään muutokset paikalliseen versioon
    git push   # Työnnetään muutokset GitHubiin

##### Salasana ongelmia

Kun yritin pushata muutokset maailmalle, törmäsin ongelmaan. Sain kuvan mukaisen virheilmoituksen:

<img width="735" height="86" alt="image" src="https://github.com/user-attachments/assets/a5c0aa1d-3778-4cdc-9a2a-da361ed94c79" />

Onnekseni virheilmoitus oli itselleni jo entuudestaan tuttu. Ymmärrykseni mukaan johtuen omista asetuksistani GitHubissa, salasanakirjautumista ei hyväksytä vaan joudun käyttämään Personal Access Tokenia (PAT). Tokenin saa luotua ja otettua käyttöön helposti GitHubin päästä

--> Settings
  ---> Developer Settings
    ---> Personal access tokens
      ---> Generate New Token (annettiin repo-oikeudet)

Tämän jälkeen saadaan oma henkilökohtainen PAT, jolla kirjautuminen onnistuu. Kokeilin siis pushata uudelleen mutta tällä kertaa niin, että salasanan sijaan syötin tokenin. 

<img width="471" height="189" alt="image" src="https://github.com/user-attachments/assets/1515adc0-2696-44d4-9e00-251a808a1a37" />

Push näytti nyt toimivan. Asia oli helppo varmentaa tarkastamlla, näkyivätkö muutokset GitHubissa. 


<img width="857" height="552" alt="image" src="https://github.com/user-attachments/assets/b23ccba6-daa3-41fd-a081-8e4160c2890b" />

Lisätty teksti saatiin onnistuneesti puskettua paikallisesti Githubiin.
________________________________________________________________________________________________________________________________________________________________________________________


### c) Doh! 
<sup>Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.</sup>



________________________________________________________________________________________________________________________________________________________________________________________


d) Tukki. Tarkastele ja selitä varastosi lokia. Näytä myös, mitä muutoksia tiedostoihin on tehty. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

________________________________________________________________________________________________________________________________________________________________________________________


e) Gitanbile. Laita Ansible-kansio versionhallintaan. Tee jokin muutos, aja ansiblella, tallenna versio (commit).

________________________________________________________________________________________________________________________________________________________________________________________


f) Hae pari projektiin Moodlen keskustelusta. (Tästä alakohdasta f ei tarvitse tehdä vaiheittaista teknistä raporttia, riittää kun toteat, että pari on hankittu.)

________________________________________________________________________________________________________________________________________________________________________________________


g) Vapaaehtoinen: Se toinen järjestelmä: kokeile Gittiä eri käyttöjärjestelmällä kuin sillä, millä teit muut harjoitukset. Selitä niin, että kyseistä järjestelmää osaamatonkin onnistuu. Mahdollisuuksia on runsaasti: Debian, Fedora, Windows, OSX...
h) Vapaaehtoinen: yhteistyötä: anna kaverillesi (tai alter egollesi) oikeus kirjoittaa varastoosi (commit access). Tehkää molemmat muutoksia varastoon gitillä.
