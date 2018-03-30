# Mise en oeuvre de l'outil PIA-Docker

*Basé sur des scripts Docker de KillianKemps*
*Création d'un référentiel GitLab pour la stack pia-docker*
*Généreration d'une configuration Docker complète de la pile applicative PIA pour un environnement de POC*

## A propos de la stack Docker PIA
L'application (open source) a été développée pour une environnement de DEV à des fins d'évaluation et de démonstration de l'outil

* **La simplicité de l'installation de Docker accélèrera la diffusion de l'outil PIA pour évaluation**

*Pour une utilisation en mode production et dans un environnement multi-utilisateur avec un transactionnel important
certaines choses manquent encore:*

* Tout d'abord - PIA est utilisable tel quel dans aucune authencation autre que le compte LDAP
* Il n'y a pas de cryptage SSL / TLS des serveurs et de la communication entre eux
(mais la sécurité du périmètre est inutile sans une porte frontale existante et n'est pas nécessaire pour accéder à Pia via localhost sur une machine locale)

### Mise en oeuvre de la stack Docker PIA avec Docker compose

 **Customisation pour déploiement de Pia-Docker sur un environnement de POC**

*Docker version 1.13.1, build 092cba3Docker version 1.13.1, build 092cba3*
*docker-compose version 1.14.0, build c7bdf9e*

**Stack Docker sous forme de 3 micro-services :**
* pia-frontend
* pia-backend
* PostgresSQL database

### Customisation du fichier docker-compose.yml

 L'option **RESTART** uniquement si on n'arrête pas le démon Docker manuellment ou au reboot de la VM
 Le Docker Engine n'est pas installé sous forme de services sur l'architecture de POC

 L'option **LINKS** (deprecated en version 3) a été retirée, utilisation du réseau et des volumes nommés
 pour passer les variables d'environnemnt à la base de données PostgresSQL

 > Lancer l'application PIA sous le contexte docker-pia

 docker-compose up -d

 > Vérifier son bon fonctionnement

 docker-compose logs -f

 >Attaching to dockerpia_cnil-pia-back_1, dockerpia_cnil-pia-front_1, dockerpia_database_1
 >
 cnil-pia-back_1   |                                    List of databases  
 cnil-pia-back_1   | ----------------+----------+----------+------------+------------+-----------------------  
 cnil-pia-back_1   |  pia_production | postgres | UTF8     | en_US.utf8 | en_US.utf8 |  
 cnil-pia-back_1   |  postgres       | postgres | UTF8     | en_US.utf8 | en_US.utf8 |  
 cnil-pia-back_1   |  template0      | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +  
 cnil-pia-back_1   |                 |          |          |            |            | postgres=CTc/postgres  
 cnil-pia-back_1   |  template1      | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +  
 cnil-pia-back_1   |                 |          |          |            |            | postgres=CTc/postgres  
 cnil-pia-back_1   | (4 rows)

### Travaux de la communauté Docker pour PIA
 1. (Je suis déjà en train de travailler dessus et je vais créer une version de l'outil PIA incluant l'authentification basée sur le principe et l'omniauth dans le backend et le jeton Angular dans   le frontend. c'est déjà fait)
 2. Ajouter omniauth-ldap pour l'authentification la plus courante utilisée dans les environnements professionnels
 3. Fusionner cela dans la pile d'application PIA

#### Send to KillianKemps
>Hi Killiam,
I suggest to you a updated version of docker-compose.yml with 'Links' option deprecated in version 3 replaced by a volume named with persistant for Postgres datas
An option of restart is also implemented unless the demon Docker is stopped intentionnaly by the admin
JMA
