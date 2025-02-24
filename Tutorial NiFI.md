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

## Prise en main

Démo

## Tuto 1: Ingestion de données

- Lab 1 : Déplacer un fichier d'une source vers une destination
  - Processors
    - GetFile
    - PutFile

- Lab 2 : Déplacer un fichier et modifier le nom du fichier
  - Processors
    - GetFile
    - PutFile
    - UpdateAttribute :  modifier le filename avec : 
      `` ${filename}_${now():toNumber():format("dd-MM-yy")}.json``


- Lab 3: Ingestion à partir d'une base de données
  - Processors:
    - ExecuteSQL
    - convertAvroToJSON or ConvertRecord
    - UpdateAtribute
    - PutFile

  - Controller Service1
    - DBCPConnectionPosol
    - AvroReader
    - JsonRecordSetWriter

- Lab 4: ConvertCSVToJSon (tuto detaillé)

- Lab 5: Nifi template/Nifi registery
  - Create template
  - Import template
  - Save Process Group Into Nifi registery
  - export template

Note : url http://localhost:18080/nifi-registry/  

Lab 5 : Nifi Monitoring

