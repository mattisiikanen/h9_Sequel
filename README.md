# h9_Sequel



## Yrityssoftaa
Kuviteltu yritys nimeltä Yritys Oy tarjoaa verkkokauppaa, jossa myydään lankkuja eri muodoissa ja mitoissa. Potentiaalisten asiakkaiden tulee luoda tili Yritys Oy:n websivustojen kautta ja tilata tuotteet käyttäjätunnusta käyttäen. Tunnusta varten tulee tallettaa seuraavat omat tiedot: </br>
- Koko nimi
- Osoite
- Puh num
- S-posti


Tilausta tehtäessä tilaaja näkee tuotteiden varastosaldon livenä, täyttää ostoskorin haluamillaan tuotteilla ja kaupan yhteydessä ostettujen tuotteiden määrä päivittää varastosaldon jokaisen tuotteen kohdalla.


Tässä on hyvä esimerkki verkkokaupasta, jossa käytetään toiminnanohjausjärjestelmää, johon tarvitaan tietokantapalvelinta esim. asiakkaan & tuotteiden tietojen sekä varastosaldojen säilyttämiseksi. Etuja esimerkiksi pelkkään tietokantaan verrattuna, jossa pyöritetään / arkistoidaan tietoja, on verkkokauppaa pyörittävän palvelimen reaaliaikainen tietojen haku sekä toimitus tietokantaan erinäisten automaatioiden avulla. Etuina tässä myös voisi hyvinkin olla se, että verkkokauppaa ja tietokantaa tarjotaan palveluna jonkin kumppaniyrityksen toimesta ja siihen on olemassa tuki sen sijaan, että sitä tulisi ylläpitää aktiivisesti itse.


## Ympäristö

Hyper-V kotikoneella (Host):

- CPU: i5-9600K
- RAM: 16Gb
- HDD: 120Gb
- Win 11 Pro x64

Virtuaalikoneen speksit:

- 2 x CPU
- 4 Gb RAM
- 50 Gb HDD
- Generation 2 (Hyper-V pyytää määrittelemään)

Linode:n speksit:

- Linode Plan: Shared CPU - Nanode 1Gb (1 CPU, 25Gb Storage, 1Gb RAM)

## Aloitus 
Tehtävän tarkoituksena oli jatkaa tunnilla käytyjä tietokantaan liittyviä tehtäviä.
Aloitin tehtävät 14.2.2023 klo 20:13 käynnistämällä Hyper-V:llä pyörivän palvelimen, jonka kautta otin SSH-yhteyden Linodessa pyörivälle palvelimelle. Tein tunnilla jo PostgreSQL asennuksen virtuaalikoneelle, mutta ajattelin kokeilla sitä vielä Linodessa olevalle palvelimelle. 

