# becausethisisourproject

**Projet Docker**

# Déploiement d'un erp (odoo) : 

**1ère méthode : docker en lignes de commandes**








**2ème méthode : docker-compose**

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
