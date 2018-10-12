# becausethisisourproject

**Projet Docker**

# Déploiement d'un erp (odoo) avec une base Postgresql et fakeSMTP: 

**1ère méthode : docker en lignes de commandes**
================================================

__Prérequis :__
* docker
```sh
sudo apt install docker

```

Cloner le repository, entrer dans le dossier
```sh
git clone https://github.com/sammour/becausethisisourproject.git
cd becausethisisourproject
```

__modifier les droits pour le fichier entrypoint.sh__
```sh
sudo chmod 755 entrypoint.sh
```

__construire l'image de l'erp__
```sh
docker build -t <IMAGE NAME> .
```
ex:
```sh
docker build -t odoo .
```

__créer un network__
```sh
docker network create <NETWORK NAME> --subnet=172.22.0.0/16 --driver=bridge
```
ex:
```sh
docker network create my-network --subnet=172.22.0.0/16 --driver=bridge
```
On attachera les containers au network au moment de leur création.

__récupérer l'image docker hub de Postgresql et créer le conteneur__

```sh
docker run --name <DATABASE CONTAINER NAME> -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres --network <NETWORK NAME> -d postgres
```
ex:
```sh
docker run --name postgresql-container -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres --network my-network -d postgres
```

__Création du conteneur Odoo__
```sh  
docker run -it --name <ODOO CONTAINER NAME> -p 8069:8069 --network <NETWORK NAME> -d <IMAGE NAME>
```
ex:
```sh
docker run -it --name odoo-container -p 8069:8069 --network my-network -d odoo
```

__récupérer l'image docker hub de fakesmtp et créer le conteneur__
 ```sh
docker run -d --name <FAKESMTP CONTAINER NAME> -p 1025:25 -v /tmp/fakemail:/var/mail --network <NETWORK NAME> digiplant/fake-smtp
```
ex:
 ```sh
docker run -d --name fakesmtp -p 1025:25 -v /tmp/fakemail:/var/mail --network my-network digiplant/fake-smtp
```

__Pour relancer votre application après un redémarrage système__
```sh
docker start <DATABASE CONTAINER NAME> <ODOO CONTAINER NAME> <FAKESMTP CONTAINER NAME>
```
ex:
```sh
docker start postgresql-container odoo-container fakesmtp
```

Vous pouvez maintenant finir la configuration de Odoo en accédant à localhost:8069


__Configuration sous mac de Fakesmtp (simuler des envois de  mails):__

1. Accéder à Odoo, aller dans __Configuration -> Paramètres généraux__
2. Cocher la case __Serveur de messagerie externe__ puis cliquer sur __Serveur de messagerie sortant__
3. Vous devriez observer une liste vide, il faut cliquer sur __Créer__.
4. Décrire le serveur (pour la vue liste précédente), par exemple FakeSmtp, et dans les __Informations sur la connexion__
5. Afin de recuperer l'adresse ip fournie __en0:__ dans votre terminal faite un :
```sh ifconfig```
6. Ouvrez votre __utilitaire réseau__, saisissez __localhost__ dans :"Saisissez une adresse Internet pour rechercher les ports ouverts." et notez l'__Open TCP Port: 	1025   		blackjack__.
7. Revenez sur Odoo et indiquer l'adresse ip correspondant à l' __en0:__ dans le champs __Serveur SMTP__
5. Notez le port correspondant a blackjack __1025__ comme port et tester la connexion. Vous devriez observer un message de Succès.

Vous pouvez maintenant accéder aux messages attrapés par fakesmtp dans le dossier /tmp/fakemail


**2ème méthode : docker-compose**
=================================

__Prérequis :__
* docker
```sh
sudo apt install docker

```
* docker-compose
```sh
sudo apt install docker-compose
```
Cloner le repository, entrer dans le dossier, et lancer docker-compose
```sh
git clone https://github.com/sammour/becausethisisourproject.git
cd becausethisisourproject
docker-compose up 
```

Vous pouvez maintenant finir la configuration de Odoo en accédant à localhost:8069

Vous pouvez aussi administrer votre base de données en passant par Adminer : localhost:8081

__Configuration Fakesmtp (simuler des envois de  mails):__

1. Accéder à Odoo, aller dans __Configuration -> Paramètres généraux__
2. Cocher la case __Serveur de messagerie externe__ puis cliquer sur __Serveur de messagerie sortant__
3. Vous devriez observer une liste vide, il faut cliquer sur __Créer__.
4. Décrire le serveur (pour la vue liste précédente), par exemple FakeSmtp, et dans les __Informations sur la connexion__ indiquer 172.99.0.4(Méthode 1) ou 172.26.0.2(Méthode 2) dans le champs __Serveur SMTP__
5. Laisser 25 comme port et tester la connexion. Vous devriez observer un message de Succès.

Vous pouvez maintenant accéder aux messages attrapés par fakesmtp dans le dossier /email présent dans à la racine du projet.


**TODO**

* Fixer le scaling (csrf token fix)
