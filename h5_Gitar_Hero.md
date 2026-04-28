_Kurssi: Palvelinten hallinta ICI001AS3A-3011__

_Tekijä: Henri Äikäs_

_Alusta: Intel i5 Macbook Pro MacOs Sequaoia 15.7.2 / Debian 13 trixie (VirtualBox)_

_Päivämäärä: 27.4.2026_

_Tämä raportti on osa Haaga-Helian Palvelinten hallinta -kurssia keväällä 2026. Tehtävänanto on h5 Gitar Hero. Opettajana toimi Tero Karvinen._

________________________________________________________________________________________________________________________________________________________________________________________

### Lue ja tiivistä: [What Is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

- Git eroaa monista muista versionhallinnoista siinä, että se tallentaa tietoa projekteista napsimalla ikään kuin tallennekuvia jokaisen commitin yhteydessä. Projektin edetessä Git vertaa ottamiaan "kuvia" aiempiin, josta se pystyy havaitsemaan muutokset.
- Lähes kaikki Gitin toiminta tapahtuu paikallisesti, joka tekee siitä erittäin nopean sekä taipuvaisen. Se ei ole riippuvainen verkkoyhteydestä tai toiselle puolelle maailmaa yhteyden ottamisesta. Commitit voidaan siis tehdä paikallisesti ja linkittää myöhemmin verkkoon.
- Git varmistaa, että tiedostot ovat eheitä käyttämällä SHA-1 hashia. Se siis muodostaa jokaisesta tiedostostaan 40-merkkisen arvon, joka muuttuu tiedostoa muokattaessa. Tällöin tiedostoja on käytännössä mahdotonta muokata ilman, että Git tietäisi asiasta.
- Git lähinnä lisää dataa. Koska se muistaa aina committien aikana tapahtuneet tilannekatsaukset, ei tiedon katoaminen ole helppoa. Sen avulla on myös turvallista testata, koska kaiken voi peruuttaa tai poistaa.
- Gitillä on käytössään kolme aluetta: _modified, staging & committed_.
      - modified kertoo, että tiedostoa on muokattu mutta sitä ei ole  vielä tallennettu
      - staged alueella olevat muutokset on ikään kuin nostettu työpöydälle valmiina seuraavaan tallennukseen (commit)
      - committed tarkoittaa, että tiedosto on tallennettu paikalliseen tietokantaan.


#### [Yleisimmät komennot](https://git-scm.com/docs)

- git add
      - Lisää kaikki muutokset staging-alueelle. 
- git commit
      - Tallentaa staging-alueella olevat tiedostot git-tietokantaan. 
- git pull
      - Hakee etärepositorion muutokset ja yhdistää ne paikalliseen haaraan
- git push
      - Puskee paikallliset muutokset etärepositorioon. 
________________________________________________________________________________________________________________________________________________________________________________________

### a) Online. 
<sup>Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun).Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "sunshine". Aiemmin tehty varasto ei kelpaa. (Muista tehdä varastoon tiedostoja luomisvaiheessa, esim README.md ja GNU General Public License 3) Edistyneemmille voi olla hauskaa etsiä ja kokeilla jokin muu palvelu kuin Github.</sup>

Loin GitHubiin uuden repositorion nimeltään "sunshine-Palvelimet". Lisäsin ohjeiden mukaisesti README- ja GNU -tiedostot. 

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

    echo "### README päivitetty komentoriviltä käsin" >> README.md

Muutos näkyi nyt paikallisesti mutta ei vielä GitHubissa. Muutos täytyi siis committaa ja pushata ulos, jotta se synkronoituisi GitHubin näkymän kanssa. 

    git add README.md    # Lisätään tiedosto staging -alustalle
    git commit -m "Lisätty tekstiä README-tiedostoon    # Kirjoitetaan commit-message, joka kuvaa tapahtumia
    git pull   # Tarkistetaan onko GitHubin päässä ollut muutoksia ja tarvittaessa vedetään muutokset paikalliseen versioon. Näin toimitaan aina ajantasaisen version kanssa.
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

Kirjoitin jälleen tekstiä README.md tiedostoon. 

      echo "Tämä teksti on virheellinen eikä kuuluisi tänne" >> README.md


Tarkastelin nyt gitin tilannetta 

      git status

<img width="538" height="170" alt="image" src="https://github.com/user-attachments/assets/7fba7c1e-f343-4c2c-8b34-cab8226bd070" />

Ilmoitus kertoo, että README.md tiedostossa on muutoksia mutta mitään ei ole vielä asetettu commitoitavaksi. Tämä on hyvä, sillä en halunnut tämän väärin tekstin päätyvään repoon. Ilmoitus ehdottaa, että käyttäisin git restore <file> -komentoa peruuttaakseni muutokset, mutta hyödynsin tällä kertaa tehtävännon mukaisesti 'git reset --hard' -komentoa. Erona näillä kahdelle on se, että 'git restore' palauttaa yksittäisen tiedoston viimeisimmän commitin tilaan kun taas 'git reset --hard' palauttaa koko projektin.

<img width="489" height="121" alt="image" src="https://github.com/user-attachments/assets/c01ad716-f173-4ad4-86f8-e6ea6cb2be4f" />

Komento palautti tilanteen aiempaan committiin asti. 'git status' kommennolla voitiin vielä todentaa, että "virheellinen" lisäys ei enää näkynyt lokissa muutoksena.
________________________________________________________________________________________________________________________________________________________________________________________


