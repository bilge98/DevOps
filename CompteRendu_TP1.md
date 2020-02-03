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
  ```nano Dockerfile```

  Contenu du Dockerfile :
  ```
  FROM postgres:11.6-alpine
  ENV POSTGRES_DB=db \
  POSTGRES_USER=usr \
  POSTGRES_PASSWORD=pwd
  ```
  
  ```nano 01-CreateScheme.sql```

  Contenu du fichier de création des tables :
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
  
  ```nano 02-InsertData.sql```
  
  Contenu du fichier d'init des tables :
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
 

## Création du container API

## Création du container Apache
