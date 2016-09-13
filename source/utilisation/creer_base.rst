.. _creer_base:


########################
Créer la base de données
########################

Vous devez au préalable récupérer le framework.
Dans le repertoire www de votre serveur apache : ::

    svn checkout svn://scm.adullact.net/scmrepos/svn/openmairie/openmairie_exemple/trunk openExemple


Il vous est proposé de créer la base de données sous PostgreSQL :

- Créer les tables nécessaires au framework openMairie : ::

    cd data/pgsql
    sudo su postgres
    dropdb openexemple && createdb openexemple && psql openexemple -f install.sql
    

- Créer les tables nécessaires à notre exemple :


    - table courrier ::
    
        courrier        int 8       cle primaire
        dateenvoi       date
        objetcourrier   text
        emetteur        int8        cle secondaire
        service         int8        cle secondaire
    
    
    - table emetteur ::
    
        emetteur        int 8       cle primaire
        nom             varchar 20
        prenom          varchar 20

        
    - table service ::
    
        service         int 8        cle primaire
        libelle         varchar 20

    - La requête correspondante en PostgreSQL est la suivante : ::

        -- Création des séquences

        CREATE SEQUENCE openexemple.emetteur_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1;

        CREATE SEQUENCE openexemple.service_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1;

        CREATE SEQUENCE openexemple.courrier_seq
        START WITH 1
        INCREMENT BY 1
        NO MINVALUE
        NO MAXVALUE
        CACHE 1;

        -- Création des tables

        CREATE TABLE openexemple.emetteur (
            emetteur          int PRIMARY KEY,  -- clé primaire
            nom               varchar(20),
            prenom            varchar(20)
        );

        CREATE TABLE openexemple.service (
            service           int PRIMARY KEY,  -- clé primaire
            libelle           varchar(20)
        );

        CREATE TABLE openexemple.courrier (
            courrier          int PRIMARY KEY,           -- clé primaire
            dateenvoi         date,
            objetcourrier     text,
            emetteur          int REFERENCES openexemple.emetteur,   -- clé étrangère
            service           int REFERENCES openexemple.service     -- clé étrangère
        );

- Modifier le paramétrage openMairie pour faire un accès à la base créée :


    dyn/database.inc.php

    *voir framework/parametrage*


- Accéder avec votre navigateur sur openExemple :

    login : **admin**
    
    mot de passe : **admin**
