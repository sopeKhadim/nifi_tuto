# Tutorial Nifi

## Installation

- A partir des fichier binaires

  Il faut télécharger à partir de l'url [NiFI](https://nifi.apache.org/download/)

  Demarrer Nifi avec la commande ci dessous :

  ```shell
  cd nifi
  . bin/nifi.sh start

  ```

  Se connecter sur https://localhost:8443

  Arreter Nifi avec la commande :

  ```shell
  cd nifi
  . bin/nifi.sh stop

  ```
- Avec docker run

```shell

docker run --name nifi \
  -p 8443:8443 \
  -d \
  apache/nifi:latest

## Recupérer le mot de passe générer
docker logs nifi | grep Generated

docker run --name nifi \
  -p 9443:9443 \
  -d \
  -e NIFI_WEB_HTTPS_PORT='9443' \
  apache/nifi:latest

docker run --name nifi \
  -p 8443:8443 \
  -d \
  -e SINGLE_USER_CREDENTIALS_USERNAME=nifiuser \
  -e SINGLE_USER_CREDENTIALS_PASSWORD=pa$$w0rd \
  apache/nifi:latest

```

```shell
#start
docker-compose up -d

#
docker-compose down
```

## Prise en main

Démo

## Labs

- **Lab 1** : Déplacement de fichiers à partir d'une source vers une destination.  
Il faut créer une liste de fichiers dans un repertoire source input et un repertoire output de destination. Configurer les processeurs correspondant.

  - Processors
    - GetFile
    - PutFile

- **Lab 2** : Modification d'attribut.
Ajouter le processor UpdateAttribute pour modifier le filename avec : ``${filename}_${now():toNumber():format("dd-MM-yy")}.txt``  

  - Processors:
    - GetFile
    - PutFile
    - UpdateAttribute
  
- **Lab 3** : Ingestion à partir d'une base de données.
Il faut suivre les étapes suivantes:
  - Créer une base de données **retail_db** à partir du fichier ***create_db.sql***.  
  - Créer un controller service DBCPConnectionPool en configurant les parametres de connexion de la base de donnée.
  - Utiliser le processor ExecuteSQL pour creer la requete d'extraction suivante : ``SELECT * FROM customers``
  - Convertir le format Avro vers Json  
  - Charger les données vers un repertoire output  

  - Processors:
    - ExecuteSQL
    - convertAvroToJSON or ConvertRecord
    - UpdateAtribute
    - PutFile

  - Controller Service:
    - DBCPConnectionPool (com.jdbc.mysql.Driver)
    - AvroReader
    - JsonRecordSetWriter

- **Lab 4**: Nifi template (ConvertCSVToJSon)
  - Importer le template ConvertCSVToJSon.json
  - Analyser et executer le Processes group

  - Modification à faire : 
    - Copier le contenu ci-dessous dans Processor ReplaceText  :

    ***ReplaceText***

    | Name                  | Value                                      |
    | --------------------- | -------------------------------------------|
    | Replacement Strategy  | `Changer la strategie par Always Replace`  |
    | Replacement Value     | `Copier les données CSV`                   |

    ```csv
    "id","ident","type","name","latitude_deg","longitude_deg","elevation_ft","continent","iso_country","iso_region","municipality","scheduled_service","gps_code","iata_code","local_code","home_link","wikipedia_link","keywords"
    6523,"00A","heliport","Total Rf Heliport",40.07080078125,-74.93360137939453,11,"NA","US","US-PA","Bensalem","no","00A",,"00A",,,
    323361,"00AA","small_airport","Aero B Ranch Airport",38.704022,-101.473911,3435,"NA","US","US-KS","Leoti","no","00AA",,"00AA",,,
    6524,"00AK","small_airport","Lowell Field",59.947733,-151.692524,450,"NA","US","US-AK","Anchor Point","no","00AK",,"00AK",,,
    6525,"00AL","small_airport","Epps Airpark",34.86479949951172,-86.77030181884766,820,"NA","US","US-AL","Harvest","no","00AL",,"00AL",,,
    6526,"00AR","closed","Newport Hospital & Clinic Heliport",35.6087,-91.254898,237,"NA","US","US-AR","Newport","no",,,,,,"00AR"
    322127,"00AS","small_airport","Fulton Airport",34.9428028,-97.8180194,1100,"NA","US","US-OK","Alex","no","00AS",,"00AS",,,
    ```

    - Remplacer les processors (ExtractText et ReplaceText) qui convertissent les données en format CSV avec le processor ConvertRecord.

    ***ConvertRecord***

    | Name          | Value                                                |
    | ------------- | ---------------------------------------------------- |
    | Record Reader | `Créer un Controller Service CSVReader`              |
    | Record Writer | `Créer un Controller Service JsonRecordWriter`       |


- **Lab 5**: Nifi registery
  Nifi registery permet de sauvegarder et de version les Process groupes et templates NiFi. Le docker file permet de lancer le Nifi registery. Vous pouvez vous connecter sur l'url :  [NIFI-REGISTERY](http://localhost:18080/nifi-registry/)

  Suivre le lien suivant [registery-docs](https://nifi.apache.org/docs/nifi-registry-docs/) pour :

  - Créer un bucket 
  - Synchronizer le registry dans NiFI Web UI
  - Sauvegarder un Process Group dans un bucket
  - Commit les modifications dans un bucket
  - Exporter template avec le registery
  - Importer un template à partir d'un bucket

Note : 

Lab 5 : Nifi Monitoring
A venir ...
