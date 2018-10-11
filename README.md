# becausethisisourproject

**Projet Docker**

# Déploiement d'un erp (odoo) : 

**1ère méthode : docker en lignes de commandes**

cloner le repository

construire l'image
```sh
docker build -t <IMAGE NAME> .
```
ex:
```sh
docker build -t odoo-tp .
```

créer un network
```sh
docker network create <NETWORK NAME>
```
ex:
```sh
docker network create my-network-odoo
```

récupérer l'image docker hub et créer le conteneur

```sh
docker run --name <DATABASE CONTAINER NAME> -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres --network <NETWORK NAME>
```
ex:
```sh
docker run --name postgresql -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres
```

demarrer le conteneur
```sh  
docker run -it --name < ODOO CONTAINER NAME> -p 8069:8069 --network <NETWORK NAME> <IMAGE NAME>
```
ex:
```sh
docker run -it --name odoo-tp-dev -p 8069:8069 --network my-network-odoo odoo-tp
```

**2ème méthode : docker-compose**
