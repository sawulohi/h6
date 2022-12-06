# h6 Kulkurin projekti

Testaukset tehty VirtualBoxin (Version 6.1.26 r145957 (Qt5.6.2)) kautta Ubuntu 22.04 LTS sekä Debian 11 (Bullseye)-distroilla. Prosessorina Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz

## x) Lue ja tiivistä (muutamalla ranskalaisella viivalla per artikkeli, poimi esim itsellesi keskeisimmät komennot)

[Karvinen 2017: Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds (Suosittelen käyttämään tässä koneena 'vagrant init debian/bullseye64')](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/)

- Pikaohjeet Vagranttiin (ja myös virtualboxin asennus)
- Uusi kone Vagrantilla esim:

 ```
 $ vagrant init debian/bullseye64
 $ vagrant up
 #ssh-yhteyden luonti
 $ vagrant ssh
 ```
- Virtuaalikoneen poistaminen `$ vagrant destroy`

[Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

- Sisältää yllämainitut ohjeet ohjelmistojen asentamiseen
- `Vagrantfile`-tiedostoon luodaan (tässä tapauksessa) kahden virtuaalikoneen luonti samanaikaisesti
- Koneita pystyy luomaan niin monta kuin haluaa lisäämällä Vagrantfileen lisää koneenmääritysrivejä
- Koneita (hosteja) voi testata esimerkiksi pingaamalla
- Sisältää myös troubleshootingin jos esimerkkinä annettu ip-osoite on järjestelmän sallitun rangen ulkopuolella

[Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/)

- Tiivistys kopioitu aiemmista läksyistä: [https://github.com/sawulohi/Palvelinten-Hallinta-Syksy-2022/blob/main/h2_package_file_service.md](https://github.com/sawulohi/Palvelinten-Hallinta-Syksy-2022/blob/main/h2_package_file_service.md)
- Ohjeistus Salt Masterin ja Salt Minionin asennukseen sekä Minionin konfigurointiin
- Masterin asennuskomennot ja oman masterin osoitteen selvitys:
``` 
$ sudo apt-get update
$ sudo apt-get -y install salt-master
$ hostname -I
10.0.2.15
```
- Palomuurin ollessa käytössä portit 4505/tcp ja 4506/tcp tulee avata
- Slaven asennus ja konfigurointi masteriin yhdistämistä varten:
``` 
$ sudo apt-get update
$ sudo apt-get -y install salt-minion
$ sudoedit /etc/salt/minion
```
Laitetaan minion config-tiedostoon masterin osoite ja minionille haluttu nimi.
```
master: 10.0.2.15
id: minion-1
```
Tallenna ja poistu editorista. Käynnistä vielä demoni uudelleen, jotta säädetyt asetukset tulevat voimaan:
```
$ sudo systemctl restart salt-minion.service
```
- Hyväksy uuden orjan avain *masterilla*:
```
$ sudo salt-key -A
The following keys are going to be accepted:
Unaccepted Keys:
minion-1
Proceed? [n/Y] Y
Key for minion minion-1 accepted.
```
Testaukset esimerkiksi (*masterilla*):
```
$sudo salt '*' cmd.run 'whoami'
minion-1:
    root
``` 