d) Tukki. 
<sup>Tarkastele ja selitä varastosi lokia. Näytä myös, mitä muutoksia tiedostoihin on tehty. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.</sup>

Lokia voitiin tutkia eri usealla eri komennolla. 

      git log    #perustarkastelu
      git log --oneline    #kompakti, yhden rivin loki
      git log --stat    #näyttää tiedostomuutokset
      git config --global user.name    #gittiin määritellyn käyttäjänimen tarkastaminen
      git config --global user.email    #gittiin määritellyn sähköpostin tarkastaminen

#### git log

<img width="669" height="204" alt="image" src="https://github.com/user-attachments/assets/3536a976-9970-4dec-bfc6-b118d5a0415c" />

Lokissa näkyi sunshine-palvelinten luominen, eli initial commit GitHubissa. Aiemmassa tehtävässä muokattu README -tiedostoon lisäys löytyi myös lokista. Myös "kirjaaja" oli tallentunut lokiin: GitHub käyttäjä sauna41 oli luonut repositorion ja Henri Äikäs tehnyt viimeisimmän muutoksen.

#### git log --stat

<img width="700" height="320" alt="image" src="https://github.com/user-attachments/assets/7b3bc236-bc38-4a95-aea1-5d03a9f2d5f3" />

'-- stat' lisäys tietoa hieman tarkemmin. Lisätietoina aiempaan verrattuna on, että komento tarkastaa mitä muutoksia on tehty ja kuinka paljon. README.md -tiedostoa muokkaamalla lisättiin tiedostoon yksi rivi, joka näkyy "1 insertion (+)" kohdassa. Jos tiedostosta olisi vastaavasta poistettu jotain, olisi sen tilalla deletions (-). Numeromäärä kertoo muutettujen **rivien** määrän.

#### git config --global user.name + user.email

<img width="596" height="73" alt="image" src="https://github.com/user-attachments/assets/a75191d7-bd6b-404b-89b5-361feb0b86e3" />

Komennoilla voitiin tarkastaa gittiin määritetty käyttäjänimi ja sähköpostiosoite. Nämä tiedot pitivät paikkansa, eivätkä täten vaatineet muutoksia. 

________________________________________________________________________________________________________________________________________________________________________________________


### e) Gitanbile.
<sup>Laita Ansible-kansio versionhallintaan. Tee jokin muutos, aja ansiblella, tallenna versio (commit).</sup>

Kurssilla paljon käytetyn Ansible-kansion lisääminen GitHubiin alkoi alustamalla kyseinen hakemisto gittiin. 

<img width="627" height="224" alt="image" src="https://github.com/user-attachments/assets/724d0a8f-d540-49a8-b8db-bc3976f2529d" />

Seuraavaksi lisäsin kaikki tiedostot

      git add --all
      git commit -m "Ansible-kansio versionhallintaan"

Tämän jälkeen muokkasin aiemmissa tehtävissä käytettyä paketit.yml -tiedostoa. Sen alkuperäinen tarkoitus oli asentaa git & nginx mutta lisäsin listalle myös apache2. Ajoin tämän jälkeen paketit.yml uudelleen. 

      ansible-playbook paketit.yml

Tallensin tuoreet muutokset lisäämällä ne staging-alustalle ja committaamalla ne kirjauksen kera

      git add --all
      git commit -m "Päivitetty paketit.yml asentamaan apache2"

Muutoksia pystyttiin jälleen tarkastelemaan lokin avulla. Koska ansible-kansiossa oli paljon tavaraa, en lähtenyt käsin etsimään kaiken seasta juuri paketit.yml muutoksia, vaan tarkensin niihin 'git show paketit.yml' komennolla

<img width="507" height="270" alt="image" src="https://github.com/user-attachments/assets/dc85a443-84a5-4937-92aa-49100579d5db" />

Tulostuksena näkyi siis juuri tekemäni commit message ja tiedostoon tehdyt muutokset. _-apache2_ rivi oli siis onnistuneesti lisätty myös versionhallintaan.
________________________________________________________________________________________________________________________________________________________________________________________


### f) Hae pari projektiin Moodlen keskustelusta.

Tämä on vielä työn alla!

________________________________________________________________________________________________________________________________________________________________________________________

### Vapaaehtoiset lisätehtävät

**Eivät vielä valmiina. Suoritan nämä loppuun ja päivitän tänne ennen kurssin päätöstä.**

g) Se toinen järjestelmä
<sup>kokeile Gittiä eri käyttöjärjestelmällä kuin sillä, millä teit muut harjoitukset. Selitä niin, että kyseistä järjestelmää osaamatonkin onnistuu. Mahdollisuuksia on runsaasti: Debian, Fedora, Windows, OSX...</sup>

h) Yteistyötä
<sup>anna kaverillesi (tai alter egollesi) oikeus kirjoittaa varastoosi (commit access). Tehkää molemmat muutoksia varastoon gitillä.</sup>


________________________________________________________________________________________________________________________________________________________________________________________


### Lähteet:

Karvinen, T. Palvelinten Hallinta opintojakson kurssimateriaali. 2026. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu 27.4.2026.

Chacon and Straub. Pro Git. 2014. Kappale 1.3. Getting Started - What is Git?. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu 27.4.2026.

Chacon and Straub. Pro Git. 2014. Dokumentaatio komennoista add, commit, pull & push. Luettavissa: https://git-scm.com/docs. Luettu 27.4.2026. 

