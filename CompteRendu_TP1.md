# DevOps
Module DevOps S8 - TP1 

Target application
3-tiers application:
1. Database 
2. Backend API
3. HTTP server


## Création du container database

* Création d'un sous-dossier postgresql
  ```
    mkdir postgresql
    cd postgresql
  ```

* Création d'un fichier Dockerfile et des deux fichiers d'initialisation pour la création et les insert dans les tables
  
  **Dockerfile :
  
  ```nano Dockerfile```
  
  contenu:
  ```
  FROM postgres:11.6-alpine
  ENV POSTGRES_DB=db \
  POSTGRES_USER=usr \
  POSTGRES_PASSWORD=pwd
  ```
  
  **Fichier de création des tables :
  
  ```nano 01-CreateScheme.sql```

  contenu:
  ```
  CREATE TABLE public.departments
  (
  id SERIAL PRIMARY KEY,
  name VARCHAR(20) NOT NULL
  );
  CREATE TABLE public.students
  (
  id SERIAL PRIMARY KEY,
  department_id INT NOT NULL REFERENCES departments (id),
  first_name VARCHAR(20) NOT NULL,
  last_name VARCHAR(20) NOT NULL
  );
  ```
  
  **Fichier d'init des tables :
  
  ```nano 02-InsertData.sql```
  
  contenu:
  ```
  INSERT INTO departments (name) VALUES ('IRC');
  INSERT INTO departments (name) VALUES ('ETI');
  INSERT INTO departments (name) VALUES ('CGP');
  INSERT INTO students (department_id, first_name, last_name) VALUES (1,
  'Eli', 'Copter');
  INSERT INTO students (department_id, first_name, last_name) VALUES (2,
  'Emma', 'Carena');
  INSERT INTO students (department_id, first_name, last_name) VALUES (2,
  'Jack', 'Uzzi');
  INSERT INTO students (department_id, first_name, last_name) VALUES (3,
  'Aude', 'Javel');
  ```
 
* Création de l'image

  ```docker build -t bilge98/postgretp1```
  

* Création du container sur le port 5433 (le 5432 est déjà pris par le serveur apache de ma machine)
  L'option -p permet de préciser les ports extérieurs/hôte et intérieurs au container. Lorsqu'ils sont identiques il n'est pas nécessaire de les préciser mais on peut le faire tout de même.
  On donne au container le nom 'postgres' avec l'option --name. Si aucun nom n'est donné, docker donnera tout de même un nom aléatoire.

  ```docker run -p 5433:5432 --name postgres bilge98/postgretp1```
  

* Mise en place d'adminer pour manager la DB 
  L'option --link lie notre container postgres au container adminer créé.
  ```docker run --link postgres:db -p 8080:8080 adminer```
  

## Création du container API

## Création du container Apache
