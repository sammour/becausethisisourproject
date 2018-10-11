# becausethisisourproject

**Projet Docker**

# Déploiement d'un erp (odoo) : 

**1ère méthode : docker en lignes de commandes**

__cloner le repository__

__construire l'image__
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

__récupérer l'image docker hub et créer le conteneur__

```sh
docker run --name <DATABASE CONTAINER NAME> -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres --network <NETWORK NAME>
```
ex:
```sh
docker run --name postgresql -e POSTGRES_PASSWORD=odoo -e POSTGRES_USER=odoo -e POSTGRES_DB=postgres -d postgres
```

__demarrer le conteneur__
```sh  
docker run -it --name < ODOO CONTAINER NAME> -p 8069:8069 --network <NETWORK NAME> <IMAGE NAME>
```
ex:
```sh
docker run -it --name odoo-dev -p 8069:8069 --network my-network odoo
```

__Pour lancer votre application__
```sh
docker start <ODOO CONTAINER NAME>
```
ex:
```sh
docker start odoo-tp-dev
```


**2ème méthode : docker-compose**
