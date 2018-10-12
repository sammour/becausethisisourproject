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
docker network create <NETWORK NAME>
```
ex:
```sh
docker network create my-network
```

__récupérer l'image docker hub de Postgresql , créer le conteneur et rattacher au network__

```sh
docker run --name <DATABASE CONTAINER NAME> -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres --network <NETWORK NAME>
```
ex:
```sh
docker run --name postgresql-container -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres --network my-network
```

__demarrer le conteneur__
```sh  
docker run -it --name <ODOO CONTAINER NAME> -p 8069:8069 --network <NETWORK NAME> -d <IMAGE NAME>
```
ex:
```sh
docker run -it --name odoo-container -p 8069:8069 --network my-network -d odoo
```

__récupérer l'image docker hub de fakesmtp et créer le conteneur__
 ```sh
docker run -d --name <FAKESMTP CONTAINER NAME> -p 1025:25 -v /tmp/fakemail:/var/mail digiplant/fake-smtp
```
ex:
 ```sh
docker run -d --name fakesmtp -p 1025:25 -v /tmp/fakemail:/var/mail digiplant/fake-smtp
```

__rattacher le conteneur de fakesmtp au network__
```sh
docker network connect <NETWORK NAME> <CONTAINER NAME>
```
ex:
```sh
docker network connect my-network fakesmtp
```

__Pour relancer votre application après un redémarrage système__
```sh
docker start <ODOO CONTAINER NAME>
```
ex:
```sh
docker start odoo-container
```

Vous pouvez maintenant finir la configuration de Odoo en accédant à localhost:8069


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