## a) Postgre. Asenna PostgreSQL ja testaa se suorittamalla SQL-komento
Klo 20.33 </br>
Yhdistin Linoden palvelimeen komennolla ```ssh matti@143.42.18.59``` ja aloitin PostgreSQL:n asentamisen päivittämällä ensin repositoriot komennolla ```sudo apt-get update``` ja sitten itse PostgreSQL:n ```sudo apt-get -y install postgresql```: </br>
![Kuva1](https://user-images.githubusercontent.com/122887740/218827958-dab472b9-5fb2-43e3-addf-5a450018e845.png)


Asennuksen jälkeen jälkeen piti vielä käynnistää sudona itse PostgreSQL käyttäen komentoa ```sudo systemctl start postgresql``` sekä varmistin vielä palvelun käynnissä olon komennolla ```systemctl status postgresql.service```:</br>
![Kuva2](https://user-images.githubusercontent.com/122887740/218828533-6d6cefb2-8895-40e0-9a4e-1903c5fd1ea1.png)


Seuraavaksi vuorossa oli vielä luoda PostgreSQL:ään käyttäjä ja tietokanta, koitin ensimmäisenä komentoa ```sudo -u postgres createdb matti```: </br>
![Kuva3](https://user-images.githubusercontent.com/122887740/218831965-05bf163a-8e2b-43a3-8d90-ab485aaf5de2.png)</br>
Kappas jotain kieliasetuksia puuttuu. Tästä hätkähtyneenä päätin Googlettaa ongelmaa ja päädyin Stack overflow:n sivuille artikkeliin Postgres locale error.
Forumin ohjeita noudattamalla pääsin eteenpäin ja ajoinkin seuraavia komentoja:</br>


```
- sudo bash
- export LANGUAGE="en_US.UTF-8"
- echo 'LANGUAGE="en_US.UTF-8"' >> /etc/default/locale
- echo 'LC_ALL="en_US.UTF-8"' >> /etc/default/locale
```


![Kuva4](https://user-images.githubusercontent.com/122887740/218832381-f3558a53-943e-45fe-bec9-55e72f7cf636.png)</br>


Toimenpiteiden jälkeen kirjauduin sessiosta ulos ja uudelleen sisälle nähdäkseni, oliko asetuksillani mitään vaikutusta. Käytin komentoja: ```sudo -u postgres createdb matti``` & ```sudo -u postgres createuser matti```:</br>
![Kuva5](https://user-images.githubusercontent.com/122887740/218832804-ee69e44d-7d01-479d-bfd9-a7e509e05135.png)


Tekemilläni toimenpiteillä näytti olevan vaikutusta, vaikkakin se oli jo virheistä huolimatta luonut tietokannan.


## b) Crud. Kokeile CRUD (create, read, update, delete) kirjoittamalla SQL-käsin
Klo 21:06</br>

Asennuksen jälkeen tuli vielä tehdä CRUD testi tietokantaan. Minulla ei ollut aiempaa kokemusta SQL-kielestä, joten jouduin turvautumaan opettajamme Teron artikkeliin / ohjeeseen aiheesta "PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu".

Aloitin kirjautumalla sisälle PostgreSQL:n komennolla ```psql```, päästyäni sisään loin ensimmäisenä taulukon komennolla ```CREATE TABLE oppilaat (id SERIAL PRIMARY KEY, nimi VARCHAR(200));```. Luonnin jälkeen varmistin vielä komennolla ```\d```, että taulukko oli luotu oikein:</br>
![Kuva6](https://user-images.githubusercontent.com/122887740/218834580-a46a0b52-933f-4d2e-b032-6d9e6819735f.png)

Seuraavana vuorossa oli syöttää tietoa taulukkoon komennolla ```INSERT INTO oppilaat(nimi) VALUES ('Masa');```, päätin vielä rohkeasti yrittää lisätä pari muuta käyttäjää: ```Keijo``` ja ```Kustaa```. Tietojen lisäyksen jälkeen tuli vielä tarkistaa taulukon tiedot komennolla ```SELECT * FROM oppilaat;```: </br>

![Kuva7](https://user-images.githubusercontent.com/122887740/218835843-f7319558-4714-4841-9f81-63c6a16317bc.png) </br>

Nyt kun taulukko oli luotu onnistuneesti, oli aika testata muokata jonkun taulukossa olevan nimen tietoja. Valitsin uhriksi nimen Masa, joka oli tarkoituksena muuttaa Matti S:ksi. Muutosta varten ajoin seuraavan komennon ```UPDATE oppilaat SET nimi='Matti S' WHERE nimi='Masa';```: </br>

![Kuva8](https://user-images.githubusercontent.com/122887740/218836599-0c64b24c-058b-4648-bf3e-bd9c92506b9e.png) </br>

Lopuksi vielä tuli testata tiedon poisto taulukosta. Otin uhriksi tällä erää Keijon ja päätin hänen oppilasuransa komennolla: ```DELETE FROM oppilaat WHERE nimi='Keijo';```: </br>
![Kuva9](https://user-images.githubusercontent.com/122887740/218837027-f1038b44-7e32-4c8c-a0c8-86dbbd323ec7.png) </br>

Oli aika pakata työkalut ja lopettaa SQL istunto komennolla: ```exit```

## n) Vapaaehtoinen: Maria. Asenna MariaDB ja kokeille sillä CRUD
Klo 21:25</br>

Viimeisenä harjoituksena oli asentaa vielä MariaDB koneelle. Käytin jälleen opettajamme Teron luomia ohjeita asennusta varten. Ennen asennusta varmistin kuitenkin virtuaalikoneen levytilan komennolla ```df -h```: </br>
![Kuva10](https://user-images.githubusercontent.com/122887740/218837800-1c0a17df-c304-49b1-84ce-715aeac9c6f1.png)</br>

Tein tarkistuksen, koska eräällä oppilaalla oli loppunut tila omasta virtuaalikoneestaan ja halusin välttää omalla koneellani vastaavan tilanteen. Nyt kun tilaa kuitenkin oli ihan riittävästi, etenin asennus vaiheeseen.

Ohjeen mukaan palomuuriin pitäisi ennen asennusta sallia portti 22 TCP-protokollalle. Tämä oli jo kuitenkin tehty jo edellisviikon tehtävissä, joten annoin asian olla.


Hyppäsin myös yli repositorioiden päivityksen, koska tein sen jo edellisissä harjoituksissa ja siirryin suoraan asentamaan MariaDB:tä komennolla: ```sudo apt-get -y install mariadb-client mariadb-server```. Asennus onnistui hyvin ja seuraavaksi oli tarkoitus aloittaa ohjelman konfigurointi komennolla: ```sudo mysql_secure_installation``` </br>
![Kuva11](https://user-images.githubusercontent.com/122887740/218839433-614b30f1-4fef-4c9b-95ad-cb7d02fda466.png) </br>
Ohjeissa ei ollut mainintaa kyseisestä kohdasta, tarkistin asiaa MariaDB:n omasta wikistä, jossa sen mainittiin olevan hyvä olla päällä.


Loput kohdista olivatkin Teron ohjeissa ja seurasinkin niitä: </br>
![Kuva12](https://user-images.githubusercontent.com/122887740/218839824-ca967684-dd86-4c03-8122-0b04957d04a5.png)</br>


Seuraavaksi olikin luvassa käyttäjän luonti ja konfigurointi: </br>
![Kuva13](https://user-images.githubusercontent.com/122887740/218842766-27ad4a0e-5663-4a39-9d33-6d8111231206.png)</br>

Luonti ja konfigurointi onnistuivat hyvin.


Nyt oli luvassa kantaan kirjautuminen juuri luodulla tunnuksella sekä oli aika siirtyä testaamaan CRUDia MariaDB:n SQL:ssä.


Alussa loin uuden taulukon:</br>
![Kuva14](https://user-images.githubusercontent.com/122887740/218844923-e4bd2931-62c2-4949-b036-23a0b38dbb62.png) </br>


Taulukon jälkeen lisäsin sinne kuvitteellisia asioita ja hintoja ja luin taulukon tiedot: </br>
![Kuva15](https://user-images.githubusercontent.com/122887740/218846280-ca002dff-a053-48fa-9c77-989af897fcb2.png) </br>


Päätin alentaa Kuninkaan kilven hintaa 200:lla: </br>
![Kuva16](https://user-images.githubusercontent.com/122887740/218847003-8c51d048-95c9-4955-9df0-f7c9f6eed326.png) </br>


Lopultakin joku meni ostamaan Kuninkaan miekan: </br>
![Kuva17](https://user-images.githubusercontent.com/122887740/218847465-8767d831-900a-4baf-8aff-5b0b53aa1b21.png)


Testit olivat vihdoin ohitse ja onnistuneita. Nyt oli aika lopetella tältä erää.



## Lopetus
Lopetin tehtävien teon klo 21:57. Kyseinen tehtävä avasi hyvin tietokantoja sekä SQL-kielen käsittelyä. Töihin meni tällä erää n. 1,5h.

## Lähteet:

- Karvinen Tero, 3.3.2016, Install PostgreSQL on Ubuntu – New user and database in 3 commands 
(https://terokarvinen.com/2016/03/03/install-postgresql-on-ubuntu-new-user-and-database-in-3-commands/)

- Karvinen, Tero, 5.3.2016, PostgreSQL Install and One Table Database – SQL CRUD tutorial for Ubuntu 
(https://terokarvinen.com/2016/03/05/postgresql-install-and-one-table-database-sql-crud-tutorial-for-ubuntu/)

- Karvinen, Tero, 20.9.2018, Install MariaDB on Ubuntu 18.04 – Database Management System, the New MySQL 
(https://terokarvinen.com/2018/09/20/install-mariadb-on-ubuntu-18-04-database-management-system-the-new-mysql/)

- Stack overflow, Postgres locale error (https://stackoverflow.com/questions/17712700/postgres-locale-error)

- MariaDB.com, Authentication Plugin - Unix Sockethttps://mariadb.com/kb/en/authentication-plugin-unix-socket/

